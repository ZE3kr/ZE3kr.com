---
title: Cloudflare Argo And Railgun Comparison Test, The technology of CDN Acceleration
tags:
  - CDN
  - Internet
id: '3108'
categories:
  - - Technology
date: 2017-05-20 19:52:23
languages:
  zh-CN: https://guozeyu.com/2017/05/cloudflare-argo/
---

This website has always implemented foreign parsing to CloudFront for foreign acceleration. Recently, I saw that Cloudflare supports the new function [Argo](https://blog.cloudflare.com/argo/), so I put the foreign CDN from CloudFront. I switched to Cloudflare and turned on Argo to try the effect. Officials claim that TTFB (first byte latency) can be significantly reduced when there is no cache, and the cache hit rate can also be improved when there is a cache. This article will also compare it with Railgun, another enterprise-grade CDN acceleration black technology from Cloudflare.
<!-- more -->

# Cloudflare Argo

## Improve Cache Hit Rate, Argo Tiered Cache

Cloudflare has many nodes, but sometimes too many nodes are not a good thing - **nodes between most CDNs are relatively independent**. The first thing to understand is how CDNs work. CDNs generally do not pre-cache content, but instead cache cacheable content while acting as a proxy for visitors to access. Take this site as an example, this site uses [Hong Kong virtual host] (https://domain.tloxygen.com/web-hosting/index.php), if a visitor from London, UK visits my website, then Since my website is cacheable, it will connect to the London node and be cached there. So what if a visitor from Manchester, UK visited? Since the CDN has another node in Manchester, the visitor will directly connect to the Manchester node. However, there is no cache in Manchester, so the node will return to Hong Kong. And obviously, if Manchester goes back to London, it would be faster to use London's cache. To sum up, if resources can be selectively obtained from other nodes, the TTFB will be lower and the cache hit rate will be increased accordingly. But the general CDN will not do this, because the nodes are independent of each other, and the nodes do not know whether the other party has cached. The general solution is to first pass a few nodes between the node and the source station. These nodes may only be distributed in a few states. For example, there is only one such node in the entire Europe. In this case, after a visitor in London visits, it is also cached by the node in Europe. In this way, when visitors from other parts of Europe connect to a node that does not have a cache, those nodes will directly serve the cache of that node in Europe. CloudFront and KeyCDN take advantage of this technology. How Cloudflare is implemented they do not officially explain in detail. However, in the actual test, no significant increase in the cache rate was observed, which is far less than the effect of CloudFront. The figure below is the TTFB tested by these nodes, and the requests are initiated one by one.

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/25a9957c-de0d-4f7a-d3d9-d3366332ba01/extra" alt="First access test of cacheable content on Cloudflare, Argo enabled" width="1940" height="1038"/>

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/86d28375-4541-48f8-a2cd-b2989cba5a01/extra" alt="CloudFront is better than Cloudflare" width="1934" height="1046"/>

## Lower TTFB, Argo Smart Routing

Usually, the connection between the node and the source station is direct, and the network between them depends largely on the network access of the host. However, with Argo Smart Routing, Cloudflare uses its own routing. Image via Cloudflare.com.

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/256fae72-3104-4b59-4c6a-fe18314b5801/extra" alt="Argo Smart Routing dynamic map" width="960" height="375"/>

When requesting a test address from abroad, the via field is the IP address of the connection between Cloudflare and this site. Through the GeoIP service query, it is found that it is the IP of Hong Kong. Cloudflare establishes a long connection between its own nodes, and also establishes a connection with the origin site in advance on the server closest to the origin site. In this way, the time required for the first connection can be greatly reduced. If the back-to-source is HTTPS, the effect is more obvious. Another test address of mine does not have this function turned on. For comparison, its back-to-source and the IP established by this site are not from Hong Kong.

### Comparison of TTFB with Flexible SSL

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/46c49e08-8914-4b54-f8e7-84137dab0c01/extra" alt="Argo disabled" width="1946" height="1050"/>

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/cf1edeb1-09e8-4e1e-438c-e8edb6ee0401/extra" alt="Argo enabled" width="1978" height="1060"/>

### Comparison of TTFB with Full SSL


<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/670536bb-6c79-403a-246e-7f479deba501/extra" alt="Argo disabled and Full SSL" width="1928" height="1040"/>

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/09b3a372-cf2f-434a-59fc-f86ec28a5201/extra" alt="Argo enabled and Full SSL" width="1940" height="1030"/>

The speed has indeed improved to a certain extent, but it is not particularly obvious, and it seems that some nodes are more unstable after opening it - originally a relatively stable speed, after opening this, some nodes suddenly become faster and slower. It seems that the best way to speed up is half-pass encryption.

#Cloudflare Railgun

Railgun is Cloudflare's ultimate acceleration solution for Business and Enterprise customers. To use it, you first need to upgrade your website plan to Business or Enterprise, then you need to install the necessary software on your server and configure it on Cloudflare. This is equivalent to a bilateral acceleration software. The implementation principle is to let the server establish a long-term TCP encrypted connection with Cloudflare, using Railgun's unique protocol instead of the HTTP protocol, which obviously reduces the connection delay. In addition, it also caches dynamic pages: considering that most dynamic pages contain a lot of the same HTML information, when the user requests a new page, the server will only send the content that has changed. This is equivalent to a multiple Gzip compression.

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/c836e291-5e2d-4d55-371a-1599cf34e101/extra" alt="Enable Railgun screenshot" width="1125" height="1310"/>

Officially, using Railgun can achieve 99.6% compression ratio and achieve twice the speed. The actual experience is also true:

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/75969fec-6ff1-4bdf-204b-a64dfd5d2901/extra" alt="Railgun enabled and Full SSL" width="1982" height="1060"/>

The acceleration effect of Railgun is still very obvious, obviously stronger than Argo.

# Summarize

Argo is not as easy to use as you think, and the starting price of **$5/mo** and **$0.10/GB** of data is not cheap. Of course, it may take a while for Argo to analyze the line delay in order to optimize it better. This article is expected to be updated in a month's time. The effect of Railgun is still extremely significant, but it requires the Enterprise Edition package to be used, and it is not close to the people.

## Dynamic Content

**Latency**: Google Cloud CDN has the lowest latency, followed by Cloudflare Railgun. **Traffic**: For a normal dynamic CMS, Cloudflare Railgun can save roughly 10 times more traffic than Google Cloud CDN can do. When I tested Google Cloud CDN in [Comparison of several full-site CDNs at home and abroad](https://guozeyu.com/2017/01/wordpress-full-site-cdn/), I was surprised by its extremely low TTFB. After careful study, it is found that the node establishes a long connection with the host, and will maintain it for a long time. In addition, all networks go through the Google intranet, which is similar to Argo and Railgun in nature. Therefore, the fastest service for dynamic content should be Google Cloud CDN, and Railgun is basically equivalent.

## Static Content

The Regional Edge Caches included with CloudFront are better than Argo Tiered Cache and Railgun at **caching static content** and **improving cache hit rate**, but Argo Smart Routing is more effective at serving dynamic non-cacheable content Take advantage. Railgun and Google Cloud CDN have no specific optimizations other than caching at the edge nodes.

## Partition Analysis of This Site

The resolution of this site does not use Cloudflare but the self-built DNS, because my Cloudflare domain name is accessed through CNAME. The IP assigned by Cloudflare will not change for a long time, so I directly set its IP to the overseas line. The purpose of using the self-built DNS is to configure CDN lines for domestic partition resolution after filing. PS: Everyone should know that enabling this function will not improve the speed of connecting to Cloudflare in China. If you want to use Cloudflare and want to be faster in China, the origin site is best to use the West Coast of the United States.
