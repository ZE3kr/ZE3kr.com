---
title: Azure DNS, NS1, Constellix, Comparison of Three International GeoDNS Service Providers
tags:
  - CDN
  - DNS
  - Internet
id: '3718'
categories:
  - - Essay
date: 2018-11-15 11:45:54
languages:
  zh-CN: https://guozeyu.com/2018/11/azure-dns-ns1-constellix-compare/
---

Recently DNSPod's resolver server [was down for a while](https://www.ithome.com/0/394/009.htm), causing many DNSPod users' websites to be inaccessible. This article will recommend several international GeoDNS services that provide 100% SLA and can be used to replace the unstable DNSPod. And describe ways to use multiple DNS providers to improve service availability.

This article includes a comprehensive comparison of Azure DNS, NS1, Constellix.
<!-- more -->

## Introduction

The three DNS recommended this time are all international service providers that support GeoDNS, and all support Anycast. Among them, the speed of Azure DNS and NS1 is very good in China. It is a direct route to Hong Kong/Asia. The average delay can be within 50ms, which is not inferior to domestic DNS service providers. The international speeds of these three companies are extremely fast, and they can kill domestic providers such as DNSPod, CloudXNS, and Alibaba Cloud Analytics.

Regarding GeoDNS, Constellix has the best granularity: it supports countries, provinces, cities (including Chinese provinces and cities), and even AS numbers (which can be resolved by operator) and IP segments. NS1 supports countries and states in the United States, as well as AS numbers and IP segments. As for Azure DNS, by using Traffic Manager, countries, states in some countries, and IP segments can be supported. Since all IP segments are supported, GeoDNS with arbitrary precision is theoretically supported.

As for the price, NS1 provides a free quota (500k requests per month), which is enough for low-traffic websites, but once this free quota is exceeded, the charges are very high: **$8/million requests** . In comparison, both Azure DNS and Constellix are cheap, with Constellix starting at **$5/month**.

## Using multiple DNS providers

It is possible to use multiple DNS service providers at the same time, as long as the DNS records of the service providers remain the same. This requires the service provider used to be able to configure the NS record for the primary domain name (recommended but not required). The Azure DNS, NS1, Constellix introduced this time, as well as the previously introduced Route 53, Google Cloud DNS, Rage4, and Alibaba Cloud DNS can configure the NS record of the primary domain name (where Alibaba Cloud DNS and Azure DNS can only add the first 3rd party records; several others can fully customize NS records). However, Cloudflare and domestic DNSPod, CloudXNS, and Alibaba Cloud DNS cannot configure NS records under the primary domain name, which means that you cannot mix DNS well.

To use multiple DNS, there are two implementation methods: the master and slave method and the two master method. After completing the configuration, configure multiple NS servers to authoritative NS records and NS lists under the domain name registrar.

Using two service providers will increase the difficulty of configuration, but can improve the stability of the DNS service. such as the recent [DNSPod outage](https://www.ithome.com/0/394/009.htm) and [Dyn outage in 2016](https://en.wikipedia.org/wiki/2016_Dyn_cyberattack ) causes many websites to become inaccessible because many websites use only one DNS provider. If multiple DNS providers are used, the website will only become inaccessible if all of the providers used are down, which is obviously very unlikely.

### Master and slave service providers

To configure this way, the secondary provider needs to support fetching records from the primary provider using AXFR, and the primary provider also needs to support AXFR transfers. Among them, both NS1 and Constellix can be used as the main service provider or the secondary service provider, which means that you can use these two at the same time, and choose any one as the primary service provider and the other as the secondary service provider.

### Two main service providers

You can also use two primary service providers and keep all records (including NS records under the primary domain) the same (recommended, but not required). It is recommended to synchronize the SOA's serial number as well.

### Example

The domain name `github.com.` uses both Route 53 and DYN services. This can be verified using the dig tool.

```
$ dig github.com ns +short
ns1.p16.dynect.net.
ns2.p16.dynect.net.
ns3.p16.dynect.net.
ns4.p16.dynect.net.
ns-1283.awsdns-32.org.
ns-421.awsdns-52.com.
ns-1707.awsdns-21.co.uk.
ns-520.awsdns-01.net.
```

After inspection, the same records are also configured in the two DNS service providers.

```
$ dig @ns1.p16.dynect.net.github.com a +short
13.229.188.59
13.250.177.223
52.74.223.119
$ dig @ns-1283.awsdns-32.org.github.com a +short
13.229.188.59
13.250.177.223
52.74.223.119
```

* * *

Let’s take a look at these three DNS providers one by one.

[List of all DNS evaluations](https://wiki.tloxygen.com/DNS_provider) (also includes CloudXNS, Route 53, Cloudflare, Google Cloud DNS, Rage4, and Alibaba Cloud DNS)

## Azure DNS

DNS service under the Microsoft Azure product line. It uses Anycast technology and can be directly connected to Hong Kong/Asia nodes in China, so the speed is very fast.

It is worth noting that, similar to Route 53, the four servers allocated by Azure use different network segments and different circuits, which may have higher availability.

Note: The partition resolution of Azure's DNS may not be compatible with IPv6, which means that the resolution result may be Fallback to the default line.

* Foreign speed: ★★★★☆, 36 ms
* North America speed: ★★★★☆, 27 ms
* Asia speed: ★★★★☆, 39 ms
* European speed: ★★★★☆, 29 ms
* Domestic speed: ★★★★☆, 49 ms
* Minimum TTL: **0s**
* Domestic partition analysis: ★★★★★, support configuration to China, support IP segment configuration (with Traffic Manager)
* Analysis of foreign partitions: ★★★★★, support configuration to big states, countries and states in some countries, support IP segment configuration (with Traffic Manager)
*DNSSEC: **Not supported**
*IPv6: **Support**
* Record Type: Support A, AAAA, CNAME, NS, MX, TXT, SRV, CAA, PTR.
* Root domain CNAME optimization: **Not supported**
* Priority: **Support** (with Traffic Manager)
* Custom NS: **Only supports adding additional NS**
* Price: $0.5/month per domain, **$0.4/million requests**
* Price for use case A: **$0.90**
* Price for use case B: **$10.50**
* SLA: 100%

## NS1

NS1 also uses Anycast technology, and can be directly connected to Hong Kong/Asia nodes in China, so the speed is very fast.

* Foreign speed: ★★★★★, 23 ms
* North America speed: ★★★★★, 9 ms
* Asia speed: ★★★★☆, 40 ms
* European speed: ★★★★★, 20 ms
* Domestic speed: ★★★☆☆, 65 ms
* Minimum TTL: **0s**
* Domestic partition analysis: ★★★★★, support configuration to China, support IP segment configuration.
* Analysis of foreign partitions: ★★★★★, support configuration to big states, countries, states in the United States, support AS number, IP segment configuration.
*DNSSEC: **Support**
*IPv6: **NOT SUPPORTED**
* Record Type: Support A, AAAA, AFSDB, CAA, CERT, CNAME, DS, HINFO, MX, NAPTR, NS, PTR, RP, SPF, SRV, TXT.
* Root domain CNAME optimization: **support**
* Priority: **Support** (with Traffic Manager)
* Custom NS: **Only supports adding additional NS**
* Price: Free for the first 500k requests per month, beyond **$8.0/million requests**
* Price for use case A: **$4**
* Price for use case B: **$156**
* Price for use case C: **$156**
* SLA: 100%

## Constellix

This DNS, known for its GeoDNS advantages, uses Anycast to guarantee the lowest latency.

Constellix has three groups of six DNS servers, each using a slightly different line.

* Foreign speed: ★★★★☆, 31.51 ms
* North America speed: ★★★★★, 9.81 ms
* Asia speed: ★★★★☆, 48.87 ms
* European speed: ★★★★★, 23.77 ms
* Domestic speed: ★★☆☆☆, 108.9 ms
* Minimum TTL: **0s**
* Domestic partition analysis: ★★★★★, which can be accurate to every province and city, and ASN can be configured to achieve operator partition analysis.
* Analysis of foreign divisions: ★★★★★, accurate to each country, province, city
*DNSSEC: **Not supported**
*IPv6: **Support**
* Record type: **more** complete, only supports A, AAAA, CNAME, NS, MX, TXT, SRV, HINFO, NAPTR, CAA, CERT, PTR, RP, SPF.
* Root domain CNAME optimization: **support**
* Priority: **Support**
* Custom NS: **Support**
* Price: **$5/month** for the first domain name, **$0.5/month** for each domain name thereafter. **$0.4/million requests**. Partition resolution $0.6/million requests
* Price for use case A: **$5.39**
* Price for use case B: **$14.90**
* Price for use case C: **$19.30**
* Statistics function: **Support**, you can see the number of requests for each country and city. You can even enable logging to see the client IP, IP packet type, and more for every request.
* SLA: 100%