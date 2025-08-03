---
title: Self-built PowerDNS GeoDNS Server
tags:
  - DNS
  - VPS
id: '1832'
categories:
  - - Development
date: 2016-08-03 09:24:21
languages:
  zh-CN: https://www.guozeyu.com/2016/08/self-host-dns/
---

Lately I've grown more and more into building things myself, like GitLab. Today, I changed the DNS server to a self-built one, and shared my experience (PS: Now in order to realize the [root domain CDN](https://www.guozeyu.com/2017/01/wordpress-full-site-cdn/), I used Route 53 instead):

The self-built DNS in this article refers to the authoritative DNS, that is, the DNS configured for your own domain name, not the cache DNS configured on the client.

## Advantages and Disadvantages

First of all, let me talk about the fatal disadvantages of using a self-built DNS server:

1. If your server is down that day, the entire domain name related services will be down. Even if your mail receiving server is a third-party, you will not be able to receive mail.
2. Basically, it must be a VPS with an open port and a **fixed IP** (and preferably at least two IPs) (of course it can be two hosts, just make sure that the configuration files are exactly the same), High requirements for service providers
3. Individuals generally have insufficient experience in DNS operation and maintenance, which can easily lead to configuration errors
4. Third-party DNS providers basically have DDOS defense, but your server may not have it. An attacker can directly attack your server through L7 DNS Flood, and then go back to the first problem.

Advantages of using a self-built DNS server:
<!-- more -->

1. The degree of DIY is extremely high, almost all DNS functions can be configured, but they are all very complicated
2. Can be built on an existing server without extra cost

In the end, I chose to use PowerDNS software (which is actually used by many service providers that provide DNS services). I installed its recently released version 4.0. Some features supported by this version:

* EDNS Client Subnet
* DNSSEC
*GEODNS
* IPv6

Wait, that's just what came to my mind. At the same time, PowerDNS supports many types of resolution records (at least the most I have seen so far): A, AAAA, AFSDB, ALIAS (also ANAME), CAA, CERT, CDNSKEY, CDS, CNAME, DNSKEY, DNAME, DS, HINFO, KEY, LOC, MX, NAPTR, NS, NSEC, NSEC3, NSEC3PARAM, OPENPGPKEY, PTR, RP, RRSIG, SOA, SPF, SSHFP, SRV, TKEY, TSIG, TLSA, TXT, URI, etc., and there are no columns that are not commonly used out, see [all supported records](https://doc.powerdns.com/md/types/). To be honest, there are some unpopular records that many resolvers do not support, but I need to use them, such as LOC, SSHFP and TLSA. Don't know what this pile of records is for? See [Wikipedia](https://en.wikipedia.org/wiki/List_of_DNS_record_types).

## Briefly Describe the Installation Process

For detailed installation method [see official documentation](https://doc.powerdns.com/md/authoritative/installation/), you need to install `pdns-server` first, and then install `pdns-backend-$backend` . Backend is something you can choose by yourself. Commonly used ones are `BIND` and `Generic MySQL`. If you need GEODNS, you can use `GEOIP`. All lists [see here](https://doc.powerdns.com/md/authoritative/) . If you want to control the background of the web version, it may be more convenient to use MySQL. Both BIND and GEOIP are fine if it is only controlled by file. I use the GEOIP version. The GEOIP version is highly scalable and uses YAML files, which is more flexible and elegant. This article will talk about the GEOIP version: Install on Ubuntu (available in the system software source):

$ sudo apt install pdns-server
$ sudo apt install pdns-backend-geoip

Then modify the configuration file:

```
$ rm /etc/powerdns/pdns.d/* # delete Example
```

### Install a Newer Version of PowerDNS

Many features, such as CAA records, require the new version of PowerDNS. Please [go to the official website to configure the software source](https://repo.powerdns.com).

### Install Geolocation Database

Note that you should already have the MaxMind GeoIP Lite database, if not, install it as follows:

> Important update⚠️: Since April 1, 2018, the GeoIP database in DAT format cannot be automatically downloaded by the software. Please [go to the official website to manually download the corresponding database](https://dev.maxmind.com/geoip/legacy/geolite /). It needs to be in Binary format.

Create a file `/etc/GeoIP.conf` with:

```
# The following UserId and LicenseKey are required placeholders:
UserId 999999
LicenseKey 000000000000
# Include one or more of the following ProductIds:
# * GeoLite2-City - GeoLite 2 City
# * GeoLite2-Country - GeoLite2 Country
# * GeoLite-Legacy-IPv6-City - GeoLite Legacy IPv6 City
# * GeoLite-Legacy-IPv6-Country - GeoLite Legacy IPv6 Country
# * 506 - GeoLite Legacy Country
# * 517 - GeoLite Legacy ASN
# * 533 - GeoLite Legacy City
ProductIds 506 GeoLite-Legacy-IPv6-Country
DatabaseDirectory /usr/share/GeoIP
```

Then install geoipupdate, execute `sudo apt install geoipupdate && mkdir -p /usr/share/GeoIP && geoipupdate -v` , your database has been downloaded.

### Configure PowerDNS

Create a file `/etc/powerdns/pdns.d/geoip.conf` with:

```
launch=geoip
geoip-database-files=/usr/share/GeoIP/GeoLiteCountry.dat /usr/share/GeoIP/GeoIPv6.dat # Select IPv4 and IPv6 country modules
geoip-database-cache=memory
geoip-zones-file=/share/zone.yaml # The location of your YAML configuration file, anywhere
geoip-dnssec-keydir=/etc/powerdns/key
```

Create that YAML file, and then start writing Zone, here is an example (IPv6 is not required, all IPs should be filled in with external IPs, this article takes the country as an example, the order of the juxtaposed content does not matter):

```
# @see: https://doc.powerdns.com/md/authoritative/backend-geoip/
domains:
- domain: example.com
  ttl: 300 # Default TTL duration
  records:
##### Default NS
    ns1.example.com:
      - a: # Your server's first IPv4 address
          content: 10.0.0.1
          ttl: 86400
      - aaaa: # Your server's first IPv6 address
          content: ::1
          ttl: 86400
    ns2.example.com: # Second IPv4 address of your server (if not, same as above)
      - a:
          content: 10.0.0.2
          ttl: 86400
      - aaaa: # Second IPv6 address of your server (if not, same as above)
          content: ::2
          ttl: 86400
##### Root domain
    example.com: # records under the root domain name
      - soa:
          content: ns1.example.com.admin.example.com.1 86400 3600 604800 10800
          ttl: 7200
      - ns:
          content: ns1.example.com.
          ttl: 86400
      - ns:
          content: ns2.example.com.
          ttl: 86400
      - mx:
          content: 100 mx1.example.com. # weight [space] hostname
          ttl: 7200
      - mx:
          content: 100 mx2.example.com.
          ttl: 7200
      - mx:
          content: 100 mx3.example.com.
          ttl: 7200
      - a: 103.41.133.70 # If you want to use the default TTL, you don't need to distinguish between the content and ttl fields
      - aaaa: 2001:470:fa6b::1
##### Servers list Your server list
    beijing-server.example.com: &beijing
      - a: 10.0.1.1
      - aaaa: ::1:1
    newyork-server.example.com: &newyork
      - a: 10.0.2.1
      - aaaa: ::2:1
    japan-server.example.com: &japan
      - a: 10.0.3.1
      - aaaa: ::3:1
    london-server.example.com: &uk
      - a: 10.0.4.1
      - aaaa: ::4:1
    france-server.example.com: &france
      - a: 10.0.5.1
      - aaaa: ::5:1
##### GEODNS partition resolution
    # @see: https://php.net/manual/en/function.geoip-continent-code-by-name.php
    # @see: https://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-3
    # unknown also is default
    # %co.%cn.geo.example.com
    # default
    unknown.unknown.geo.example.com: *newyork # resolves to US by default
    # continent
    unknown.as.geo.example.com: *japan # Asia to Japan
    unknown.oc.geo.example.com: *japan # Oceania resolves to Japan
    unknown.eu.geo.example.com: *france # Europe resolves to France
    unknown.af.geo.example.com: *france # Africa to France
    # nation
    chn.as.geo.example.com: *beijing # China parsing Beijing
    gbr.eu.geo.example.com: *uk # UK resolves to UK
  services:
    #GEODNS
    www.example.com: [ '%co.%cn.geo.example.com', 'unknown.%cn.geo.example.com', 'unknown.unknown.geo.example.com']
```

This configuration is equivalent to parsing www.example.com to the partition. Due to some problems in this parsing at present, GEODNS cannot be set under the root domain name and subdomain at the same time. I have submitted feedback on this bug. (https://github .com/PowerDNS/pdns/issues/4276). If you want to only set the parsing precision at the continent level, just write %cn.geo.example.com one less level. If you need to be accurate to the city, you can write one more level, but you need to add the GeoIP city database to the configuration file. However, the city version of the free city database is not accurate, and you also need to purchase a commercial database, which is an additional cost.

### Configure the Domain Name

Go to your domain name registrar, enter the background to modify the settings, and add a subdomain name server record to the domain name, as shown in the figure:

<img src="https://cdn.tlo.xyz/images/09ca1def-0fc9-43ba-13d5-befca09c7900/extra" alt="Add subnameserver record" width="1406" height="892"/>

Since the NS to be set is under your own server, you must register your NS server IP address with the upper-level domain name (such as .com) on the domain name registrar, so that the upper-level domain name can resolve the IP of the NS and build your own DNS For example, there is a NS of its own under icann.org:

```
$ dig icann.org ns +short
a.iana-servers.net.
b.iana-servers.net.
c.iana-servers.net.
ns.icann.org.
```

Then look at its upper-level domain name org:

```
$ dig org ns +short
a2.org.afilias-nst.info.
b0.org.afilias-nst.org.
d0.org.afilias-nst.org.
c0.org.afilias-nst.info.
a0.org.afilias-nst.info.
b2.org.afilias-nst.org.
```

Find any server and query the authoritative record (I don't need +short ):

```
$ dig @a0.org.afilias-nst.info icann.org ns
;; QUESTION SECTION:
;icann.org.INNS
;; AUTHORITY SECTION:
icann.org.86400INNSc.iana-servers.net.
icann.org.86400INNSa.iana-servers.net.
icann.org.86400INNSns.icann.org.
icann.org.86400INNSb.iana-servers.net.
;; ADDITIONAL SECTION:
ns.icann.org.86400INA199.4.138.53
ns.icann.org.86400INAAAA2001:500:89::53
```

It can be seen that the NS server in this org has already returned the records of ns.icann.org. This is why you need to fill in the IP address in the domain name registrar. However you'd better return the same NS and the same IP on the DNS server under your domain name as well. Finally, don't forget to change the NS record of the domain name.

### Some Advanced Writing of YAML

In the YAML I just used, I have actually used the advanced YAML writing method, that is, `&variable` sets variables, `*variable` uses variables, which is very similar to the LINK record under CloudXNS, for example, under CloudXNS you can write:

```
www.example.com 600 IN A 10.0.0.1
www.example.com 600 IN A 10.0.0.2
www.example.com 600 IN AAAA ::1
www.example.com 600 IN AAAA ::2
sub.example.com 600 IN LINK www.example.com
```

Then in your YAML record you can write:

```
www.example.com: &www
  - a: 10.0.0.1
  - a: 10.0.0.2
  - aaaa: ::1
  - aaaa: ::2
sub.example.com: *www
```

This is a high-level way of writing YAML without any additional support.

### Add DNSSEC support

For details, you can [Reference Documentation](https://doc.powerdns.com/md/authoritative/dnssec/), run the following command:

```
$ mkdir /etc/powerdns/key
$ pdnsutil secure-zone example.com
$ pdnsutil show-zone example.com
```

The result returned by the last command is the record you need to set at the domain name registrar. It is not recommended to set all of them. Just set ECDSAP256SHA256 - SHA256 digest. Finally, check the settings online [Test address 1](http://dnssec-debugger.verisignlabs.com/) [Test address 2](http://dnsviz.net), there may be a few days of caching time. My inspection [results](http://dnsviz.net/d/guozeyu.com/dnssec/)

### Some Other Interesting Stuff

You can write this in YAML, to make it easier for you to debug:

```
"*.ip.example.com":
  - TXT:
      content: "IP%af: %ip, Continent: %cn, Country: %co, ASn: %as, Region: %re, Organisation: %na, City: %ci"
      ttl: 0
```

These variables can be used as your GEODNS criteria, as well as checking your GEOIP database. Then, check the posture correctly:

```
$ random=`head -200 /dev/urandom md5` ; dig ${random}.ip.example txt +short
"IPv4: 42.83.200.23, Continent: as, Country: chn, ASn: unknown, Region: unknown, Organisation: unknown, City: unknown"
```

The IP address is the DNS cache server address (if you enable EDNS Client Subnet and the cache server supports it, then it is your own IP, but if you use 8.8.8.8, you will see that the last digit of your IP is 0), if you If you specify to check from your own server locally, then return your own IP address directly. Since I only have the country database installed, everything is Unknown except for the continent and country.

## Advanced Use

### Establish Distributed DNS

In general, it is a DNS resolution server of a Master and a Slave, but in this case, there may be problems with DNSSEC, so I set up two Master servers, automatically synchronize records, and set **same DNSSEC Private Key** , nothing seems to go wrong (after all, all records, including SOA, are exactly the same), my server's current configuration

```
$ dig @a.gtld-servers.net guozeyu.com
;; QUESTION SECTION:
;guozeyu.com.INA
;; AUTHORITY SECTION:
guozeyu.com.172800INNSa.geo.ns.tloxygen.net.
guozeyu.com.172800INNSc.geo.ns.tloxygen.net.
;; ADDITIONAL SECTION:
a.geo.ns.tloxygen.net.172800INA198.251.90.65
a.geo.ns.tloxygen.net.172800INAAAA2605:6400:10:6a9::2
c.geo.ns.tloxygen.net.172800INA104.196.241.116
c.geo.ns.tloxygen.net.172800INAAAA2605:6400:20:b5e::2
```

Among them are two IPv4 and two IPv6, of which a.geo.ns.tloxygen.net. is an IP address using Anycast technology, which is provided by three servers behind it. c.geo.ns.tloxygen.net. belongs to the host of another service provider, so there is a backup after it hangs, which is more stable.

#### Anycast or Unicast?

A distributed DNS like mine is actually a combination of Unicast and Anycast. One problem is that connecting to one of them in one place will be faster, but the other will be slower. Only with a DNS cache server that supports asynchronous query or with GeoIP, it is possible to connect to the fastest DNS authoritative server, otherwise it is a random connection, and if a server hangs, then the corresponding IP of the server is obsolete. Anycast is an IP corresponding to multiple hosts, but I have no conditions to use it. This may cost more for individuals. Either you have an AS number and let the host provider give you access, or your host provider provides cross-regional access. Load Balancing IP. My VPS is in two different hosting companies, and I don't have AS, so I can't use Anycast. I think Anycast should be used for DNS services, because the IP corresponding to the DNS server cannot be GEODNS (because this is the root domain name that is resolved for you). After using Anycast, the fastest connection rate can basically be guaranteed, and a server has an IP address. Still usable. Additionally, DNS must forward both TCP and UDP port 53.

### Automatically Switch when Downtime

How to realize automatic switchover when downtime? The process to achieve this is: Monitoring service finds downtime -> Sends a downtime request to the server -> The server processes the downtime, resolves to an alternate IP/pauses parsing monitoring service finds that the service is normal -> Sends a normal service request to the server -> The server processes the service normally, and the recovery analysis can create two YAML files, one is used by default, and the other is used when the server is down. When the monitoring service finds that the server is down, reload another YAML file, and then this is Downtime mode.
