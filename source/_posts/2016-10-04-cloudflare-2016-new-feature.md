---
title: Cloudflare's new feature experience - Load Balancing, Rate Limiting
tags:
  - CDN
id: '2031'
categories:
  - - Development
date: 2016-10-04 09:26:00
languages:
  zh-CN: https://guozeyu.com/2016/10/cloudflare-2016-new-feature/
cover: <img src="https://cdn.yangxi.tech/6T-behmofKYLsxlrK0l_MQ/643d7c6c-859b-4f5f-42e3-44bd338c1101/extra" alt="Load Balancing screenshot" width="1600" height="877"/>
---

Cloudflare finally added two blockbuster features in late 2016, namely:

* Load Balancing (formerly: Traffic Manager)
* Rate Limiting (formerly: Traffic Control)

Load Balancing supports more advanced load balancing functions, and finally supports the cross-region load balancing function that everyone needs; Rate Limiting supports advanced access limit functions. More and more features that were previously required to be configured on the server are now also configurable on Cloudflare. (At present, these two functions are still in the Beta stage, and users need to be authenticated to use them)
<!-- more -->

## Load Balancing

This feature can use Cloudflare as a load balancer without the need to configure load balancing on the hosting provider, [official website introduction](https://www.cloudflare.com/load-balancing/). Currently this feature is only available to Enterprise accounts and some eligible users. There are two ways to implement this function, as follows:

### Via Cloudflare's CDN (Anycast)

The load balancing function is implemented on Cloudflare's edge servers, which are implemented through Layer 7 inversion. In fact, it is very similar to the original CDN function, but the back-to-source can be highly customized. The origin server can be configured with multiple regions (the location of the server needs to be manually set), and multiple servers can be configured in each region, and these servers can be set as a group. Point the domain name to this Group, and then Cloudflare's edge server back-to-origin can **automatically** select the closest origin server based on the server's region. This can effectively reduce the delay of the first byte, which will greatly help to improve the speed of dynamic resources.

<img src="https://cdn.yangxi.tech/6T-behmofKYLsxlrK0l_MQ/b61a7a83-cbf0-401c-495c-2f340bfb9201/extra" alt="Configurable server region (both ways)" width="1008" height="736"/>

In addition, Cloudflare also comes with a Health Check function, which can automatically change back to the source when the server goes down. Although the DNS method can also be used to switch after downtime, the DNS method will be affected by the cache time after all. If CDN switching is used, it can achieve second-level switching. One of my WordPress sites, tlo.xyz, uses this feature. The default is cross-regional load balancing between US East and Asia East, and one of the two will automatically switch if it fails. If all goes down, fallback to static pages on Google Cloud Storage. You can observe the Header of TLO-Hostname on https://tlo.xyz to determine which server responded.

<img src="https://cdn.yangxi.tech/6T-behmofKYLsxlrK0l_MQ/643d7c6c-859b-4f5f-42e3-44bd338c1101/extra" alt="Load Balancing screenshot" width="1600" height="877"/>

### Implemented via DNS (GeoDNS + Weights)

Using this function does not need to open its CDN function, so the visitor directly connects to the origin site. The same way as above is also to configure a Group, but without enabling the CDN function, then Cloudflare will only function as a DNS server. It will automatically perform GeoDNS, return the IP address of the nearest server to the visitor, and also support the Health Check function, which will automatically switch the resolution when the server is down. Unlike other DNS resolvers, Cloudflare is truly intelligent. You only need to configure a Group, and Cloudflare will do the rest automatically, instead of manually choosing which region to resolve to which server. This function has been tested by me. During the current test period, the positioning function of GeoDNS is determined according to the CloudFlare server where the request finally arrives, that is to say, it is determined by the Anycast system. However, the vast majority of Chinese users will be directed to CloudFlare's western US server by the operator, and will be parsed by the GeoDNS system to the result corresponding to the western US location. Therefore, this function is still very limited and not suitable for domestic use.

## Rate Limiting

Cloudflare can finally limit the request rate of IP, this feature can filter CC attacks quite effectively, and has almost no impact on ordinary visitors (previously it was only possible through the I'm under attack feature, but this feature will make all users wait 5 seconds before load). It can configure different request rates according to different paths, and can achieve functions such as preventing brute force password cracking and preventing API interface abuse.
