--- 
layout: post
title: Dell Studio Laptop Wakes Itself up from Hibernate
tags: []

---
My new laptop, a Dell Studio 1535, has started exhibiting a strange problem as of late; when I hibernate it over night, it resumes early in the morning, waking me up with the fan spinning.

![](http://cdn.robgolding.com/uploads/2008/11/dell-studio-15.jpg "dell-studio-15")I was preparing to send the thing back to Dell, when it happened one night and I checked out the event logs. It turns out that Windows Update was resuming the machine from hibernate - and then updating itself. After turning this off, I've had a good night's sleep! Also, Windows Defender was scheduled to scan the machine every morning at 2.00am, so that's been disabled too.

<span style="color: white;">-</span>

I hope this post can provide answers to anyone else experiencing a similar issue.
