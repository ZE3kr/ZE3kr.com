---
title: Comparison of Several Full-Site CDNs
tags:
  - CDN
  - DNS
  - WordPress
  - Website
id: '2562'
categories:
  - - Development
date: 2017-01-30 08:00:23
languages:
  zh-CN: https://guozeyu.com/2017/01/wordpress-full-site-cdn/
---

Configuring a site-wide CDN can cache HTML pages and speed up time to first page load. This article focuses on WordPress site-wide caching, solutions for mixing domestic and international CDNs, and the correct way to cache pages on CDNs. This article mainly introduces CloudFront, and also compares several CDNs such as Cloudflare, Youpaiyun, Baidu Cloud Acceleration, KeyCDN, and Google Cloud CDN.
<!-- more -->

## Where Can The Site-Wide Cdn Be Accelerated?

### I have only cached static resources such as images, videos, CSS and JS like you did before. What are the advantages of a site-wide CDN?

Even if you don't cache any content, a site-wide CDN has its advantages:

1. **SSL offload**: HTTP transmission is used before origin site to CDN, and HTTPS transmission is used from CDN to user. In this way, the hardware burden originally caused by SSL on the origin site can be reduced. However, since the transmission from the source station to the CDN is in plain text, it is only recommended to use it in the case of the entire intranet or when the line is controllable, otherwise there will be great security risks. However, almost all CDNs can also be configured to use HTTPS encrypted transmission between the origin site and the CDN.
2. **Security protection**: All CDNs have their own protection based on the first, second, and third layers of the network (to prevent SYN attacks), and most of them also bring the seventh layer of protection (to prevent CC attacks).
3. **Reduce latency**: Before establishing an HTTP connection, the SSL handshake between TCP and HTTP is required, which will increase the time required for the first load. Many CDNs will pre-connect with the server, and the CDN's server is closer to the user, so it can reduce the time it takes to load the first time, even if the page is not cached.
4. **Cache dynamic pages**: The so-called dynamic pages, in fact, most of the content is fixed for users who are not logged in (such as WordPress article pages), so it can be cached for users who are not logged in. This requires the CDN to automatically determine whether the user is logged in based on the cookie, and only Cloudflare Enterprise Edition and CloudFront can do it.

## Using CloudFront as a Full-Site CDN

CloudFront has Amazon's self-built network. The unit price is high but starts from 0 yuan, which is suitable for small and medium customers. This article will focus on how CloudFront and WordPress work together to achieve dynamic and static separation and cache HTML pages. Some other CDNs will be compared later.

* Overseas speed: ★★★★☆, the number of nodes is large, but it is worse than Cloudflare, and does not support Anycast.
* Domestic speed: ★★★☆☆, the domestic start to go to the Asian node, the speed is faster than Cloudflare
* Customizability: ★★★★★, you can use different servers according to different paths back to the source, or even back to different servers, and there is no upper limit on the number of rules. In addition, with [Lambda@Edge](http://docs.aws.amazon.com/zh_cn/AmazonCloudFront/latest/DeveloperGuide/what-is-lambda-at-edge.html), it is even possible to convert the original server response Dynamic content is handed over to the cache server, just use Node.js.
* Cheap index: ★★★☆☆, starting from free, Pay-as-you-go, the unit price is really high, but you can exchange for a lower price by sacrificing speed by price level
* Convenient access: ★★☆☆☆, need to configure various cache parameters to use, NS access is not direct, CNAME access is more difficult than NS. The SSL certificate also needs to be applied manually for the first time, but it is automatically renewed.
* Cache hit: ★★★★☆, support _Regional Edge Caches_, first cache to 9 nodes around the world, and then distribute down, greatly improve the cache hit rate
* Dynamic and static separation: ★★★★★, automatic separation, one service can set different Behaviors according to different directories, or even configure multiple origin servers, support matching Cookie, GET, Header rule caching, support disabling POST and other submission methods
* Cache refresh: ★★★★☆, support single URL refresh and rule matching refresh
* Access method: NS/CNAME
*Certificate Compatibility: By default, only browsers that support SNI are available, and additional services ($600/month) can be purchased to be compatible with all browsers.

Features of CloudFront as a site-wide CDN:

* Support root domain name
* Cache static files
* cache page
* page update automatically clean cache
* Free SSL certificate
* Can be configured to mix domestic and international CDNs

### Instructions

First go to the [CloudFront Control Panel](https://console.aws.amazon.com/cloudfront/home), click "Create Distribution", select "Web", and configure something like the following. **Origin site configuration** Note that the Origin Domain Name must be a full domain name, and if HTTPS is used, a valid certificate must be configured under that domain name.

![CloudFront basic configuration screenshot](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/1a7f2efe-1d91-458a-12dc-de586ea9ef00/large)

**Cache configuration**, I added Host to the Header whitelist, and Cookie to add `wordpress*` and `wp*` to the whitelist.

![CloudFront detailed configuration screenshot](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/9170b325-17e1-4263-6083-633d9320bf00/large)

Then **front-end configuration**, the certificate point "Request or Import a Certificate with ACM" can apply for a certificate issued by Amazon. Fill in the domain name of your website under CNAMEs.

![CloudFront front-end configuration screenshot](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/692a51b9-2e24-481b-ce2e-a0de07dfdd00/large)

Note that it may take less than an hour after creation to be accessed. In order to use the root domain name with CloudFront, I also had to change the DNS resolution of Route 53. Since this is a very high-precision GeoDNS, it is necessary to submit the resolution server to the major DNS cache servers, so that these cache servers can be added to the EDNS Client Subnet-enabled whitelist for your DNS cache server. Fortunately, Route 53 is one of the most popular GeoDNS, so if you use the NS records it gives you and don't customize it, you don't have to worry about it. When configuring the root domain name, simply select the A record, open Alias, and fill in the CloudFront domain name. If you want to support IPv6, then create another AAAA record. This way if you parse from the outside, you will parse directly to A records and AAAA records, not CNAMEs!

![Screenshot 1 of CloudFront with Route 53](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/550e246f-35ba-42ce-9a01-f4510dbf3c00/large)

At this point, CloudFront is configured. CloudFront now automatically caches pages for about a week, so you need to configure the cache to clear when articles are updated. I wrote a plugin that cleans all caches when there is an article update/theme modification/kernel update, cleans the article page when there is a new comment, and controls the refresh frequency to 10 minutes (this is due to the fact that CloudFront refreshes the cache is surprisingly slow, and Flushing the cache is only free for the first thousand times). Welcome to [use a plugin I made](https://wordpress.org/plugins/full-site-cache-cf/). However, the access speed of CloudFront in China is not as good as the GCE I used before. What should I do? It doesn't matter, Route 53 can use GeoDNS, I still resolve China and Taiwan to the original GCE, so the speed is only improved. Note that if you want to do this, the original server must also have a valid certificate (similarly, if your domain name has been filed, you can set it as the IP of the domestic CDN to achieve the effect of mixing domestic and international CDNs). CloudFront will affect the issuance of Let's Encrypt, so you need to continue to implement port 80 file authentication by setting Behaviors and multiple origin servers. In the actual test, the IPv4 recognition rate of Route 53 for China is 100%, and the IPv6 recognition rate is not good.

![Screenshot 2 of CloudFront with Route 53](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/6b6bd584-041b-4a72-1349-bd1829b3de00/large)

### Actual Usage

#### GeoDNS Test

Domestic analysis:

````
$ dig @8.8.8.8 +short guozeyu.com a
104.199.138.99

$ dig @8.8.8.8 +short guozeyu.com aaaa
2600:9000:2029:3c00:9:c41:b0c0:93a1
2600:9000:2029:9600:9:c41:b0c0:93a1
2600:9000:2029:2a00:9:c41:b0c0:93a1
2600:9000:2029:1600:9:c41:b0c0:93a1
2600:9000:2029:c00:9:c41:b0c0:93a1
2600:9000:2029:ce00:9:c41:b0c0:93a1
2600:9000:2029:6400:9:c41:b0c0:93a1
2600:9000:2029:ac00:9:c41:b0c0:93a1
````

Analysis of international countries:

````
$ dig @8.8.8.8 +short guozeyu.com a
52.222.238.236
52.222.238.227
52.222.238.207
52.222.238.107
52.222.238.208
52.222.238.71
52.222.238.68
52.222.238.67

$ dig @8.8.8.8 +short guozeyu.com aaaa
2600:9000:202d:5c00:9:c41:b0c0:93a1
2600:9000:202d:ec00:9:c41:b0c0:93a1
2600:9000:202d:7c00:9:c41:b0c0:93a1
2600:9000:202d:2a00:9:c41:b0c0:93a1
2600:9000:202d:9400:9:c41:b0c0:93a1
2600:9000:202d:c600:9:c41:b0c0:93a1
2600:9000:202d:f600:9:c41:b0c0:93a1
2600:9000:202d:6200:9:c41:b0c0:93a1
````

That's right, the number of IPs that CloudFront assigns is too many, and it will be very powerful for others to see.

#### Ping Before and After Startup

Only the speed of international countries is compared here.

![Origin speed](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/f4fc9c84-05a8-4925-b6fe-16e7ffd60300/large)

![CDN speed](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/3170443b-593f-44ad-016f-9e6bfe087600/large)

#### HTTPs Get Before and After Startup

It is only compared to the speed of international countries also.

![Origin speed](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/076b09e9-37e7-478d-4df3-77a0bc18ee00/large)

![CDN speed](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/d6a047f9-2b60-4d68-105f-b47105553c00/large)

TTFB is almost fully green after CDN is launched, and the time to establish TCP and TLS is significantly reduced.

#### Amazon's SSL Certificate

The SSL certificate issued by CloudFront for free is a **multi-domain wildcard certificate** (Wildcard SAN), and the main name is customized, which is higher than Cloudflare's shared certificate. Such certificates cost $10 per month on Cloudflare. Such certificates are hard to buy in the market, and the price depends on the number of domain names, ranging from thousands to tens of thousands a year. However, this certificate can only be used on AWS' CloudFront and load balancers.

![Amazon Certificate](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/cf5ba387-927b-4147-c1f6-c425f9b2af00/large)

CloudFront's long certificate chain affects TLS times, but since it's also a CDN, this reduces TLS times to almost negligible levels. The main reason is that the Amazon Root CA is not directly trusted on macOS. If you trust it directly, you don't need to do this.

## Comparison of Other CDNs

All providers listed below (or some versions of a provider) meet the following criteria, or are listed separately if they do not:

1. It is a CDN manufacturer
2. Support HTTPS, HTTP/2
3. Free SSL certificate issuance and automatic renewal
4. I am using it now, or have used it for a long time, or have a deep understanding of it
5. Can speed up WordPress site-wide CDN

### Cloudflare Free/Pro

It has a self-built network, the fastest speed and the lowest price, mainly provides website security protection, and of course also comes with a CDN. The NS service it provides is also the industry's first speed (international). This site uses Cloudflare, and you are welcome to directly test the speed of this site.

> Use the agent [cf.tlo.xyz](https://cf.tlo.xyz) to access the domain name to Cloudflare CDN, which can realize CNAME/IP access, and also supports Railgun.

* Overseas speed: ★★★★★, full marks for having numerous overseas nodes and supporting Anycast. The speed indicator refers to the Ping value around the world, the same below
* Domestic speed: ★★☆☆☆, the domestic node goes to the west coast of the United States, the speed is not good
* Customizability: ★★★★★, you need to use Page Rules for basic customization, and you can also use Worker for programming customization.
* Cheap index: ★★★★★, start from free, give full marks
*Easy access: ★★★★★, directly access after changing NS, SSL certificate is automatically issued, no server configuration is required, is there anything simpler than this?
* Cache hit: ★★★★★, due to the large number of nodes, each place needs to be cached separately, so the cache hit rate is very low; but if [Argo](https://blog.cloudflare.com /argo/), then a higher cache hit rate can be achieved, and the optimal line can be automatically allocated. Argo requires additional monthly consumption ($5/mo + $0.10/GB).
* Dynamic and static separation: ★★★☆☆, automatic separation, it abides by Cache-Control rules, you can also set _Page Rules_ to modify the default cache rules. However, HTML pages are not cached by default, there are only 3 _Page Rules_ limits, and no caching of matching cookie rules is enabled.
* Cache refresh: ★★☆☆☆, only supports to refresh the URL of a certain page and refresh all content, does not support rule refresh
* Access method: NS (if it is Partner, you can have free CNAME access)
* Certificate Compatibility: SNI-enabled browsers only

Features of Cloudflare as a site-wide CDN:

* Support root domain name
* Cache static files
* **NOT SUPPORT** Cached pages
* Free SSL certificate

One difference is that Cloudflare issues a shared certificate, which looks like this:

![Cloudflare Shared Certificate](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/cf5ba387-927b-4147-c1f6-c425f9b2af00/large)

I think Cloudflare issues shared certificates for two reasons. One is a legacy issue: Cloudflare Professional Edition's SSL certificate service supports clients without SNI, and in order to support clients without SNI, only one certificate can be configured for one IP. Therefore, a shared certificate is used to save IP resources. And now the free version also has SSL. Although the free version uses SNI technology, the certificate cannot be more advanced than the paid version, so a shared SSL certificate is still used; the second is to add more value-added services, which is now available on Cloudflare Purchase Dedicated SSL Certificates and implement independent certificates (if the paid version is enabled, clients that do not support SNI still fall back to the shared certificate, so they are still compatible with devices that do not support SNI).

### Cloudflare Enterprise Edition

The introduction is the same as above. This version is suitable for large customers, and the greater the traffic, the more suitable it is. Baidu Cloud Acceleration is deeply integrated with Cloudflare. In fact, the infrastructure is exactly the same, but Baidu Cloud Acceleration does not provide an API interface.

* Overseas speed: ★★★★★, same as above
* Domestic speed: ★★★★★, there are many domestic nodes
* Customizability: ★★★★★, same as above, the number of Page Rules has been improved
* Cheap index: ★★☆☆☆, fixed monthly expenses, not pay-as-you-go, the larger the amount, the more affordable
* Easy access: ★★★★★, same as above
* Cache hit: ★★★★★, same as above
* Dynamic and static separation: ★★★★☆, for WordPress, it can be automatically separated after installing the official plugin. Compared with the free version, it has more _Page Rules_ and supports matching cookie rule caching, which is worse than CloudFront
* Cache refresh: ★★★★☆, compared to the free version, it supports Cache-Tag refresh rules, but requires server-side configuration
* Access method: NS/CNAME
* Certificate Compatibility: **all** browsers

Features of Cloudflare Enterprise Edition as a site-wide CDN:

* Support root domain name
* Cache static files
* Support cached pages
* The page update automatically cleans the cache (perfectly cleans the cache) (Because Baidu Cloud Acceleration does not provide API, Cloud Acceleration does not have this function)
* Powerful DDOS defense and WAF function
* Free SSL certificate

### UPYUN

Using the computer room managed by yourself, the network is somewhat limited by the environment in China, and the unit price is the lowest in the industry.

* international speed: ★★☆☆☆, North America, Europe, Asia have a certain number of nodes, but the speed is not good. In addition, the NS used for CNAME access has no nodes outside China, so it sacrifices a lot in terms of parsing speed.
* Domestic speed: ★★★★★, there are many domestic nodes, but since only CNAME access is possible, it needs to parse the request one more time, which takes time
* Customizability: ★★★★☆, many caching rules can be set, and there are [custom](http://docs.upyun.com/cdn/rewrite/) [Rewrite](http://docs. upyun.com/cdn/rewrite/), which can achieve richer functions than Rewrite, but the functions are limited.
* Cheap index: ★★★★½, starting from free, Pay-as-you-go, the price also drops again and again, the industry's lower standard
* Convenient access: ★★★☆☆, no need to configure many parameters, CNAME access is more difficult than NS. The SSL certificate needs to be manually added for the first time and automatically renewed.
* Cache hit: ★★★★½, with the function of source site resource migration, which is directly and permanently cached after the first visit. However, if you want to delete the file, you also need to use the API to delete it manually, and half points will be deducted.
* Dynamic and static separation: ★★★★☆, automatic separation, you can configure caching rules for different directories, but does not support cookie rule caching
* Cache refresh: ★★★★☆, support single URL refresh and rule matching refresh
* Access method: CNAME, so the root domain name cannot be used
* Certificate Compatibility: SNI-enabled browsers only

Features of UPYUN as a site-wide CDN:

* **NOT SUPPORT** Root domain name
* Cache static files
* **NOT SUPPORT** Cached pages
* With WAF function
* Free SSL certificate
* Can be configured to mix domestic and international CDNs

Both UPYUN and the following KeyCDN issue independent certificates from Let's Encrypt, but they are all single-domain certificates, and even those with www and without www have to apply separately.

### Google Cloud CDN

It has the densest network cluster in the world, the fastest speed, lower unit price, mainly provides load balancing, SSL offloading, and of course comes with CDN. Due to the low cache hit rate, it is effective for websites that require very large traffic. It is precisely because of this that Google only uses this CDN system for the search service with a large number of users, and many other CDNs use Cloudflare and Fastly. Google's network has intranet links to Cloudflare and Fastly's networks. [For details, see this article on this site](https://guozeyu.com/2016/10/build-a-anycast-network-gce/)

* Overseas speed: ★★★★★, full marks for having numerous overseas nodes and supporting Anycast.
* Domestic speed: ★★★☆☆, which is directly connected to the Hong Kong node in China, which is almost the fastest network in Hong Kong. It has access to several major domestic operators, but after all, there is no domestic node, which is not comparable to some domestic speeds. (But some IPs currently allocated, China Unicom will detour to West America)
* Customizability: ★★☆☆☆, you can configure different servers according to different paths, and then there is nothing else to customize.
* Cheap index: ★★☆☆☆, due to occupying IP resources, it needs to spend a fixed price of $18 per month, and you need to pay for traffic. The unit price of traffic is lower.
* Easy access: ★☆☆☆☆, requires various complex configurations, but once the configuration is completed, it will have features such as load balancing, elastic scaling and so on.
* Cache hit: ★½☆☆☆, too many nodes, it is difficult for small traffic websites to encounter hits. However, cross-region load balancing can be used to improve the cache hit rate.
* Dynamic and static separation: ★★☆☆☆, automatic separation, but no rules can be configured.
* Cache refresh: ★★★★☆, support single URL refresh and rule matching refresh
* Access method: IP binding, it directly assigns an independent Anycast IP to you, and only needs A record analysis.
* Certificate Compatibility: **all** browsers
* Does not include free SSL, you need to buy it yourself

Features of Google Cloud CDN as a site-wide CDN:

* Support root domain name
* Cache static files
* **NOT SUPPORT** Cached pages
* **NO** Free SSL Certificate
* Can be configured to mix domestic and international CDNs

### KeyCDN

They rent independent servers from others and provide integrated CDN services with the lowest unit price in the industry.

* Overseas speed: ★★★★☆, which is comparable to CloudFront, but because it can only access CNAME, it needs to parse the request one more time, which takes time
* Domestic speed: ★☆☆☆☆, if it is domestic, the Hong Kong node, but the detour is crazy, but it is slower than the United States
* Cheap index: ★★★☆☆, with minimum annual fee (equivalent to the starting price)
* Convenient access: ★★★½☆, no need to configure many parameters, CNAME access is more difficult than NS. SSL can be added with one click.
* Cache hits: ★★★★☆, similar to CloudFront _Regional Edge Caches_
* Dynamic and static separation: ★★☆☆☆, automatic separation, but rules cannot be configured. Cache configuration for cookies is supported, but cannot match cookie content
* Cache refresh: ★★★★
