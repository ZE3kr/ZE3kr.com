---
title: The pros and cons of static web pages
tags:
  - Website
id: '1008'
categories:
  - - Development
date: 2016-01-03 14:00:00
languages:
  zh-CN: https://guozeyu.com/2016/01/static-website/
---

Static web pages, that is, **pure HTML web pages**, each page in the blog is a `.html` file. First of all, there is a misunderstanding here. Some people think that static web pages cannot be easily updated. In fact, static web pages can be easily updated. With the help of static web page generators, updating them is not complicated. When it needs to update an article, it needs to regenerate the home page and the article, which is usually done in less than a minute. What if the blog were to use dynamic web pages? It is certainly possible to do so, and there are many mature software, such as WordPress, that run in a PHP environment and require a MySQL database. Every time a web page is accessed, the server needs to read the content of the database (or read from the cache), and then process it into HTML with a certain style and return it to the user. Of course, dynamic web pages can realize all the functions of static web pages, and of course there are more functions, such as image uploading and regular publishing. Since dynamic web pages can realize all the functions of static web pages, what are the advantages of static web pages?
<!-- more -->

## Advantage 1: Save Server Overhead, not Afraid of Attacks

### Not Afraid of DDOS Attack

After a lot of DDOS attack tests, I found that static web pages are much harder to attack than dynamic web pages, even one to several orders of magnitude harder. Every time you visit a dynamic web page, you need to parse PHP, read the database or cache, and require much more computation than static web pages. Therefore, the DDOS attack is basically ineffective, and the speed is not even slow.

### Hard to be Hacked

A static web page does not need a database at all, it is just a file itself, so there is no problem such as database injection at all. There are even fewer bugs.

### Simple CDN on the Web

CDN on static web pages is extremely simple, and site-wide caching is enough. Every page can be cached directly, and all caches are cleared when the page is updated. After you have a CDN, you are not afraid of DDOS. It is not a problem to attack the other party or even GB, and people just need to pay a few cents for the traffic fee.

## Advantage 2: Simple Server Deployment

A static web page doesn't need a database at all, it just needs a server that can upload files and link externally, such as Amazon S3 can put it. Most servers can deploy it.

## Advantage 3: Simple Maintenance

Basically, as long as you know HTML, CSS and JavaScript, you will write static web pages, and then use some static web page generators to generate all the pages directly.

## Problem 1: Update Real-Time

If a static page generator is used, there must be a delay, and the amount of delay depends on the speed at which the page is generated. There are two generation methods to choose from, one is to generate locally, the other is to generate on the server.

## Problem 2: Comments and Statistics

These functions can generally only be implemented in dynamic web pages, because they all require a database. However, static web pages can choose third-party services. For example, you can use [Disqus](https://disqus.com) for comments, [Google Analytics](https://www.google.com/analytics/) for statistics, or You can build your own dynamic server to do these things and install Piwik.

## Summarize

There is no problem with blogging with static web pages, and it is also recommended by me (slap in the face, I have actually switched to WordPress now). It's convenient and simple, and there's nothing wrong with it.
