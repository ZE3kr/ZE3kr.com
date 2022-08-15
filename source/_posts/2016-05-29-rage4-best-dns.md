---
title: Comprehensive Comparison of DNS Services such as CloudXNS, Route 53, and Alibaba Cloud DNS
tags:
  - DNS
  - Website
  - Internet
id: '1668'
categories:
  - - Technology
date: 2016-05-29 14:43:00
languages:
  zh-CN: https://www.guozeyu.com/2016/05/rage4-best-dns/
---

DNS ([Domain Name System](https://en.wikipedia.org/wiki/Domain Name System)) is a service of the Internet. It can point the domain name to an IP (server), so that you can access a website through the domain name. A website that can be accessed through a domain name requires a DNS server. This refers to the authoritative DNS used for the webmaster's domain name, not the cached DNS. This article includes a comprehensive comparison of CloudXNS, Route 53, Cloudflare, Google Cloud DNS, Rage4, and Alibaba Cloud DNS.
<!-- more -->

## CloudXNS

**Note**: CloudXNS does not support TCP.

The best domestic free DNS, as a DNS service, its functions are also complete. There are quite a few CloudXNS servers in China, but they do not use Anycast technology, so it is useless to talk about any foreign servers.

CloudXNS uses GeoDNS to partition and resolve the domain names of its NS servers, domestic ones are resolved to domestic servers, and foreign ones are resolved to foreign servers. However, four of the Glue Records of its root domain name are still four domestic servers. In actual analysis, Glue Records will be used first, so no foreign servers will be used.

Why not add these foreign servers to Glue Record too? Because Glue Record does not support GeoDNS, the resolver will randomly select the server, so if this is the case, some domestic requests will also go to the US server, which will slow down.

* Foreign speed: ★☆☆☆☆, 248 ms
* North America speed: ★☆☆☆☆, 272 ms
* Asia speed: ★★☆☆☆, 196 ms
* European speed: ★☆☆☆☆, 283 ms
* Domestic speed: ★★★★☆, 32 ms
* Minimum TTL: **60s**
* Domestic partition analysis: ★★★★★, accurate to most operators and provinces
* Analysis of foreign divisions: ★★☆☆☆, accurate to continents and some countries, but not cities
*DNSSEC: **Not supported**
*IPv6: **NOT SUPPORTED**
* Record type: **Basically** complete, only supports A, AAAA, CNAME, NS, MX, TXT, SRV.
* Root domain CNAME optimization: **Not supported**
* Priority: **Support**, you can configure the priority of parsing to different servers
* Custom NS: **Not supported**, because it does not support modifying SOA and NS under the root domain name, so this is impossible
* Price: **Free, charge by function**
* Price for use case A: **Free**
* Use Case B Price: **Free**
* Use Case C Price: **Free**
* Statistics function: **support**, can be accurate to country, province, operator
*SLA: 99.9%. (It will be reduced to 98% when the protection peak value or the monthly resolution volume of the account is exceeded)

## Route 53

The DNS services that are quite popular in foreign countries have all the necessary functions. Servers are located all over the world and use Anycast to guarantee the lowest latency.

* Foreign speed: ★★★☆☆, 84 ms
* North America speed: ★★★☆☆, 61 ms
* Asia speed: ★★★☆☆, 79 ms
* European speed: ★★★☆☆, 82 ms
* Domestic speed: ☆☆☆☆☆, 328 ms
* Minimum TTL: **0s**
* Domestic partition analysis: ★★½☆☆, which can only be set separately for this region of China, and does not support provinces and operators. If you use regional intelligent resolution, you can set up two servers that are biased towards Ningxia and Beijing.
* Analysis of foreign divisions: ★★★★★, accurate to each country, many countries are also accurate to the province/city
*DNSSEC: **Not supported**
*IPv6: **Support**
* Record type: **more** complete, support A, AAAA, CNAME, NS, MX, TXT, SRV, PTR, SPF, NAPTR, CAA.
* Root domain CNAME optimization: **Not supported**
* Priority: **Support**
* Custom NS: **support**, and you can also modify SOA and NS under the root domain name
* Price: **$0.5/month** per domain name, **$0.4/million** requests, partition resolution $0.7/million requests
* Price for use case A: **$0.90**
* Price for use case B: **$10.50**
* Price for use case C: **$16.50**
* Statistics function: basic **not supported**, only seen in the monthly final bill
*SLA: 100%

## Cloudflare

There is a considerable amount of free DNS abroad, with servers all over the world, using Anycast to ensure the lowest latency.

* Foreign speed: ★★★★★, 13 ms
* North America speed: ★★★★★, 10 ms
* Asia speed: ★★★★☆, 30 ms
* European speed: ★★★★★, 8 ms
* Domestic speed: ★★☆☆☆, 193 ms
* Minimum TTL: **120s**
* Domestic partition analysis: ☆☆☆☆☆, domestic partition analysis is not supported at all, and domestic requests are generally regarded as the West Coast of the United States.
* Foreign partition analysis: ★★★☆☆, if you purchase the Load Balancing service, you can configure partition analysis according to different regions. Although it is not differentiated by country and city, it is sometimes more refined than country (for example, it supports partition analysis for different regions in North America, east, west, and central regions)
* DNSSEC: **support**, also supports DNSSEC-specific records, including SSHFP, TLSA, DNSKEY, DS
*IPv6: **Support**
* Record type: **more** complete, support A, AAAA, CNAME, TXT, SRV, LOC, MX, NS, SPF, CERT, NAPTR, SMIMEA, URI
* Root domain CNAME optimization: **support**
* Priority: Support, need to purchase Load Balancing service
* Custom NS: **NOT SUPPORTED**, only supported after purchasing the $200/month Business version
* Price: **Free**
* Price for use case A: **Free**
* Use Case B Price: **Free**
* Price for use case C: **$20.00**
* Statistics function: Partial **support**, free users can see limited statistics
*SLA: None (Business and Enterprise editions are 100% SLA guaranteed)

## Google Cloud DNS

Inexpensive, offers a 100% SLA, and uses Anycast to guarantee the lowest latency.

* Foreign speed: ★★★☆☆, 60 ms
* North American speed: ★★★☆☆, 53 ms
* Asia speed: ★★★☆☆, 95 ms
* European speed: ★★★★☆, 30 ms
* Domestic speed: ★★☆☆☆, 171 ms
* Minimum TTL: **0s**
* Partition analysis: ☆☆☆☆☆, **Not supported**
* DNSSEC: **support**, also supports DNSSEC-specific records, including IPSECKEY, SSHFP, TLSA, DNSKEY, DS
*IPv6: **Support**
* Record Types: **Almost Complete**, supports A, AAAA, CNAME, NS, MX, TXT, SRV, SPF, LOC, NAPTR, PTR, CAA, and the DNSSEC related records listed above.
* Root domain CNAME optimization: **Not supported**
* Custom NS: **Support**
* Price: **$0.2/month** per domain, **$0.4/million requests**.
* Statistics function: basic **not supported**, only seen in the monthly final bill
*SLA: 100%

## Rage4

Both DNSSEC and partition resolution are supported, and Anycast is used to ensure the lowest latency.

* Foreign speed: ★★★★☆, 30 ms
* North American speed: ★★★★★, 18 ms
* Asia speed: ★★★☆☆, 59 ms
* European speed: ★★★★☆, 26 ms
* Domestic speed: ★★☆☆☆, 153 ms
* Minimum TTL: **Depends on the package**
* Domestic partition analysis: ★★☆☆☆, can only be configured for East Asia and China
* Foreign partition analysis: ★★★★☆, partition analysis can be configured according to different regions
* DNSSEC: **Support**
* IPv6: **Support**
* Record types: **more** complete, support SOA, NS, A, AAAA, CNAME, TXT, MX, SRV, PTR, SPF, SSHFP, TLSA, LOC, NAPTR
* Root domain CNAME optimization: **support**
* Priority: **Support**
* Custom NS: **support**, and you can also modify SOA and NS under the root domain name
* Price: From **€2/month** per domain, priced based on functionality rather than usage
* Price for use case A: **€2**
* Price for use case B: **€10**
* Price for use case C: **€100**
* Statistics function: **support**, can be accurate to the country
* SLA: 99.99%

## Alibaba Cloud DNS

For Alibaba Cloud's products, all analysis servers use Alibaba Cloud's computer room, which guarantees speed. Charges are based on features used, not resolution. There are many servers in China, but they do not use Anycast technology, so the speed abroad is very poor. Speed ​​benchmark:

ns1.alidns.com. 106.11.141.111

The Aliyun paid version is different from the free version. The paid version includes overseas lines, but because there is no Anycast, the final line is randomly selected. Therefore, compared with the free version, the paid version has a slightly shorter response time abroad. The domestic analysis time is slightly longer. **Note**: Alibaba Cloud DNS does not support TCP.

* Foreign speed: ★☆☆☆☆, 238 ms
* North America speed: ★☆☆☆☆, 229 ms
* Asia speed: ★★☆☆☆, 186 ms
* European speed: ★☆☆☆☆, 251 ms
* Domestic speed: ★★★★☆, 33 ms
* Shortest TTL: Free plan: **600s**; Paid plan: 1~120s
* Domestic partition analysis: ★★★★★, the free version supports operators, and the paid version supports regional analysis
* Analysis of foreign partitions: ★★★☆☆, the free version only supports overseas, and the paid version can support continents and countries
* DNSSEC: **Not supported**
* IPv6: **NOT SUPPORTED**
* Record Type: Support A, AAAA, CNAME, NS, MX, TXT, SRV. The CAA is only for the paid version.
* Root domain CNAME optimization: **Not supported**
* Priority: **Support**
* Custom NS: **Not supported**
* Price: **Free, charge by function**
* Price for use case A: **Free**
* Use Case B Price: **Free**
* Use Case C Price: **Free**
* Statistics: **Paid version only**
* SLA: 99.95%
