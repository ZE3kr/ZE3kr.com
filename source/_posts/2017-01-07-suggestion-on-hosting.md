---
title: Some suggestions on website construction and service purchase
tags:
  - VPS
  - Website
  - Internet
id: '1752'
categories:
  - - Technology
date: 2017-01-07 07:00:18
languages:
  zh-CN: https://www.guozeyu.com/2017/01/suggestion-on-hosting/
---

Building a website, building a software (or game) server, etc., may have many entanglements and pits. This article is based on my personal experience to help you choose the most suitable solution for you, as well as some suggestions, listed at the end Come out with some services that I recommend.

## Three Types Of Services

In general, the services that need to be purchased for building a website also belong to these three services. From SaaS, Paas to IaaS, from ease of use to complexity, from low to high scalability. Understanding these three service types can help you choose the right service.
<!-- more -->

+ **SaaS, software as a service**: For example, WordPress.com provides a complete set of services (including domain name, hosting, platform and software), even if it is SaaS, this service is the most convenient for beginners to use, professional People can also play a lot of tricks, but the scalability is limited by the software, usually this kind of service has high availability and is distributed.
+ **PaaS, platform as a service**: such as web hosting based on cPanel, Plesk panel, you can install existing open source software on the platform, or upload your own software code, the overall use is almost as simple as SaaS, using specific operating system and code language, install various programs. There are ordinary and distributed PaaS, and the latter is better than the former in terms of availability, data assurance, and scalability.
+ **IaaS, Infrastructure as a Service**: includes VPS, Dedicated Server, Cloud VPS and Public/Private Cloud Server. The main difference between VPS and independent servers is in configuration, including CPU, memory, hard disk, etc. The configuration of the latter will be much higher, and there is not much difference in availability. Cloud VPS and public/private cloud are actually based on cloud-based distributed VPS and dedicated servers. They are the same, their availability, data assurance, and scalability are better than VPS and dedicated servers. IaaS requires you to choose your own operating system, and all services and software are configured by yourself, usually giving you root privileges.

## Service Selection

+ **Domestic for location selection**: First thing to consider is domestic or foreign? If you choose domestic, first of all you need to go through the filing of the domain name, which may take nearly a month, which undoubtedly increases a lot of costs for building a website. And domestic servers are generally more expensive. However, if you want to choose domestic, the host I recommend is Sina's SAE, which is very cheap, pays on demand, and has free quotas. It belongs to distributed PaaS, provides high availability services, and supports Docker. Container**, DDOS defense. However, SAE does not provide CDN and custom domain name HTTPS for non-enterprise users. I recommend that you use UPYUN to implement CDN and custom domain name HTTPS functions. Domestic advantages: It can ensure low network delay and can comply with some current laws and policies.
+ **Location selection abroad**: Many Asian hosts (including but not limited to Hong Kong, Japan, Singapore) will "detour" from the mainland connection, resulting in high latency, you need to understand before buying, Hong Kong is cheap to share Hosting recommendation [TlOxygen](https://domain.tloxygen.com/web-hosting/index.php?promo=ze3kr). Hosts in Asia generally have low network bandwidth and less traffic. If you don't choose Asia, it is recommended to choose North America first. Foreign hosts usually have lower prices and better international bandwidth than many domestic hosts. If possible, you can configure multiple servers, the recommended solution for **two servers** is **Asia + North America** east coast, and point both Asia and Oceania to Asia, and the rest to North America. If you can configure more servers, you can configure four servers of ** North America East Coast + North America West Coast + Europe + Asia** at the same time and partition analysis. As far as my actual test is concerned, point Africa to Europe and Oceania to point The West Coast of North America can reach the fastest speeds.

**Some features that may be required**: Based on the different functions to be implemented, there may be requirements in the following aspects, which need to be prioritized when selecting services.

* **High data reliability**: Almost all websites or network-related software need to store user data, so high data reliability is very important. So the disk array is RAID1 and RAID10 is the first choice. And the backup of user data is also very important, which can be stored in a third party such as AWS S3. Some service providers provide backup functions, which can actually guarantee high data reliability.
* **The ability to defend against attacks**: The cost of attacks has become lower and lower, and there are many types. Usually the most difficult type of attack to defend against is DDOS, the rest are almost all software implementations. It is recommended to choose a server with DDOS defense when choosing a server, or directly use a layer-7 defense such as CloudFlare.
* **Scalable, pay-as-you-go**: The simplest example is that the network billing method is not based on bandwidth, but based on how much money per GB, unlimited bandwidth, which can avoid unnecessary expenses at the same time It will also allow users to enjoy the fastest speed. In the same way, the mode of charging according to the usage of system resources is the best choice. Generally, cloud-based PaaS is billed in this way, while IaaS needs to configure itself to upgrade the configuration when resources are insufficient, and recycle the configuration (or Auto Balancing) when resources are excessive. ) level billing The IaaS billing method is most suitable for scaling in this way to achieve pay-as-you-go.
* **Network reliability**: Network reliability is also very important. If the service network is not good, it will greatly affect the user experience and cause users to not be able to use the service.
* **SLA**: Important services require SLA. SLA can promise you the availability of the service. When the service fails to meet the promise, the service provider will compensate you.
* **IPv6**: For now, it's fine if IPv6 is not supported. But native IPv6 support is better than nothing, and with more and more operators supporting IPv6, using IPv6 may have lower network latency, and there are advantages to configuring IPsec.

## Other

+ **Domain name related**: Whether you choose to use a virtual host, VPS or a dedicated server, you usually need to buy a domain name yourself, configure DNS resolution, etc. The recommended free DNS resolution includes CloudFlare, Rage4, and domestic CloudXNS, some web hosting providers and domain name registrars also provide free resolution, but they are not as good as the ones introduced earlier ([For details about DNS, please see here](https://www.guozeyu.com/2016/05/rage4- best-dns/)). If you choose to use CloudFlare, then you can also choose to enable Proxy (CDN) in the DNS background, which can cache and speed up your website (but actually it may slow down for domestic purposes), and get some basic DDOS protection, And hide your origin IP address.
+ **Choice of domain name registrars**: First of all, to ensure that the total number of domain names is large; secondly, to have a complete range of domain name suffixes; thirdly, it is best to add free corporate email and advanced privacy protection; fourth, After the domain name is registered, it must be able to be deleted and refunded, so as to prevent the wrong registration from shaking hands; fifth, the price should not be too expensive. On the public domain name registration service [TlOxygen](https://domain.tloxygen.com/?promo=ze3kr) I made, the domain name registrars used are among the top ten global domain name registrars; there are more than 500 domain name suffixes , more than GoDaddy; gift business mailbox, you can configure unlimited forwarding mailboxes, even subdomain mailboxes, and support DKIM; most types of domain names can be refunded for deletion; the price is much cheaper than Godaddy, and the renewal price is not so pitiful , basically 30% off; domain name privacy protection is only 5 yuan, but the function is more advanced than many free ones: even if domain name privacy protection is turned on, when someone sends you an email, you will receive a form, and you can continue to fill in the form to contact you.

## Recommendations of Some Service Providers

The order in which they are listed is only relative to when they were added to this list, with new additions being placed at the end. The service providers added here are all recommended by individuals and will continue to be updated in the future. Annotation list:

* (6): Fully supports IPv6
* (6): Partial support for IPv6
* (D): Support free DDOS defense

### SaaS

#### Build a Website

* [WordPress.com](https://wordpress.com)
* [Tumblr](https://www.tumblr.coom) (6)
* [GitHub Pages](https://pages.github.com)

#### CDN

The following CDNs all support HTTPS and IPv6

* [UPYUN/You Paiyun](https://www.upyun.com) (6)
* [CloudFront](https://aws.amazon.com/cloudfront/) (6)
* [CloudFlare](https://www.cloudflare.com) (6, D)

#### DNS

* [Route 53](https://aws.amazon.com/route53/) (6)
* [Rage4](https://rage4.com) (6, D)

#### Data Storage

* [AWS S3](https://aws.amazon.com/s3/)

#### Code Hosting

* [GitHub](https://github.com)
* [GitLab.com](https://gitlab.com)
* [Coding](https://coding.net)

There are many more SaaS, too many things are SaaS, so here are only a few examples.

### PaaS

#### Virtual Host

* [BlueHost](https://www.bluehost.com/shared)

#### Cloud Hosting

Or a distributed virtual host, but with independent CPU and memory resources.

* [ResellerClub](https://www.resellerclub.com/cloud-hosting)
* [BlueHost](https://cloud.bluehost.com/products/cloud-sites/)

#### Custom Programming Language Hosting / Docker Container

* [Sina SAE](https://sae.sina.com.cn) (6¹, D)
* [OpenShift](https://www.openshift.com)
* [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/)
* [AWS EC2 Container Registry](https://aws.amazon.com/ecr/)

### IaaS

#### VPS/Cloud

Annotation list:

* (U): Exclusive CPU³
* (C): Cloud-based technology, high availability
* (S): No billing after shutdown

* [Vultr](https://www.vultr.com/?ref=6886257) (6, D⁶)
* [OVH](https://www.ovh.com) (6², D, C⁴)
* [Google Compute Engine](https://cloud.google.com/compute/) (D⁵, C, U, S)
* [Linode](https://www.linode.com) (6)
* [DigitalOcean](https://www.digitalocean.com) (6)
* [AWS EC2](https://aws.amazon.com/ec2/) (S)
* [QingCloud/Qingyun](https://www.qingcloud.com) (S)
* [Tencent Cloud](https://www.qcloud.com)

### Remark

1. SAE's IPv6 activation requires additional monthly payment
2. OVH's SSD VPS does not currently support IPv6
3. When purchasing a certain configuration, if you are told to share the CPU, then share the CPU
4. OVH only Cloud VPS is cloud-based VPS
5. Google does not officially says that they provide DDOS protect, but it is actually capable of that
6. Vultr's DDOS defense requires additional purchase, and is only supported in some regions, and the maximum defense is only 10Gbps
