---
title: Several recommended plugins for WordPress
tags:
  - WordPress
id: '96'
categories:
  - - Development
date: 2016-01-29 10:19:00
languages:
  zh-CN: https://guozeyu.com/2016/01/wordpress-plugins/
---

## EWWW Image Optimizer

Lossless and lossy compression of JPEG and PNG images, support for compressing existing images, can speed up the loading of images for visitors. Also supports progressive loading of JPEG. Under normal circumstances, when the network speed is low, the picture is loaded bit by bit from top to bottom, but with progressive loading, the low-resolution version of the picture is loaded first, and then gradually becomes clearer. **I have become a paid user of this plugin and can further lossy compress PNG and JPEG images, reducing server CPU usage (otherwise each uploading image will consume a lot of CPU). However, I recently used Cloudflare's Polish function, and I no longer install similar software on the server.

## Autoptimize

This plugin can automatically merge CSS and JS and compress them, which is very convenient for <!-- more -->. And some themes will have a lot of inline CSS, when merge CSS is turned on, these inline CSS will be automatically added to the file. My **some configuration** in this plugin, to share:

### Exclude Scripts from Autoptimize

functions.js,player,mediaelement,sparkline,toolbar,admin,akismet,themes

* functions.js,themes: Exclude all scripts related to the main theme, and comprehensively solve the problem of menu interaction after merging
* player,mediaelement: Exclude the merging of WordPress players, because some pages in this part of the file have and some pages do not
* sparkline, toolbar, admin: these files will only be loaded after the user is logged in
* akismet: It is recommended to add after installing the akismet plugin

### Exclude CSS from Autoptimize

admin,mediaelement,wordfence,piwik,toolbar,dashicons,crayon-syntax-highlighter/themes,crayon-syntax-highlighter/fonts

* admin,mediaelement,toolbar,dashicons: same as above, only the admin will load, or only some of the pages will load
* crayon-syntax-highlighter/themes,crayon-syntax-highlighter/fonts: Exclude CSS that the Crayon Syntax Highlighter plugin only loads on some pages
* wordfence, piwik: exclude some plugins

### Recommended to Enable

* **Optimize HTML Code**: Optimize HTML code to reduce page size
* **Optimize JavaScript Code**: Optimize JS code, reduce JS size and merge JS
* **Force JavaScript in `<head>`**: can load JS ahead of time
* **Optimize CSS Code**: optimize CSS code, reduce CSS size and merge CSS
* **Generate data: URIs for images**: Embed files directly into CSS to reduce the number of requests
* **Remove Google Fonts**: Remove Google Fonts, these font files are only valid for Western, not meaningful for Chinese.
* **Also aggregate inline CSS**: Putting inline CSS into CSS file can reduce page size
* **Save aggregated script/css as static files**: can improve processing performance

### Recommended to Disable

* **Keep HTML comments**: Sometimes HTML comments are useful
* **Also aggregate inline JS**: The JS of each page is different, do not enable
* **Add try-catch wrapping**: something that doesn't make sense
* **Inline and Defer CSS**: Embedding CSS fully into the page will greatly increase the page size, but it can reduce the number of requests, but in most cases it will only make the website slower.
* **Inline all CSS**: same as above

##AMP

Plugin author: Automattic, optimized for Google, showing super fast AMP pages in search results. Details: [Building Ultra-Fast Mobile Pages with AMP](https://guozeyu.com/2016/10/amp-html/)

## Google Authenticator

Two authenticators (need to cooperate with the mobile client of the same name, or 1Password Professional Edition). Have you ever heard of the Heart Bleed exploit? With two certifications, there is no need to be afraid even if there is another Heart Bleed vulnerability. Principle: Scan the QR code on the mobile phone generator to save the key, and then the generator generates a 6-digit number according to the time variable. These 6 digits have an expiration date and can only be used once. Security is even greater than SMS verification code

## IP Dependent Cookies

After changing the IP, you need to log in again. Even using HTTP can avoid the risk of cookie leakage to a certain extent.

## Wordfence

WordPress #1 **Security Defense** plugin, which can limit the number of password attempts to prevent brute force cracking, add WAF function to your WordPress, and view real-time access. By looking at its log, I found out that my WordPress has been being maliciously cracked by brute force, sometimes thousands of times a day, **almost every day**. Fortunately, as long as you try 3 wrong passwords, the IP will be blocked directly, which is why there are not many Failed Logins. This kind of defense is based on the application layer, and it is not something that cloud-accelerated and anti-DDOS services such as Cloudflare can defend against.

## Crayon Syntax Highlighter

This plugin enables WordPress to display auto-highlighted source code, with a variety of themes to choose from and support for multiple programming languages.

## Slimpack

![Slimpack screenshot](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/60c81119-cf51-482d-8603-17a82c163800/large)

This is a simplified version of Jetpack, without the bunch of useless features of Jetpack, no need to log in to wordpress.com, full-featured, and very easy to use.

## XML Sitemap & Google News feeds

This plugin can automatically generate `sitemap.xml` and `robots.txt` of the website, you can directly submit `sitemap.xml` to Baidu and Google, so that search engines will not miss every article on your website article too. This plugin supports WordPress multisite without any configuration and is recommended to be enabled on the entire network.

##WP-Piwik

![WP-Piwik screenshot](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/86f98b39-867f-4be9-4c16-6ffc480f2200/large)

This plugin enables your entire website to have statistical functions, supports WordPress multi-site, and is recommended to be enabled on the entire network. About Piwik with WordPress, please refer to this article: [Use Piwik with WordPress to build a powerful statistical system](https://guozeyu.com/2016/01/piwik-wordpress/).

## Blubrry PowerPress

This plugin enables your WordPress to generate a Podcast Feed, allowing you to have a podcast platform, and you can also submit this feed directly to places like iTunes.

![screenshot of podcast software on iOS](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/4688a970-5350-4c93-f6ff-e1579be7ea00/large)

## Exif Caption

This plugin allows you to insert the Exif information of the picture into the description of the picture. The disadvantage is that it needs to be added manually after uploading the picture, but it can be added in batches, which is quite convenient. After inserting the Exif information into the description of the picture, and then inserting the picture into the article, the Exif information can be displayed in the article.
