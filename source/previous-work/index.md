---
title: My Previous Works' Portfolio
tags: []
categories: []
comments: false
date: false
layout: post
adb: false
---

I will update more details here soon.

## My own websites

What you are visiting right now, `ze3kr.com`, is part of my portfolio. You can visit the [homepage](/) to see my previous posts. I also operate the following websites:

+ [TlOxygen.com][https://www.tloxygen.com]: Selling domain register and web hosting services.

## WildHelper

WildHelper is a mobile app for the students at Beijing University of Technology to check their scores. Besides checking the score, it also supports viewing class schedule, exam, and add supports for score notification. [Official Website (Chinese only)](https://wildhelper.github.io/)

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/6f776c31-c9ab-4aa0-6bd8-c8f7009bdd01/extra" alt="Main screenshot" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/5a967062-8ce1-4bfb-2ed3-2f5faa75fd01/extra" alt="CET 4/6 score" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/6de62e16-d9d3-42fe-bd14-f60fd0494401/extra" alt="Course Schedule" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/6ca84385-9a43-47e0-5560-3dafab9c4801/extra" alt="View the exam date and location" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/129f2f42-1cb3-4877-0984-c8f1ce5e9701/extra" alt="Course listing and grade distribution" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/73617252-4b2a-4a2d-f00e-1af456fc8401/extra" alt="Grade distribution for a specific course" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/32620276-6e88-4351-576c-775fbf5c4b01/extra" alt="Dark mode" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/63fa9d0d-50ce-4d88-c9ad-c006065b7c01/extra" alt="Dark mode schedule" width="1125" height="2436"/>

### Core Function

+ Check score: Automatically calculate the weighted average score, GPA, number of courses, total credits, pass rate and other information of each semester, minor/double degree, first degree, etc., and automatically handle special situations such as retakes, make-up exams, and failures; display grade details in groups, Can display teachers, course types, course numbers, course credits, and support sorting and searching; support score sharing
+ Check class schedule: automatically highlight courses according to the current week number; support semester selection; support class schedule sharing
+ View exams: In the exam week, the exam subjects, exam time, and exam location will be displayed
+ Course listing and grade distribution: Through big data, display basic information such as the average score and average grade point of each course; automatically draw a histogram of course score distribution; support filtering by dimensions such as course types, sorting by indicators such as grades, and searching by attributes such as course names
+ Score notification: support score push notification after subscription, and students will receive a reminder of the score as soon as possible after the score is issued

### Technical Highlights

+ Complete separation of front and back ends, common RESTful API
+ Use native applet development, support WeChat data preloading, periodic update
+ Connect with schools to realize real-time automatic data acquisition
+ Support alumni certification, 100% guarantee that users are authentic, easier to use
+ User histogram is automatically drawn using Canvas2D
+ Error logs are automatically reported, and sent to WeChat group which are convenient for maintenance
+ User feedback button to access customer service at any time
+ Using end-to-end encryption (AES-256-GCM), middlemen (including WeChat) cannot get any user data
+ Users in non-selective weeks can only see the courses they choose
+ The server never stores user passwords
+ Revocation of authorization mechanism, users can completely delete all data at any time

## Cloudflare CNAME Setup (discontinued)

This project allows Cloudflare Hosting Partner to provide a panel for customers, which allows customers to have CNAME setup for free. Because the Cloudflare discontinued the Hosting Partner service, this project was discontinued. [Official Website](https://web.archive.org/web/20211008175842/https://github.com/ZE3kr/Cloudflare-CNAME-Setup). This project received more than 1.3k stars.

### Features of this panel

+ Manage all your DNS records in one place. Using the Cloudflare API v4, this project supports various types of DNS records.
+ Advanced Analytics. You can see the analytics report for a whole previous year, rather than a month.
+ Supports NS setup. This panel provides NS setup information so you can switch to Cloudflare DNS at any time. You can also find your DNSSEC feature is also supported.
+ Supports IP setup. This service provides the anycast IPv4 and IPv6 of the CDN. You can safely use this service on the root domain.
+ Supports mobile devices.
+ Supports multi-languages.

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/f1ceb02c-63ab-45b0-be4a-fe2be0be3001/extra" alt="Screenshot on Desktop" width="2508" height="1362"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/15f61a4d-64d7-4b51-53d5-30e4a90c6101/extra" alt="Screenshot on Mobile" width="866" height="938"/>
