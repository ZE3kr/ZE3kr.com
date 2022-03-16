---
title: A Few WordPress Optimization Suggestions
tags:
  - HTML5
  - WordPress
  - Responsive Design
  - Website
  - Internet
id: '1709'
categories:
  - - Development
date: 2016-06-09 17:36:00
languages:
  zh-CN: https://guozeyu.com/2016/06/wordpress-optimize/
---

WordPress is the most popular content management system out there, and this site uses it. But for a fresh installation of WordPress, its performance is not very high, and when the traffic to the website suddenly increases, it is very important to optimize the performance. By implementing the following solutions, you can greatly improve the speed of WordPress access:
<!-- more -->

## Configure Cache

WordPress is a dynamic system. If the cache is not configured, the server needs to read the database and generate page content for each request. For hosts with different performance, this may take 20ms~1000ms or even slower. If the cache can be configured correctly, it can significantly speed up and reduce the computing resources of the host.

### Configure Page Cache

Using a plugin to configure caching is the easiest way. Here I recommend [WP Super Cache](https://wordpress.org/plugins/wp-super-cache/), which is a caching plugin produced by WordPress.com. As far as page caching is concerned, it has very comprehensive functions and supports many Various caching modes, including `mod_rewrite`, if you use Nginx, then [you can use my configuration file](https://gist.github.com/ZE3kr/3c28029ffa4c91392045e9a579599646), you can skip PHP and serve static files directly , the corresponding speed of the page has been significantly improved. Also, it is necessary to return the correct Cache-Control for browsers, especially CSS and JS files.

### Configure Object Cache

Object caching is more flexible and widely used than page caching, but certainly not as fast as page caching. The APCu caching system is recommended here. The installation method on Ubuntu/Debian is as follows:

```
$ apt install php-apcu
```

Then restart the Web server, [Install APCu Object Cache Backend](https://wordpress.org/plugins/apcu/installation/) . Redis, Memcached, etc. are all caches of this type, but APCu is the easiest to configure and fast.

### Build a Distributed Cache System

For example, my website uses North America East Coast (main) and Asia VPS, the main server is configured with Nginx, PHP and MySQL; the Asian server is only configured with Nginx. Configure the cache on these servers, and use lsyncd to synchronize the cached content. Nginx checks the cache every time you visit, and only proxy if there is no cache, which can greatly reduce the delay of the first page.

## Using CDN

### Use sitewide CDN

By using the site-wide CDN, you can avoid the problem of configuring cache on your own server, you can also add HTTPS, HTTP/2 and other functions to the server, and at the same time, it can filter illegal traffic and defend against DDOS (provided that your IP is not exposed, Or you set up a whitelist). In addition, the use of CDN can significantly improve the loading speed of the website, benefiting visitors.

#### Using CloudFlare

CloudFlare is free to use, you need to change the DNS service provider before using CloudFlare, and then you can use it without additional configuration. But it only caches CSS, JS and multimedia files, not HTML pages, that is to say, users will still return to the original server every time they visit, the speed of the page itself will not be significantly improved, and configuring caching on the original server is also necessary.

#### Using KeyCDN with Plugins

[KeyCDN](https://app.keycdn.com/signup?a=7126) is a pay-per-use CDN provider, using the plugin [Full Site Cache for KeyCDN](https://wordpress. org/plugins/full-site-cache-kc/) can be easily configured, this plugin will automatically refresh the cache. Compared with CloudFlare, KeyCDN can cache HTML pages, which greatly reduces the pressure on the origin server. At the same time, when refreshing the cache, it can be refreshed by tag to avoid unnecessary refreshes. When the website has a large number of visits, the use of KeyCDN can significantly improve the speed, and the cache hit rate will also be greatly improved.

### Only Use CDN for Resources

You can configure another domain name, use a CDN on that domain name, and then implement a partial CDN by rewriting the page address through a plugin. The WP Super Cache mentioned above can configure CDN, or use [CDN Enabler](https://wordpress.org/plugins/cdn-enabler/) to implement some CDN functions. As for the choice of CDN, it doesn't matter, as long as it supports [Origin Pull](http://knowledgelayer.softlayer.com/questions/365/How+does+Origin+Pull+work%3F) (that is, requesting back-to-origin).

## Server Performance

The easiest way to improve the performance of the server itself is to use more memory and more core CPUs to speed up significantly. In addition to this, it is also important to improve the performance of the application on the server:

### Script Performance

The processing speed of PHP scripts is a major bottleneck of WordPress. With the latest version of PHP, higher performance can be obtained, for example [7.0 is 3 times faster than 5.6](https://www.zend.com/en/resources /php7_infographic).

Second, using fewer plugins reduces the amount of scripts PHP needs to execute, because WordPress loads all plugins on every page load. A small number of plugins (under 10) have little impact on WordPress speed, and of course it depends on the plugin itself.

### Database Performance

The database is one of the bottlenecks of WordPress performance, and optimizing on the database can improve the speed to a certain extent. In general, if you use WordPress correctly, you don't need to clean up the database. However, there may be some plugins that may create too many useless tables in the database, and the response speed of the server will be greatly reduced (about 1 to 3 times). It is recommended to use [WP-Optimize](https:// wordpress.org/plugins/wp-optimize/) to clean up. It is not too many articles, but it will not affect the loading speed (the speed below 10,000 articles is actually acceptable, but why do you write so many articles? Quality is more important than quantity).

## Image Optimization

Images occupy a large part of the size of the web page and are also related to the user experience.

### Picture Compression

Compressing images can not only improve the image loading speed during access, but also reduce server bandwidth. It is recommended to use the free [EWWW Image Optimizer] (https://wordpress.org/plugins/ewww-image-optimizer/), which can be used on the server. pictures are processed. If your server performance is limited, you can use [Optimus](https://optimus.io/en/) to do it online, the free version has very limited features.

### Responsive Images

Loading different images for different devices, such as images loaded on mobile phones, can be much smaller than images loaded on computers with retina screens. Using the [srcset technology mentioned on this site] (https://guozeyu.com/2015/08/using-srcset/) can be the easiest to implement this function, as long as your theme supports it (the latest official The default theme already supports it), if the theme itself does not support it, it can also be implemented through plugins.

## Disable Unwanted Services

WordPress has some built-in services that you probably don't need, and disabling them can reduce the amount of resources a page needs to load and what the server needs to do.

### Emoji Support

WordPress supports Emoji but will therefore introduce extra CSS into the page, [use this script](https://gist.github.com/MaruscaGabriel/fc7c069860406c77304a) to disable it (if you don't need it).

### Google Fonts

Google Font is very slow to load in China, and the page will remain blank until the loading is complete. You can specifically install the Disable Google Fonts plugin, or disable it in the settings in the Autoptimize plugin mentioned below.

### Pingback

Go to Settings => Discussions to disable the "Try to notify blogs linked in articles" and "Allow other blogs to send link notifications (pingbacks and trackbacks) to new articles" features (if you don't need them). This feature does not affect page load time.

## Reduce the Number of Requests and Page Size

Reducing the number of requests can also improve loading speed in some cases, and reducing page size can shorten the time it takes to download a page. [Autoptimize](https://wordpress.org/plugins/autoptimize/) is recommended, it can minify CSS, JS and HTML, and combine CSS and JS to reduce the number of requests.

However, if your blog has the HTTP/2 protocol enabled, there is no need to reduce the number of requests, but in order to enable HTTP/2, HTTPS must be used, so it is hard to say whether it will be faster or slower in the end. However, the use of HTTPS is still strongly recommended. For security, what is the sacrifice of speed.

## Summarize

Doing the above points can effectively speed up the speed. My website has achieved the above points. The timeline of this website is as follows under the condition of no-cache Wi-Fi in China:

![Load including images in under 1 second](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/88a75a95-6438-4553-f5ab-b27ef97c9e00/large)
