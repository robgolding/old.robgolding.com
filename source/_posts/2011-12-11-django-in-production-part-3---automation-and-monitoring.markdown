---
layout: post
title: "Django in Production: Part 3 - Automation &amp; Monitoring"
date: 2011-12-11 13:25
comments: true
tags: 
- django
- automation
---

*This is the third part in a series about Django in Production. If you haven't
already, read [part 1][part1] and [part 2][part2] first.*

In the first two posts of this series, I described the core stack which powers
a Django application in production, and the Celery task queue which can be used
to execute code asynchronously. In this third post, I'll describe how
a production Django application can be monitored, and how common tasks such as
deployment can be automated.

<!--more-->

Monitoring Django Applications
------------------------------

There are many ways to monitor a Django application, but the ones that are of
particular interest to me are those that report on the running status of the
various components comprising an application (i.e. internal monitoring). This
makes up just one part of the suite of monitoring tools that can (and should)be
used in a production setup.

### Monit



[part1]: /blog/2011/11/12/django-in-production-part-1---the-stack/
[part2]: /blog/2011/12/11/django-in-production-part-3---automation-and-monitoring/
