---
title: How to configure for pure IPv6-Only network access
tags:
  - Internet
id: '1804'
categories:
  - - Development
date: 2016-08-09 20:32:16
languages:
  zh-CN: https://www.guozeyu.com/2016/08/talk-about-config-ipv6-on-server/
cover: <img src="https://cdn.tloxygen.com/images/8b2395b9-3296-4572-f144-8a299767a900/extra" alt="Test screenshot" width="1008" height="670"/>
---

On May 4 this year, Apple began to require new applications to support IPv6 DNS64/NAT64 networks, which means that Apple began to push IPv6 networks, on [Apple's official website](https://developer.apple.com/library/mac/documentation/NetworkingInternetWeb/Conceptual/NetworkingOverview/UnderstandingandPreparingfortheIPv6Transition/UnderstandingandPreparingfortheIPv6Transition.html#//apple_ref/doc/uid/TP40010220-CH213-SW1) has introduced some advantages of IPv6, mainly to be more friendly to mobile networks, And can improve some performance, reduce some transmission overhead. Recently, I have also fully implemented IPv6 on all my servers, fully supporting IPv6-Only networks.
<!-- more -->

What is an IPv6-Only network Strictly speaking, only IPv6 addresses can be connected to an IPv6-Only network, which means that the DNS cache server must also be an IPv6 address and can only be connected to servers that support IPv6. If a domain name is to be resolved, the DNS server of the domain name itself and the root domain name to which it belongs must also support IPv6. In conclusion, there is no IPv4 in the whole process. So if you want to support IPv6-Only network, you need to work hard in almost all links.

## Use IPv6-capable Root Domains and DNS Servers

In order to support IPv6, first of all, the root domain name you use must support IPv6. Fortunately, most root domain names support IPv6. [From here](http://bgp.he.net/ipv6-progress-report.cgi), we can see that 98% of the root domain names already support IPv6. Generally, people still prefer to use third-party DNS instead of building their own, so it is necessary to use a DNS server that supports IPv6 for themselves. Fortunately, there are also many DNS servers that support IPv6, such as CloudFlare, OVH, Vultr, DNSimple, Rage4, etc. Most foreign DNS resolutions, oh, except for Route53; Baidu Cloud Acceleration, sDNS DNS resolution support has been seen in China, other Commonly used CloudXNS, DNSPod, Alibaba Cloud, etc. are not supported. To detect whether the root domain name or the DNS server used by a domain name supports IPv6, just execute the command and replace the `<domain>` with the root domain name (such as com) or any first-level domain name (such as example.com):

```
$ dig -t AAAA `dig <domain> ns +short` +short
```

Then check whether the output IP is all IPv6 addresses. If nothing is output, the DNS server does not support IPv6. Example of proper IPv6 configuration:

<img src="https://cdn.tloxygen.com/images/e81dd301-947a-4e87-694e-7f0d29a9f300/extra" alt="The root domain name com and the first-level domain name example.com are both correctly configured with IPv6-enabled DNS servers" width="772" height="174"/>

If you want to build your own DNS server, you can refer to [How to Build Your Own PowerDNS](https://www.guozeyu.com/2016/08/self-host-dns/). If your root domain name does not support IPv6, then you can contact the root domain name to have them support it, or change the root domain name. If your first-level domain name does not support IPv6, contact the DNS resolver to ask them to support it, or simply change it.

## Make Servers such as Websites And APIs Support IPv6

### Scheme 1, Configure IPv6 Directly

That is to make your own server support IPv6, you need to contact your server provider and ask them to assign you an IPv6 address. If it still does not support IPv6, then you can use IPv6 Tunnel Broker, such as Hurricane Electric's free Tunnel Broker, so Usually no better than the native IPv6 that the service provider gives you, but you can only use it until the service provider supports native IPv6. Tunnel Broker **equivalent to a proxy** built on the network layer (the third layer), requires the support of your server's operating system, and the server must have a fixed IPv4 address. In order to use it you need to reconfigure the network card in the system (so shared hosting is out of the question), and then you can use IPv6 in the normal way, supporting IPv6 at almost zero cost. Note that host to Tunnel Broker transfers are in clear text, but as long as applications are using secure protocols (eg HTTPS, SMB, SFTP, SSH) this is not a problem.

It is a pity that my current two servers do not have native IPv6 available for the time being, so I can only use Tunnel Broker. After using it, I found that although it is free, the effect is also good: there seems to be no speed limit when downloading, my 100M unique The download speed of the shared bandwidth is 12Mbyte/s on native IPv4, which is almost the same speed on IPv6. There will still be some increase in the ping delay, mainly because there will be some delay between the Tunnel Broker server connecting to your server, it is equivalent to a proxy, so be sure to select the Tunnel Broker closest to your server (the one with the shortest delay) when creating. , instead of the user's nearest Tunnel Broker.

Note that it is strongly not recommended for domestic hosts to use HE's Tunnel Broker, because China's connection to HE will bypass the United States, which will eventually add 500ms of ping delay to IPv6, more than 1000ms of TCP delay and several seconds of HTTPS delay. It is recommended that domestic hosts use plan two.

After the server supports IPv6, make sure that the newly set AAAA record on the domain name resolves to the IPv6 address.

### Option 2, on CDN/HTTP Proxy

The solutions just introduced are direct support, or use Tunnel Broker to build a proxy at the network layer. Of course, there is another way of proxying, which is based on the application layer (layer 7). What is built on the seventh layer is actually HTTP Proxy, but most places that provide HTTP Proxy functions can cache static content. , so that is CDN. The best solution is to use the free CloudFlare directly, then turn on the CDN function, which requires changing the DNS server (now you can use [CNAME/IP access](https://cf.tlo.xyz), and even configure IPv4 Back-to-origin, only use IPv6 CDN), but then all services including DNS can support IPv6, similar to Akamai, they can give you IPv6 support after proxy. However, if this scheme is used, the original function of collecting the user's real IP will fail, including firewalls and web applications. But with just a few changes to the configuration, the guest IP can be collected again.

* * *

After doing these configurations, the website can be accessed by IPv6-Only network. However, this is only one step. Don’t forget that the CDN on the website and the mail service on the domain name do not yet support IPv6. Finally, let me list:

## Some List of Services that Support IPv6

### CDN

* CloudFlare ([CNAME/IP access](https://cf.tlo.xyz))
* Akamai

### VPS

* [Vultr](https://www.vultr.com/?ref=6886257)
* Linode
*DigitalOcean
* OVH

### DNS

Note: The services listed in CDN and VPS all provide DNS that supports IPv6 and will not be listed here (the DNS services provided by Linode and DigitalOcean are actually also provided by CloudFlare)

*Rage4
* Hurricane Electric DNS
* Route 53
* Google Cloud DNS

### Tunnel Broker

* Hurricane Electric Tunnel Broker

### Mail Service

*Gmail / G Suit (Gmail for Work)

* * *

When you have it configured, you can [test your website on IPv6 Test](http://ipv6-test.com/validate.php).

<img src="https://cdn.tloxygen.com/images/8b2395b9-3296-4572-f144-8a299767a900/extra" alt="Test screenshot" width="1008" height="670"/>

Until now, it is still not necessary to support IPv6-Only network access in production, because there are very few IPv6-Only networks, generally compatible with IPv4, and many large websites do not support IPv6 at all. Apple's requirement to support IPv6-Only is only that the program needs to use IPv6 communication internally, and the program cannot have an IPv4 address, which can be used by operators that only assign IPv6 addresses (however, these operators still support IPv4).
