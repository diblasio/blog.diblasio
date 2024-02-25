---
title: "Gitea Actions for Docker Builds"
date: 2024-02-25T16:35:38+01:00
# weight: 
# aliases: []
tags: ["Gitea", "Docker"]
# author: "Me"
# author: ["Author1", "Author2"] # for multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: true
#description: "Your default description."
canonicalURL: ""
disableHLJS: false # to disable highlightjs
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: false
UseHugoToc: false
cover:
    image: "" # image path/url
    alt: "" # alt text
    caption: "" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
---

## Introduction 

I've been using Gitea to manage my local code projects for quite a while now, and recently new functionality was added that provides a nice simple to use builder system which is very similar to Github Actions. You can read all about this feature [here](https://blog.gitea.com/feature-preview-gitea-actions). Now if we combine this with Gitea's [packages functionality](https://docs.gitea.com/usage/packages/overview), we end up with a very lightweight system to handle our custom docker containers end to end. 


## Sample Workflow

It took me a bit to sort out how to do dual arch builds so I figured I'd share an annotated sample in case anyone else is trying to do the same thing. 

**docker-publish.yml**
```bash
name: Docker Build  # Name of the Gitea Actions workflow

on:  # Specifies when this workflow will run
  schedule:  # Triggers the workflow on a schedule
    - cron: '37 1 * * 6'  # Runs at 01:37 AM UTC every Saturday (Cron syntax)
  push:  # Triggers the workflow on a push event
    branches:  # Specifies which branches will trigger the workflow
      - main  # Triggers only for pushes to the main branch

jobs:  
  build:  # Name of the job
    runs-on: ubuntu-latest  # Specifies the type of virtual host machine to run the job on
    container:  # Specifies the container to run the job inside
      image: catthehacker/ubuntu:act-latest  # The Docker image to use as the container
    strategy:  # Strategy for the job execution
      matrix:  # Allows running jobs with different variations
        arch: [amd64, arm64]  # Variations of architecture to run the job with

    steps:  # Steps to be executed in the job
    - name: Login to Docker Hub  # Step to log in to Docker Hub
      uses: docker/login-action@v2.2.0  # Uses an action to perform the login
      with:  # Inputs to the login-action
        registry: {{ registry_url }} # Specifies the registry to log in to. You can set a variable or just hardcode this value.
        username: ${{ secrets.DOCKER_USERNAME }}  # Docker Hub username from secrets
        password: ${{ secrets.DOCKER_TOKEN }}  # Docker Hub token/password from secrets

    - name: Checkout repository  # Step to check out the repository code
      uses: actions/checkout@v3.6.0  # Uses the checkout action

    - name: Set up Docker Buildx  # Step to set up Docker Buildx
      uses: docker/setup-buildx-action@v2.10.0  # Uses an action to set up Buildx

    - name: Get base image digest  # Step to get the digest of the base image
      id: base-image-digest  # Sets an ID for this step to refer to its outputs
      run: echo "::set-output name=digest::$(docker pull ubuntu:20.04 | grep -Eo 'sha256:[a-f0-9]+' | cut -d ':' -f 2)"  # Pulls base image and extracts digest

    - name: Get current date and time  # Step to get the current date and time
      id: current-date-time  # Sets an ID for this step to refer to its outputs
      run: echo "::set-output name=rfc3339::$(date -u +'%Y-%m-%dT%H:%M:%SZ')"  # Gets current date/time in RFC 3339 format

    - name: Build Docker image  # Step to build and push the Docker image
      uses: docker/build-push-action@v4.2.1  # Uses an action to build and push Docker images
      with:  # Inputs to the build-push-action
        context: .  # Specifies the build context
        push: true  # Enables pushing the built image to a registry
        tags: {{ registry_url }}roonserver:latest  # You can set a variable or just hardcode the tag depending on your needs.
        platforms: linux/amd64,linux/arm64  # Specifies the target platforms for the image
        build-args: |  # Specifies build arguments
          TARGETARCH=${{ matrix.arch }}  # Sets the TARGETARCH build argument
          BASE_IMAGE_DIGEST=${{ steps.base-image-digest.outputs.digest }}  # Sets the BASE_IMAGE_DIGEST build argument
          IMAGE_CREATED=${{ steps.current-date-time.outputs.rfc3339 }}  # Sets the IMAGE_CREATED build argument

```

## 🎉

Here's what we'll see once everything is done:
{{< img "gitea_roon.png" "" >}}

Hope this helps someone out there 😄.
