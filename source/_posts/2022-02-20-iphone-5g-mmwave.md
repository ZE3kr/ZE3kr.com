---
title: How Fast is mmWave 5G? 2000Mbps! See if Your iPhone Supports mmWave
tags:
  - Apple
  - iPhone
categories:
  - Technology
date: 2022-02-20 23:59:59
languages:
  zh-CN: https://guozeyu.com/2022/02/iphone-5g-mmwave/
---

This article compares the speed difference between IF 5G and millimeter wave 5G, provides a method to determine whether the iPhone uses millimeter wave, describes the meaning of different icons of 5G, compares the low frequency, intermediate frequency and millimeter wave of 5G, and lists the differences between iPhone/iPad The model supports mmWave.

Recently, I used the National Bank and the US version of the iPhone to compare the intermediate frequency and millimeter wave 5G. All tested on the same carrier (Verizon Prepaid) on the same plan (Unlimited Plus), using a physical SIM, in the exact same geographic location.

<!-- more -->

![mmWave 5G (28 GHz)](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/a8a88cbe-db0b-4d74-442d-7fedd95d2600/extra)

It can be seen that millimeter wave 5G (high frequency, mmWave) easily ran to 2000Mbps. In theory, millimeter wave can reach 3000Mbps, but I have tried many times and the highest "just" 2000Mbps.

![IF 5G (3.7 GHz)](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/0e1f4e4b-93b8-4807-273c-35d6de27fe00/extra)

The mid-band 5G (Mid-Band) "only" ran to 929Mbps.

According to this test, millimeter wave 5G is about 2 times faster than intermediate frequency 5G. In their respective ideal cases, mmWave 5G can be 2-3 times faster than mid-band 5G.

## How to Tell if iPhone Uses mmWave?

Enter `*3001#12345#*` on the call page of the system, then click to call:

![System call interface](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/893f8af5-75fb-4dec-d4ad-bff5b76bd200/extra)

Then we can see the Field Test Mode as shown below:

![iPhone Field Test Mode](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/893f8af5-75fb-4dec-d4ad-bff5b76bd200/extra)

Then click More on the top right and select Nr ConnectionStats in 5G. If you can't see 5G, it means there is no 5G signal currently.

![Enter 5G menu in Field Test Mode](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/8633a9c8-aa7c-4385-29ba-04c498b90f00/extra)

Then look at the numbers in Band:

![5G - Nr ConnectionStats - Band](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/611c5513-5f69-4c97-3de7-c4fee2a24700/extra)

If the number is less than 100 (as shown above), mmWave is not used. If the display is greater than 100 (common 257-262), it means that you have connected to mmWave 5G. The specific frequencies used can refer to [this table](https://en.wikipedia.org/wiki/5G_NR_frequency_bands#Frequency_bands).

Not all 5G-enabled iPhones support mmWave 5G. Currently only the iPhone 12 and 13 series purchased in the U.S. can use mmWave 5G networks in the U.S.

## 5G Icons

According to [Apple's official website] (https://support.apple.com/zh-cn/HT211828), 5G has various icons. If only 5G is displayed, it is connected to the most common 5G, and the speed is relatively slow. If you see 5G+, 5G UW, and 5G UC, you may be connected to mmWave 5G, which is faster. But in reality, showing 5G+, 5G UW, and 5G UC does not imply the use of mmWave 5G (and possibly only mid-band 5G). Also, in countries other than the US, even if connected to mid-band 5G, only 5G is shown.

![5G Icon List](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/b854be96-8d2f-41ab-1410-6f125d1dc600/extra)

## Three Common frequency bands for 5G: low frequency, medium frequency, mmWave

The low frequency range is within 1 GHz, the mid frequency is 1-6 GHz, and the millimeter wave is 24-40 GHz.

Low-frequency 5G, also known as 5G Nationwide (Verizon), Extended Range 5G (T-Mobile), 5G (AT&T). It is the most widely covered 5G, but the speed is not very ideal, and sometimes it is not even as fast as 4G/LTE. Many existing 4G base stations can be easily upgraded to low-frequency 5G. In my opinion, it's just a quasi-5G network.

Mid-band and mmWave 5G, also known as 5G Ultra Wideband (Verizon), Ultra Capacity 5G (T-Mobile), 5G+ (AT&T). It is a real 5G network.

Currently in China, all carriers use mid-band for 5G. Compared with millimeter wave, IF has wider coverage under the same number of base stations. This is because the intermediate frequency has a longer wavelength and is less likely to be blocked by obstacles than millimeter waves during propagation.

LTE-Advance, also known as 5G Evolution (AT&T). Refers to 4G networks that use technologies such as carrier aggregation, 4x4 MIMO, and 256 QAM. This kind of network is not a 5G network at all, just a faster 4G network.

## List of 5G supported by different iPhone models

Search your phone model on [Apple's official website](https://www.apple.com.cn/iphone/cellular/) (for example, A2629, A2634, A2639, A2644 are the models of the iPhone 13 series in mainland China, Hong Kong and Macau) ), then see if any of the bands in n257-262 are supported. As of now, only the iPhone 12/13 sold in the US supports mmWave. You can see the model number on the back of the fuselage. The models that currently support mmWave are:

+ A2483: iPhone 13 Pro
+ A2484: iPhone 13 Pro Max
+ A2482: iPhone 13
+ A2481: iPhone 13 mini
+ A2341: iPhone 12 Pro
+ A2342: iPhone 12 Pro Max
+ A2172: iPhone 12
+ A2176: iPhone 12 mini

There's an easier way: look for an opening for a millimeter-wave antenna on the right side of the iPhone (Image credit Apple)

![mmWave antenna locations for iPhones that support mmWave](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/362700f0-52e2-4b88-e3b4-e8dcfc9e6500/extra)

![iPhones that do not support mmWave do not have mmWave antennas](/cdn-cgi/imagedelivery/6T-behmofKYLsxlrK0l_MQ/e2d925bc-93df-4d3f-1438-a362567e3d00/extra)

This mmWave antenna opening is very similar to the wireless charging opening for the Apple Pencil in the iPad series, but they are really not the same thing, don't get confused.

## A list of 5G supported by different iPad models

Similarly, you can search for your iPad model on [Apple's official website](https://www.apple.com.cn/ipad/cellular/networks/). You can see the model number on the back of the fuselage. The models that currently support mmWave are:

+ A2379: 12.9-inch iPad Pro (5th generation)
+ A2301: 11-inch iPad Pro (3rd generation)
