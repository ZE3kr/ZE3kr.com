---
title: Google Compute Engine Beginner Tutorial and Experience
tags:
  - Google Cloud Platform
  - VPS
  - Internet
id: '1933'
categories:
  - - Development
date: 2016-10-01 11:00:48
languages:
  zh-CN: https://guozeyu.com/2016/10/asia-google-compute-engine/
---

The process of creating a new GCE is very simple. You only need to customize the configuration, select the operating system, configure the SSH Key, and then select Create. The whole process is very similar to VPS, but the customizable functions are far more than VPS.
<!-- more -->

## Price and Configuration

[Please refer to the official price list for specific prices](https://cloud.google.com/compute/pricing). (Due to the continuous use discount, the actual monthly price is lower than the hourly price) The price of GCE is relatively affordable, and the minimum configuration (f1-micro) 1 shared core-0.6 GB memory-10GB HDD costs less than $5 per month, And because the CPU, memory size and disk size are adjustable, you can buy the most suitable one according to your needs, which can save unnecessary overhead. And for some computer rooms in North America, the first minimum (f1-micro) instance of the account can enjoy [permanent free quota](https://cloud.google.com/free/?hl=zh-cn), for In terms of building a website (and using it with Cloudflare), it is still very cost-effective. (In addition, you can directly [use CNAME to access Cloudflare](https://cf.tlo.xyz), so you don't need to change the DNS server.) For traffic, for all available zones, **connect to mainland China $0.23/Gbyte* *, US and Europe $0.12/Gbyte, the price of traffic is a little expensive, but if it is connected to Google's own services (including but not limited to Gmail, YouTube), the traffic is not billed (but the traffic is bidirectional, so it is local through GCE upload is completely free, download is still the original price). Another special thing about GCE is that it is billed by the minute, and when the service is in a terminated state (equivalent to shutdown, disk data retention), there is no charge (except for a small amount of disk usage). Every time you calculate Uptime, if it is less than 10 minutes, it will be counted as ten minutes. After more than 10 minutes, it will be billed by minutes, but it is still very cost-effective.

### Supplements on the Example of Shared Cores

The f1-micro (0.6 GB) and g1-small (1.7GB) versions use shared cores (the rest of the configurations are independent cores), and according to Google, 0.60GB is 0.2 vCPU and 1.70GB is 0.5 vCPU. However, it supports Bursting, which means that it can use up to 1.0 vCPU in a short time. So what is 1.0 vCPU? Check cpuinfo, it is Intel(R) Xeon(R) CPU @ 2.50GHz. That is to say, these two versions can occupy up to 2.5GHz. But if used for a long time, the speed will be compressed to 0.5GHz and 1.75GHz.

![Monitor Image](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/a35ea803-7cb3-4000-38fc-e8b80dcf7400/large)

I installed monitoring software on my f1-micro, and compared the CPU usage given by GCE (dark blue) and the system itself monitored usage (light blue), and found that the CPU usage on the GCE chart is exactly 5 of the local statistics. That is to say, if the CPU usage seen locally is 20%, the GCE chart shows exactly 100%, the local is 20~100%, and the GCE chart is 100~500%, then it is called Bursting. Compared with other VPS, other VPS are almost all shared core, but you can't judge whether it is oversold. For example, if there are 10 users sharing a core, if those 10 people are constantly occupying the CPU, then your CPU speed will be lower than one-tenth of a single core. And Google's shared core guarantees a minimum speed (0.2 vCPU and 0.5 vCPU), even if other users use it hard, it can still guarantee you a certain speed.

## Usage Process and Configuration Method

First, you need to go to the [Create Instance](https://console.cloud.google.com/compute/instancesAdd) page, and then configure

### Basic Configuration

![Some basic configuration](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/f231a98f-2a32-49d6-6f14-754617953e00/large)

### Other Options Configuration Introduction

* **"Preemption"**: This mode can get a lower price, but it can't be used for services that need to stay online for a long time (such as Web services), its longest usage period is 24 hours, but in my use , it is sometimes terminated in less than an hour. It is only suitable for calculating some things for a short time, and it is terminated after the calculation. Do not turn on this function for ordinary general use.
* **Auto-restart**: Recommended to turn on to get the benefits of being in the cloud, and better Uptime
* **During host maintenance**: It is recommended to select "Migration" for the same reason as above
* **IP forwarding**: It is recommended to turn off this function, it is almost never used, and turning it off helps to improve security
* **SSH**: This may be different from some other VPSs, it does not automatically generate user passwords by default, so you must configure the public and private keys for remote login. Moreover, the user name at the end of the public key that you fill in is useful. The user name you fill in is the user name you need to log in. By default, root login is not supported unless you set the user name to root.

![SSH config screenshot](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/c2c01644-20d1-4626-2b5c-7758d8d45e00/large)

### Firewall Configuration

GCE has the firewall turned on by default and cannot be turned off. It can only allow the traffic of the protocol and port you specify by yourself; after my own actual test, GCE can automatically filter the equivalent DDOS attack traffic. Since the firewall cannot be turned off, services similar to IPv6 Tunnel cannot be configured, so the current GCE cannot support IPv6, but I believe that Google will still enable IPv6 support in the future. In "Network", you can find [Firewall Rules](https://console.cloud.google.com/networking/firewalls/list), then you can [Add Firewall Rules](https://console.cloud.google. com/networking/firewalls/add). SSH, ICMP, etc. are already allowed by default (starting with default)

![list of all rules I have enabled](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/ba0e6843-f6c3-4ca6-6f2c-c40a2bb4ae00/large)

![SNMP monitoring configuration](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/dd3fba9c-4342-4e7c-3168-e84ce4f20e00/large)

I just need another host to access the SNMP monitoring, I don't need to expose it to the internet, so the IP is restricted.

![Configuration for authoritative DNS server](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/e7c635ae-cfd7-4c50-00c1-06cc3b546f00/large)

Some DNS requests are sent over TCP, so you need to enable TCP requests as well. If a target tag is configured, it is not a firewall rule that is applied to all instances by default, and the same tag needs to be configured on the instance.

![add the same tag](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/ff0b4a21-ef43-429d-a742-b6e307d38800/large)

## The Internet - as Great as Google Itself

Since GCE is owned by Google, its network is actually Google's AS15169, so you don't have to think about it and know that it will be fast to connect to Google's services, but after actual use, I found that it is much faster than I thought, GCE's connection to Google's services is like an intranet Same.

```
$ ping google.com
PING google.com (74.125.203.102) 56(84) bytes of data.
64 bytes from th-in-f102.1e100.net (74.125.203.102): icmp_seq=1 ttl=53 time=0.631 ms
64 bytes from th-in-f102.1e100.net (74.125.203.102): icmp_seq=2 ttl=53 time=0.433 ms
64 bytes from th-in-f102.1e100.net (74.125.203.102): icmp_seq=3 ttl=53 time=0.330 ms
64 bytes from th-in-f102.1e100.net (74.125.203.102): icmp_seq=4 ttl=53 time=0.378 ms
64 bytes from th-in-f102.1e100.net (74.125.203.102): icmp_seq=5 ttl=53 time=0.413 ms
^C
--- google.com ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 3999ms
rtt min/avg/max/mdev = 0.330/0.437/0.631/0.103 ms
```

It's even more magical if traceroute:

```
$ traceroute google.com
traceroute to google.com (64.233.189.113), 64 hops max
  1 64.233.189.113 0.826ms 0.313ms 0.435ms
```

At present, the speed of China's connection to GCE Asia is not bad. It is billed by traffic, and the actual test can exceed 100M bandwidth. The location of the Asian region is in Taiwan, and the Chinese connection usually goes around Hong Kong, and then the route from Hong Kong to Taiwan takes Google's own backbone network, so the final result is only a dozen milliseconds slower than the Hong Kong server.

If other countries are connected, the advantages are even more obvious. Google's network is too powerful. Whether it is Asia or Europe and the United States, it has almost jumped into Google's backbone network before leaving the city/country, and then Google's own backbone network is selected. The route is usually better than the operator's route selection, and there are almost no detours.

### Configure Anycast

**Details have been introduced in [next article](https://guozeyu.com/2016/10/build-a-anycast-network-gce/)**, can be assigned to a ** by Load Balancing Independent** IPv4 address, can also have native IPv6, support CDN and HTTPS. How fast is Google's Anycast? As fast as Google.

### Internal Network Communication

Each instance has its corresponding intranet IP, which is convenient for establishing a direct connection between two hosts. The magic of this intranet IP is: it can be used across Availability Zones, including across continents. After actual testing, the connection speed established using the intranet IP will be slightly faster, and the price charged (if it is interstate) is also discounted.

## Function

### Statistics

In the background of GCE, it can display data such as CPU utilization, disk bytes, disk operation bytes, network bytes, network packet volume and other data for up to 30 days. updated in real time.

### Snapshot

You can add snapshots to the host for easy recovery in other instances, or for backup purposes. GCE's snapshots are incremental backups, which means that if there are two snapshots, the second snapshot only backs up the differences from the first snapshot. [Using this script](https://github.com/jacksegal/google-compute-snapshot) can realize automatic backup, and can automatically delete old backups when they expire.

### Additional Hard Drive

You can easily add hard drives of any size (greater than 10 GB), with a variety of disk types to choose from. After my tests, if you perform high I/O operations for a long time, the read and write speed of the hard disk will be significantly reduced. And it's not necessarily that SSDs are faster than HDDs. The size of the hard drive is positively correlated with throughput limits and random IOPS limits, which means that a 40G HDD is as fast as a 10G SSD.

## Use Cases

The low-profile version is good for building a website or as a "ladder", and the price of $5 per month is still very attractive (be sure to pay attention to the traffic price for "ladders"!). The already well-established web control page can greatly reduce the threshold for use and learning. High-profile versions have independent CPUs and are highly scalable. Used in conjunction with various functions of Google Cloud, you can build large websites or [game servers](https://cloud.google.com/solutions/gaming/).

## Some Pits of GCE

Let's talk about some of its pits, especially some of the problems that will be encountered when switching from traditional VPS to GCE. Since GCE is cloud-based, a lot of things are different.

### The External Network IP of the Host is not Directly Assigned

Although GCE provides independent IP, when you execute `ifconfig -a`, you will see that the IP assigned by default is a reserved IP of the intranet (such as 10.123.0.1), which is for convenience between instances IPs that connect to each other. When you configure some services that require external network to bind to a specified IP, you only need to bind to this IP, and the external network IP assigned to you will automatically forward traffic to this IP. Moreover, your host does not know that the external IP is the host itself, so all traffic destined for the external IP corresponding to the host will pass through a router, and then return by the router, and the firewall policy will be applied. Therefore, if local communication is required, it is recommended to use the loopback address (such as 127.0.0.1, ::1) or the intranet address as much as possible.

### Firewall

The firewall that cannot be turned off may be a little uncomfortable for the first time, but once you get used to it, you will fall in love with this function. This firewall function based on the router is more convenient than the restriction in iptables. It is strongly recommended to take advantage of the firewall function and not allow all ports by default. For example, if all your website traffic passes through Cloudflare, then you can use the firewall to allow only Cloudflare's IP to connect to your host.

### Multi-IP and IPv6 are not Supported

You cannot purchase multiple IPv4 addresses by adding money on GCE, so an instance can only have one IPv4 address. If you need multiple IPv4 requirements, you can try multiple instances (or you can implement multiple IP addresses through Load Balancing). At the same time, GCE does not currently support IPv6, which is a real shame. It is now possible to implement IPv6 [through a load balancer](https://guozeyu.com/2016/10/build-a-anycast-network-gce/).
