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

+ [TlOxygen.com](https://www.tloxygen.com): Selling domain register and web hosting services.

## WildHelper

WildHelper is a mobile app for the students at Beijing University of Technology to check their scores. Besides checking the score, it also supports viewing class schedule, exam, and add supports for score notification. [Official Website (Chinese only)](https://wildhelper.github.io/)

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/02099d11-b593-4bfa-e16d-9417cc0fcd00/extra" alt="Main screenshot" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/bcc49c6d-9859-48ed-7c48-941f9c30c400/extra" alt="CET 4/6 score" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/a8b7a293-4ae2-4c63-c7b2-6a174f457f00/extra" alt="Course Schedule" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/fcb23914-d589-4088-4ade-9e33a9701b00/extra" alt="View the exam date and location" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/b6441a40-9560-4ae6-ae09-38b114a86b00/extra" alt="Course listing and grade distribution" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/73550bd5-3c1b-4a17-5e16-60ae91a61e00/extra" alt="Grade distribution for a specific course" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/b945eff9-8f02-467f-999c-ec437726ce00/extra" alt="Dark mode" width="1125" height="2436"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/15c8cd1f-9fd5-4e58-ada0-206e0313da00/extra" alt="Dark mode schedule" width="1125" height="2436"/>

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

## Cloudflare CNAME Setup

This project allows Cloudflare Hosting Partner to provide a panel for customers, which allows customers to have CNAME setup for free. Because the Cloudflare discontinued the Hosting Partner service, this project was discontinued. [Official Website](https://web.archive.org/web/20211008175842/https://github.com/ZE3kr/Cloudflare-CNAME-Setup). This project received more than 1.3k stars.

### Features of this panel

+ Manage all your DNS records in one place. Using the Cloudflare API v4, this project supports various types of DNS records.
+ Advanced Analytics. You can see the analytics report for a whole previous year, rather than a month.
+ Supports NS setup. This panel provides NS setup information so you can switch to Cloudflare DNS at any time. You can also find your DNSSEC feature is also supported.
+ Supports IP setup. This service provides the anycast IPv4 and IPv6 of the CDN. You can safely use this service on the root domain.
+ Supports mobile devices.
+ Supports multi-languages.

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/04c82c8f-2f18-4fa6-1797-50e1e817f300/extra" alt="Screenshot on Desktop" width="2508" height="1362"/>

<img src="https://cdn.tlo.xyz/6T-behmofKYLsxlrK0l_MQ/8d530cec-e0e0-4051-0298-e66d1bf04900/extra" alt="Screenshot on Mobile" width="866" height="938"/>
