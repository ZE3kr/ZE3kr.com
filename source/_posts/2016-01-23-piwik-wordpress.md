---
title: Using Matomo with WordPress to Build a Powerful Statistics System
tags:
  - WordPress
id: '70'
categories:
  - - Development
date: 2016-01-23 08:00:00
languages:
  zh-CN: https://www.guozeyu.com/2016/01/piwik-wordpress/
---

Before it can be used together, Matomo (formerly Piwik) needs to be installed first. [Go to Matomo's official website to download the package](https://matomo.org/download/), and then extract it to the server. Of course, it is better if your server supports one-click installation of Matomo. A PHP environment and MySQL database are required. Just follow the steps to install. It is best to install it under the same host as WordPress, which is more convenient to use.

## Geolocation Function

Go to the Geolocation page in Settings and select Download a GeoIP database in the lower left corner of the page. After downloading, you can use GeoIP (Php), however this is slow. I recommend you to use GeoIP (PECL), if you are using cPanel, then you can enable GeoIP module <!-- more --> directly in Select PHP Version page, then go to `php.ini` to add or Edit this line:

```
geoip.custom_directory = /Matomo/misc
```

Then OK, select GeoIP (PECL) and that’s it!

## Working with WordPress

First, you need to install [Matomo Analytics](https://wordpress.org/plugins/matomo/) under WordPress (perfectly supports multi-site), and then go to settings. If you have both WordPress and Matomo installed on one site, choose the PHP API, otherwise choose the HTTP API. Fill in the Matomo path and Auth Token (which can be found in Matomo's backend), then open Enable Tracking and select the default tracking. In Show Statistics, you can choose which statistics are displayed in which places, which is very convenient.

When you choose Show per post stats, you can see the visitor information of this article on the edit page of each article, which is very nice.

## Working with CloudFlare

First, in order to correctly identify the user IP, you need to add the following line of code to `config/config.ini.php`.

```
proxy_client_headers[] = "HTTP_CF_CONNECTING_IP"
```

If your CloudFlare has IP Geolocation enabled, then you don't actually need to enable GeoIP on the host. Just create a `.htaccess` file in the root directory and add the following lines of code:

```
RewriteEngine On
RewriteBase /
RewriteRule ^ - [E=GEOIP_COUNTRY_CODE:%{HTTP:CF-IPCountry}]
```

Then go to the geographic location option of Matomo statistics. Now, your geographic location option can choose the third GeoIP (Apache), which is the fastest.
