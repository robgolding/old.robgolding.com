---
layout: post
title: "Django in Production: Part 2 - Background Tasks"
date: 2011-11-27 20:58
comments: true
tags: 
- django
- programming
---

In my last post, I described in detail the core stack which powers a Django
application in production. In this second part, I'll talk about something which
has become commonplace in modern web applications - background tasks.

Celery
------

The most common task queue used with Django is called [Celery][celery].
Although it's a completely separate application, Celery is really easy to
integrate - particularly when using the `djcelery` app.

[celery]: http://celeryproject.org
