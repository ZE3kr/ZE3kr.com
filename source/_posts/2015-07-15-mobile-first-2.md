---
title: Mobile First - Speed
tags:
  - HTML5
  - Responsive design
id: '34'
categories:
  - Development
date: 2015-07-15 14:00:25
languages:
  zh-CN: https://guozeyu.com/2015/07/mobile-first-2/
---

Internet speeds on mobile devices are usually not fast, they usually have slower speeds and higher latency than broadband, so how do mobile browsers load pages? That's what this article is about. In fact, whether you are loading a page on a mobile or desktop side, the following steps are required:

### 1\. Resolve Domain Name

The domain name usually corresponds to an IP address, and the browser can communicate with the server only after knowing <!-- more --> this IP address. In order to resolve the domain name, the client will send a request for the domain name to the DNS resolution server, and the IP address of the domain name can be queried. DNS resolution has a "TTL time to live", which is the cache time. This content will be cached on the DNS resolution server, router and client, and stored according to the cache time. The DNS resolution server is usually provided by the operator, and if there is a cache, the resolution speed is quite fast. (The current DNS resolution is transmitted in very insecure plaintext, and any middleman can destroy or modify the resolution result)

### 2\. Make a Request

After having the IP address, the browser establishes a TCP connection with the server and makes a request. The request will include the domain name of the website that needs to be accessed (so that IP can provide different content to multiple domain names), request method, URL path, acceptable data, compression type and encoding method, cookies and browser information (which can determine your whether the website is accessed on a mobile phone).

### 3\. Download page

The browser starts to download the page itself, and after downloading the page itself, it starts to download other content that the page depends on, including CSS, JavaScript, images, etc. Depending on the priority, they may be downloaded before or after rendering.

### 4\. Rendering the page

The browser will wait for the most important dependencies (that is, CSS and JavaScript in the `head` section, which will be highlighted later) to be loaded, before rendering. Because rendering depends on these contents, CSS is also what we often call style sheets. These style sheets are very important, such as this website of Baidu:

<img src="https://cdn.yangxi.tech/6T-behmofKYLsxlrK0l_MQ/09022b05-0350-4e22-f4cd-bc6559b30401/extra" alt="Baidu without stylesheets" width="2518" height="1410"/>

Without CSS style sheets, the layout of the entire web page is "out of control" and the page doesn't load in the expected way, which is a terrible experience. A web page without a style sheet and a web page with a style sheet are usually two completely different things. However, JavaScript is usually not related to layout and can be loaded later without affecting page styles.

## Boost Speed

Every content in the web page has to go through such a process in order to be obtained by the client. Only when the page is rendered can the user see the content of the page. Before that, the page is blank. Be aware that if a page is blank for 3 seconds, more than half of users will log out. Mobile-first considers the user experience on high-latency mobile networks, but when it comes to desktop, you can also enjoy the speed boost brought by limited mobile. In order to increase the speed, there are several common methods:

### Put Unnecessary JavaScript at the End of the Page

If JavaScript is placed at the end, it can be loaded after the page is rendered, greatly reducing the time when the page is blank. This is the simplest and the most obvious improvement.

### Using CDN

Using CDN means putting static files on the CDN server as much as possible. A CDN contains many nodes. When users download files on CDN, they will be downloaded from the node with the nearest physical location or the same operator. If there is a cache, it will be directly passed to the user. If there is no cache, it will be downloaded from the nearest data center and cached, and then passed to the user. My approach is to put all the images, videos, CSS stylesheets and JavaScript on the site on the CDN, the site itself is not on the CDN. Doing so can greatly reduce load times at a small cost. Whether it is a personal blog or an industry website, it is necessary to use a CDN. If you don’t have the money to buy your own CDN, it’s okay, you can use the CDN of common open source CSS and JavaScript for free. The official website address is: [cdnjs.com](https://cdnjs.com/).

### Proper Use of Caching

Cache images, CSS style sheets and JavaScript settings. I'm used to setting a fairly long time period (a week, a year, etc.), and some people adjust the cache period to a smaller value in order to be able to modify it frequently. I change the file directory directly when I modify the file, so that the file can always be kept up-to-date. Taking advantage of the cache can greatly speed up the user's second visit.

## Summarize

For website speed, we should not only focus on the first startup speed, but also use the cache to improve the next visit speed. And you should pay attention to user experience while improving speed.
