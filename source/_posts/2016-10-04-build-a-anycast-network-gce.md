---
title: Build an Anycast Network with GCE, Super Fast Hong Kong Node, Google Cloud CDN
tags:
  - Google Cloud Platform
  - VPS
  - Internet
id: '1998'
categories:
  - - Development
date: 2016-10-04 09:27:00
languages:
  zh-CN: https://guozeyu.com/2016/10/build-a-anycast-network-gce/
cover: https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/4dcd6c19-bcaa-4d1a-2b5f-db00454a7c00/large
---

Update February 2018: Due to the [free CloudFlare CNAME/IP access](https://cf.tlo.xyz), which is also Anycast, this site no longer uses Google Cloud CDN. [In the last article](https://guozeyu.com/2016/10/asia-google-compute-engine/), I briefly introduced the basic use of Google Compute Engine (GCE for short). In this article I will describe how to build an Anycast network with GCE and test its speed. To achieve this function, you need to use [Cross-Region Load Balancing](https://cloud.google.com/compute/docs/load-balancing/http/cross-region-example) (cross-region load balancing) , this function is equivalent to an HTTP(S) reverse proxy, so it can only perform load balancing for HTTP/HTTPS requests.
<!-- more -->

## Brief overview

This function implemented on GCE is based on the network proxy of the seventh layer, so its topology diagram is as follows: User -> Edge Server -> Instance

* **Connection between user and edge server**: Use HTTP or HTTPS; if it is an HTTPS connection, the TLS encryption process is implemented on the edge server.
* **Connection from edge server to instance**: Use HTTP or HTTPS connection, the previous network was a dedicated line of Google.

Edge servers use all of Google's edge servers, regardless of how many instances are configured. After enabling this function, you will get another Anycast IP address, which is an exclusive IP address.

What is Anycast? Anycast allows multiple hosts to use one IP address. When a user connects to this IP address, they are connected to only one of the multiple hosts. Usually, the fastest line is selected, which can effectively reduce the delay, so many DNS/CDN providers all use Anycast.

Also, using load balancing is currently the only way to get it to natively support IPv6. For details, please refer to its documentation: [IPv6 Termination for HTTP(S), SSL Proxy, and TCP Proxy Load Balancing](https://cloud.google.com/compute/docs/load-balancing/ipv6)

![Screenshot of reserved IPv6 address](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/52580111-68c9-4b63-a007-49b707f16100/large)

## Configuration method

### Create instance

First, you need to go to the GCE backend and create at least two instances in different regions. I created two new instances specifically for testing the Anycast function:

![Two instances built for Anycast](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/0353378c-82b5-44eb-fdcd-05cafc50c800/large)

You can also create multiple instances per region to improve availability, but I only created one instance for each region, these two instances are called anycast-asia and anycast-us.

### Create instance group

Then, you need to [create an instance group](https://console.cloud.google.com/compute/instanceGroups/add) for the instances in each region:

![Instance group configuration page](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/4c590a68-e98a-450d-392c-85958e7c1500/large)

It should be noted that the "Multi-zone" in the location in the instance group configuration page refers to different **available zones** (Zone) in the same **region** (Region), not multiple different regions, so this is actually a translation error, it should be called "multi-availability zone".

* * *

People who are new to cloud services may not understand the concept of availability zones, you can refer to [this article by AWS](http://docs.aws.amazon.com/en_us/AWSEC2/latest/UserGuide/using-regions-availability- zones.html) to understand. To put it simply, the concept of a region refers to an area that is far away (such as between cities, such as Beijing and Shanghai), all servers in Beijing are counted as Beijing area, and all servers in Shanghai are counted as Shanghai area. However, in order to achieve higher availability, multiple data centers are usually set up in the same region, that is, availability zones. Although these availability areas are in one area, the distance between them may be tens or even hundreds of kilometers, but the distance between these availability areas is much smaller than the distance between different areas, so these availability areas The network latency between them is also low.

The significance of setting up multiple availability zones is: it may increase the availability (mainly to avoid external factors: fire, etc.), although it is distributed in different places, but the distance between the availability zones is not far, so the network delay can be neglect.

I only created one instance per zone, so I just had to select "Single-zone". An instance group needs to be established for each region, so a total of two (or more) instance groups need to be established. I have configured two instance groups called asia-group and us-group.

### Create load balancer

After completing the first two steps, you need to establish load balancing rules. You need to select "HTTP(S) Load Balancing" to realize the function of Anycast.

![Three load balancing modes](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/6574fd57-514c-4319-1550-c8c1bf83a000/large)

In the load balancing configuration interface, add both instance groups to the "backend". This function also needs to create a health check (equivalent to a monitoring function), which can be switched when the host is down.

![Disable CDN function for now](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/8dcf1a51-670a-4292-feb8-6d87e7e26000/large)

![leave the default "host paths and rules"](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/75e45756-92ae-422c-0d69-8df497efa600/large)

![This is an example that requires HTTP. If HTTPS is required, a certificate needs to be specified. ](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/3390be79-2e20-4a09-2506-c4fe0cea8d00/large)

After the creation is successful, you can see the following interface, where the IP address is Anycast IP.

![Successfully created an Anycast IP](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/8b9fc433-8d84-4b7c-c3de-6f443e763300/large)

### Nginx configuration

The configuration here is only for the convenience of debugging, and there is no need to modify the Nginx configuration in actual use. Install Nginx on both hosts, then slightly modify the default configuration file: add two headers, then Reload.

```
add_header X-Tlo-Hostname $hostname always;
add_header Cache-Control "max-age=36000, public";
```

### Check if available

Before testing Anycast, test both hosts for issues. For readability, I replaced curl's IP address with the hostname and omitted the Header field that is the same for both hosts

```
$ curl anycast-us -I
HTTP/1.1 200 OK
…
ETag: "57ef2cb9-264"
X-Tlo-Hostname: **anycast-us**
$ curl anycast-asia -I
HTTP/1.1 200 OK
…
ETag: "57ef2b3b-264"
X-Tlo-Hostname: anycast-asia
```

It can be seen that there is no problem with both hosts. Then, I use my computer to test that Anycast IP:

```
$ curl anycast-ip -I
HTTP/1.1 200 OK
…
ETag: "57ef2b3b-264"
X-Tlo-Hostname: anycast-asia
Accept-Ranges: bytes
Via: 1.1 google
```

As you can see, this is what the anycast-asia host responds to. Now I continue to test this Anycast IP with another host in the US:

```
$ curl anycast-ip -I
HTTP/1.1 200 OK
…
ETag: "57ef2cb9-264"
X-Tlo-Hostname: anycast-us
Accept-Ranges: bytes
Via: 1.1 google
```

At this point, the anycast-us host responds because the client is closer to the server. When accessing via Anycast IP, you can see the `Via: 1.1 google` field in the HTTP Header.

## Speed ​​test

### Ping test

The Ping test found that the speed is very fast, and it seems that the anti-generation operation is placed on Google's edge server. **As fast as Google! **

![Foreign speed test on Anycast IP](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/44b63586-f953-451d-8549-1ffa3295d400/large)

The speed in China is first-class and fast. Google has an edge node in Hong Kong, so it basically goes directly to the Hong Kong node, which is much faster than the original connection to the Taiwan availability zone. (Only some IP ranges are fully connected directly)

![Domestic speed test on Anycast IP](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/1ffc3042-5f7f-41e2-816f-87cd4a9b3100/large)

### HTTP GET test

Before the CDN function is turned on, the load balancer will not cache any content, so you will find that the speed of Connect is very fast, but there is still a lot of TTFB delay.

![HTTP GET test for Anycast IP](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/43b7f72f-09f1-4c65-72ae-58c640921c00/large)

It can be predicted that if the HTTPS function is enabled, the waiting time required for TLS will be very short, the TTFB time will remain unchanged, and the total time will not be extended too much.

#### HTTP GET test after CDN is enabled

When the CDN is turned on, the load balancer will automatically cache the static resources. When the cache hits, the Age field will be displayed. This field indicates that the self-cache is hit, and the number behind represents the content that was cached in seconds ago. .

curl anycast-ip -I
HTTP/1.1 200 OK
…
Via: 1.1 google
Age: 10

After executing this command multiple times, there is a certain chance that the Age field disappears, which may be that the traffic refers to different availability zones in the same region. But in short, the cache hit rate is not high, even if it has been accessed before.

![HTTP GET test with CDN turned on](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/17c182ca-f880-487e-e1fb-74a01d63ed00/large)

After running the test several times to make sure there is a cache, there doesn't seem to be much noticeable improvement in speed. The obvious improvement is that the TTFB latency in Paris and Amsterdam has been reduced from 200ms to 100ms, but it is still not satisfactory. The possible reason is: Google does not cache the content on the edge node closest to the visitor, but on another node. [Location list of CDN cache servers](https://cloud.google.com/cdn/docs/locations)

![Location of cache server](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/4dcd6c19-bcaa-4d1a-2b5f-db00454a7c00/large)

## Statistics and Logs

When Load Balancing is turned on, some information will be automatically recorded under Google Cloud Platform.

### Real-Time Traffic View

In the backend of the web page, under Network, Load balancing, and Backend service of the advanced menu, you can view the real-time traffic situation:

![The graphics are still pretty](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/e67b48a2-54bc-4950-c44d-187de69d2b00/large)

### Latency Log

In Stackdriver, Trace in the background of the webpage, you can see the latency log:

![Latency log screenshot 1](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/4f9e9e5f-fea7-4d71-2a56-224606de8600/large)

![Latency log screenshot 2](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/0386e758-48cd-48fa-3b25-124a520f5700/large)

The delay here includes network delay and server response delay

## Summarize

The Anycast function that GCE can implement can only be implemented through HTTP proxy (layer 7), so it can only proxy HTTP requests, and other functions (such as DNS) cannot be implemented. Therefore, many functions are limited by the functions of the load balancer (for example, certificates and HTTP2 need to be configured on the load balancer). However, since the TLS encryption and decryption process is implemented on the edge server, and it also has the CDN function, it will Faster than pure Anycast (eg based on IP layer, or TCP/UDP layer).

### Price

The first five Rules **$18/month**, the traffic cost is unchanged compared to GCE, and the traffic of the content that has been cached has a little discount.

## Compared

### Cloudflare

Anycast can also be implemented by using the services provided by Cloudflare, which is also based on the seventh layer, and will soon be able to implement the function of Cross-Region Load Balancing. Although it can't adjust the weight according to the CPU usage of the host (after all, it can't get the data), it has a powerful Page Rules function and WAF function. CloudFlare doesn't offer dedicated IP addresses, but that's not a big deal. Since it is a third-party service and is not restricted by service providers, it can provide Anycast functions to a variety of different service providers; and it can be used regardless of whether the service provider supports it or not. In terms of connection speed, GCE's connection speed in China has obvious advantages.

### BuyVM

BuyVM is a VPS provider, but it provides a free Anycast function. Its Anycast function is directly based on Anycast at the IP layer, so various services other than HTTP can be configured. BuyVM does not have the so-called edge server, it can only have three nodes, the result of Ping is not as fast as the first two, and the TLS process is also performed on the original host (the one that is closest to the user among the three hosts), which will also There is a certain delay. BuyVM doesn't offer any hosting in Asia, so China's connections aren't that much faster than Cloudflare, and not all of Asia.
