---
title: "From New Year's tunes to tech tools: My journey to a better playlist."
date: 2024-01-24T18:07:29+01:00
# weight: 
# aliases: []
tags: ["javascript", "chatgpt", "music"]
# author: "Me"
# author: ["Author1", "Author2"] # for multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: true
description: ""
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

Every year, I jokingly set New Year's resolutions based on whatever is on my mind at the time. Sometimes these resolutions morph into serious life changes, and other times, I abandon them immediately. This year, one of my resolutions was to listen to more music, assuming it would make me happier and more prosperous moving forward. As I pondered how to measure my success at this---I began exploring available solutions. Technically, there's a dominant choice among music enthusiasts: [Last.fm](https://www.last.fm). However, my initial experience on Last.fm was marred by ads and prompts to subscribe, which was less than ideal.

In search of an alternative, I turned to GitHub and discovered a project that seemed to be the perfect solution. Given the variety of services I use for music (like Roon, Apple Music, Qobuz), I found that I'd still need to use Last.fm in the short term to log my listens. However, I came across a fantastic project called [Multi-Scrobbler](https://github.com/FoxxMD/multi-scrobbler), which acts as a proxy to scrobble music from one service to another. This discovery allowed me to maintain my current setup while utilizing a more open platform, ListenBrainz, free from ads. [ListenBrainz](https://listenbrainz.org/) offers the unique feature of creating playlists based on listening habits but outputs them in the somewhat obscure JSPF format, which none of my services support, necessitating a conversion solution.

This need led to the inception of my project, Jspfconvert—a perfect opportunity to test whether ChatGPT could develop a functional solution. I envisioned a simple, single-page application (SPA) for this task. 

{{< img "webapp_jspf_to_m3u_request.png" "Screenshot of a chat message requesting the creation of a simple web application for converting jspf files to m3u format through a drag-and-drop interface." >}}

Initially, ChatGPT suggested a Node.js backend with a modern frontend framework, but I specified the need for a straightforward SPA, and off we went.

{{< img "spa_conversion_tool_development_guide.png" "Screenshot of a response from ChatGPT detailing the creation of a Single Page Application (SPA) for converting JSPF files to M3U format. It outlines the use of HTML, CSS, and JavaScript, and suggests the possible inclusion of a framework or library like React, Angular, or Vue.js. The response includes two sections: 1. Front-End Development with technologies and steps for creating the user interface, and 2. Drag-and-Drop and File Input Handling, with instructions for implementing drag-and-drop functionality for file input." >}}

Once m3u was done I learned [Soundiiz.com](https://soundiiz.com/) also accepts XSFP so I quickly added this and here we are:

{{< img "playlist_converter_ui_design.png" "A user interface design for a web application featuring a drag-and-drop upload area. The interface includes a prominent button labeled 'Convert to XSFP' and instructional text below that reads 'Drop JSPF file here or click to upload. Convert your playlist to M3U or XSPF format." >}}

Take a look at the app over at https://jspfconvert.pages.dev/. And if you're curious about the code behind it, feel free to dive into the GitHub repo at [diblasio/jspfconvert](https://github.com/diblasio/jspfconvert). 

Cheers!

