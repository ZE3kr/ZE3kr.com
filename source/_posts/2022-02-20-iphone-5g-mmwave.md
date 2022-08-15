---
title: How Fast is mmWave 5G? 2000Mbps! See if Your iPhone Supports mmWave
tags:
  - Apple
  - iPhone
categories:
  - Technology
date: 2022-02-20 23:59:59
languages:
  zh-CN: https://www.guozeyu.com/2022/02/iphone-5g-mmwave/
---

This article compares the speed difference between IF 5G and millimeter wave 5G, provides a method to determine whether the iPhone uses millimeter wave, describes the meaning of different icons of 5G, compares the low frequency, intermediate frequency and millimeter wave of 5G, and lists the differences between iPhone/iPad The model supports mmWave.

Recently, I used the National Bank and the US version of the iPhone to compare the intermediate frequency and millimeter wave 5G. All tested on the same carrier (Verizon Prepaid) on the same plan (Unlimited Plus), using a physical SIM, in the exact same geographic location.

<!-- more -->

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/bf1266e2-c4fa-4e2b-6fa3-3fba29280001/extra" alt="mmWave 5G (28 GHz)" width="1125" height="2436"/>

It can be seen that millimeter wave 5G (high frequency, mmWave) easily ran to 2000Mbps. In theory, millimeter wave can reach 3000Mbps, but I have tried many times and the highest "just" 2000Mbps.

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/b786139e-7ed3-4ba3-4bcb-a30f00afdc01/extra" alt="IF 5G (3.7 GHz)" width="1170" height="2532"/>

The mid-band 5G (Mid-Band) "only" ran to 929Mbps.

According to this test, millimeter wave 5G is about 2 times faster than intermediate frequency 5G. In their respective ideal cases, mmWave 5G can be 2-3 times faster than mid-band 5G.

## How to Tell if iPhone Uses mmWave?

Enter `*3001#12345#*` on the call page of the system, then click to call:

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/cd468b11-8d24-4919-a22f-1c411c840001/extra" alt="System call interface" width="1170" height="2532"/>

Then we can see the Field Test Mode as shown below:

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/d9ef249f-8ece-4f5a-72fb-0e6afe7fdc01/extra" alt="iPhone Field Test Mode" width="1170" height="2532"/>

Then click More on the top right and select Nr ConnectionStats in 5G. If you can't see 5G, it means there is no 5G signal currently.

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/0bfd16de-f2a5-41a1-50f8-2d3b7fa54701/extra" alt="Enter 5G menu in Field Test Mode" width="1170" height="2532"/>

Then look at the numbers in Band:

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/de78c1e9-ce9d-4955-78fa-069119620401/extra" alt="5G - Nr ConnectionStats - Band" width="1170" height="2532"/>

If the number is less than 100 (as shown above), mmWave is not used. If the display is greater than 100 (common 257-262), it means that you have connected to mmWave 5G. The specific frequencies used can refer to [this table](https://en.wikipedia.org/wiki/5G_NR_frequency_bands#Frequency_bands).

Not all 5G-enabled iPhones support mmWave 5G. Currently only the iPhone 12 and 13 series purchased in the U.S. can use mmWave 5G networks in the U.S.

## 5G Icons

According to [Apple's official website] (https://support.apple.com/zh-cn/HT211828), 5G has various icons. If only 5G is displayed, it is connected to the most common 5G, and the speed is relatively slow. If you see 5G+, 5G UW, and 5G UC, you may be connected to mmWave 5G, which is faster. But in reality, showing 5G+, 5G UW, and 5G UC does not imply the use of mmWave 5G (and possibly only mid-band 5G). Also, in countries other than the US, even if connected to mid-band 5G, only 5G is shown.

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/535cb463-5945-436c-80c6-e2b94597f501/extra" alt="5G Icon List" width="828" height="918"/>

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

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/a7e34af1-5d89-4f14-1d38-35fc8758a801/extra" alt="mmWave antenna locations for iPhones that support mmWave" width="2636" height="472"/>

<img src="https://cdn.tloxygen.com/6T-behmofKYLsxlrK0l_MQ/d8668aba-ce84-4436-57a9-4fbb9f11f301/extra" alt="iPhones that do not support mmWave do not have mmWave antennas" width="2636" height="472"/>

This mmWave antenna opening is very similar to the wireless charging opening for the Apple Pencil in the iPad series, but they are really not the same thing, don't get confused.

## A list of 5G supported by different iPad models

Similarly, you can search for your iPad model on [Apple's official website](https://www.apple.com.cn/ipad/cellular/networks/). You can see the model number on the back of the fuselage. The models that currently support mmWave are:

+ A2379: 12.9-inch iPad Pro (5th generation)
+ A2301: 11-inch iPad Pro (3rd generation)
