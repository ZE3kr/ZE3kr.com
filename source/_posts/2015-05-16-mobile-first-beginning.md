---
title: Mobile First - Origins
tags:
  - HTML5
  - Responsive design
id: '1000'
categories:
  - Development
date: 2015-05-16 13:36:43
languages:
  zh-CN: https://guozeyu.com/2015/05/mobile-first-beginning/
---

Most of the existing desktop operating systems have browsers, which allow us to browse a web page conveniently. However, the original mobile browsers were difficult to access the web, because their low resolution, small screen and browser did not support many cutting-edge CSS and JavaScript, making them incapable of accessing the desktop version of the website. At that time, the websites accessed by mobile phones were greatly simplified versions specially designed for the device, like this:

![A simplified version of the mobile webpage](https://cdn.ze3kr.com/6T-behmofKYLsxlrK0l_MQ/0a8a6649-0972-4b84-9b97-1b67c618aa00/large)

That all changed a lot, though, until the release of the iPhone. Because the browser on iPhone - Safari is a real and very cutting edge web browser <!-- more --> and it also supports HTML5 standard. It comes with almost the same browser as a computer - so much so that it supports CSS3 and JavaScript. But all this is displayed on a device that is only 320 pixels wide. However when a desktop browser is reduced to 320 pixels in width, it becomes very different, when he visits a web page that is not optimized for it, it will scale it (the width will be scaled to 1/3), Take Baidu News as an example, if there is no adaptation, the computer version of the website will be displayed, so that the words on the page will be small, but the user can solve it by zooming, so there is no problem. However, Baidu News made a version specifically for the iPhone, like this:

![Baidu News for iPhone](https://cdn.ze3kr.com/6T-behmofKYLsxlrK0l_MQ/51bb36ab-cfb8-46cb-da81-505b7739ad00/large)

It adapts better to touchscreens and doesn't have the problem of small fonts or scaling. But please note that this page is no longer the page just now, which means that Baidu needs to redesign a webpage for the iPhone version, which is very costly. However, with the help of CSS, there is no need to prepare two web pages to adapt to both computer and mobile phones. They load the same page, but the applied style sheets are different. For example, without a mobile-optimized [Wikipedia navigation page](https://www.wikipedia.org/), Safari on the iPhone will zoom in and out, and the layout will be the same as when visiting on a computer:

![No mobile-optimized Wikipedia navigation page](https://cdn.ze3kr.com/6T-behmofKYLsxlrK0l_MQ/64ab7b14-6fb5-4420-59cb-0fa193c77800/large)

However, with the addition of CSS style sheets (and of course the `viewport` in the `head`), the page will be displayed perfectly on the mobile phone, like the image below:

![Mobile-optimized Wikipedia navigation page](https://cdn.ze3kr.com/6T-behmofKYLsxlrK0l_MQ/dd08ee3e-f91c-4f17-e109-065af7469200/large)

A web page like the Wikipedia navigation page that is designed according to the computer layout, and then adds CSS to make it mobile-friendly, is desktop-first rather than mobile-first. Mobile first is just the opposite. The page is designed according to the mobile phone, and then it is adapted to the computer. The computer and the mobile phone also access the same page, which is mobile first. Since mobile-first websites are designed based on mobile phones, and since the release of the iPhone, many smartphones have also used cutting-edge browsers, so mobile-first websites will naturally use many new features in HTML5 and CSS3, greatly shortening the time required for a web page. development time. Moreover, mobile-first websites are designed for the high-latency network of mobile phones, reducing page size, and loading speed is greatly improved no matter what network environment you are in. The advantages of mobile-first are numerous, which is what led to its popularity.
