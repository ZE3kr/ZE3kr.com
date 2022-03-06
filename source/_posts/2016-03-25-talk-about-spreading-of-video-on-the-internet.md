---
title: Talk about the streaming of video on the Internet
tags:
  - HTML5
id: '1433'
categories:
  - - Technology
date: 2016-03-25 23:15:47
languages:
  zh-CN: https://guozeyu.com/2016/03/talk-about-spreading-of-video-on-the-internet/
---

Videos are distributed on the Internet, the most common way is through the **World Wide Web** - the user directly enters a web link/search engine search/link to other websites, or through any reader (including Podcasts client in inside) to play. Doing this is not limited to any platform at all, but it means that you have to pay for your video traffic, but let's leave that out (this is just the ideal solution). First, let's start with the format of the video. In order for the user to play the video, you need to use a format that the user can decode. You might want a free format that can be played on all platforms, but there is no such format.
<!-- more -->

The H.264 + AAC format encapsulated in MP4 is the most compatible format at present, and can be played on most devices, but it is not an open source format, and can be used on some clients or servers that only use open source software. This format cannot be used. A typical example is that Wikimedia Commons, the media resource site behind Wikipedia, does not use the MP4 format, resulting in videos on Wikipedia that cannot be played at all on some browsers. For most cases, however, using the MP4-encapsulated H.264 + AAC format is sufficient. Secondly, the video needs to be displayed, which is not difficult in the 21st century. If you want to display on the web, the good news is: all major browsers follow a unified specification (HTML5), as long as you also use this specification, then you can embed video on mainstream browsers (Although they follow the same set of specifications, the supported encodings are different. It is like saying that the user's mailboxes are all unified specifications now, and you can put the books you want to send into these mailboxes. But your books are in Chinese written, so people who do not understand Chinese will not be able to understand the content of the book.).

A version of the same video that is streamed on the Internet is of a different quality than it is played locally. In order to make the video play smoothly on the Internet, the quality of the video will be reduced. Many video websites will also prepare multiple versions of a video with different picture quality, so that the better picture quality version can be played in different network environments.

In fact, the purpose of writing this article is to talk about the gap between Chinese video websites and foreign countries. Some are unavoidable factors, but some are what these sites should have done, but didn't. The screen resolution of foreign YouTube videos can reach 7680×4320@60FPS, and it is not dare to call it “ultra-clear”. However, in China, the resolution of 1280×720@30FPS or even lower is generally called “ultra-clear”. This is nearly 100 times lower than the former (in terms of the amount of information in pixels). Moreover, the picture quality under the same resolution is also very different, the lower the picture quality, the more money you can save (many video websites in mainland China are very rich, but why save money? And know~).

Higher video resolution means better results on higher resolution monitors. There are already many 4K and 8K TVs and monitors, and more and more cameras/phones can record 4K and 8K video. These high-quality content are moving towards the low-end consumer market, so video websites still need to support them.

But what I said before, I don't expect domestic video sites to do it, because they still want to make more money. But why use a bug that is full of bugs, and the official has stopped maintenance, you need to install additional plug-ins to play videos? I'm talking about Flash (of course, there are a few websites that don't use it anymore), just playing a video, using HTML5 is enough, there is no need for such a complicated thing. If you want them to not use Flash, you must first make Flash completely disappear from the client (it would be better if browsers can actively block Flash, such as Safari), and gradually, they will find a large number of users churn (this thing is happened), and then one day it will finally be possible to use the new technology. In foreign countries, it is much ahead of China. Since many years ago, mainstream video sites have stopped using Flash by default.

There is actually a lot more to be said about the streaming of video on the Internet, and this article is just a rough talk.
