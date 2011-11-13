---
layout: post
title: "Django in Production: Part 1 - The Stack"
date: 2011-11-12 22:35
comments: true
categories: 
---

Everyone has their preferred way of doing things, and this is more and more
true when there are many options available. In the Django world, this
translates to everyone having their favourite web server, database, proxy, and
so on.

In spite of this, I'm going to spend some time over the next few posts
describing how I deploy Django applications in production - from start to
finish. In the first part of this series, I'll talk about the stack upon which
the applications run.

Gunicorn
--------

Python-based web applications have traditionally been run under Apache, using
a module such as `mod_python` or `mod_wsgi`. Apache, however, can be somewhat
of a resource-hog, particularly on virtual servers which often have limited
memory. Also, as I'll discuss in detail later in this series, deploying code
frequently to an Apache-hosted application can be troublesome. A new solution
is in order.

There is quite a choice of Python-based web servers for us "cool kids" to use,
many of which are tested in this detailed (if a little old)
[benchmark by Nicholas Piël][benchmark].

[Gunicorn][gunicorn] offer super simple configuration, an extremely small
footprint, and it's pure Python - so you can install it with a quick `pip
install gunicorn`.

[benchmark]: http://nichol.as/benchmark-of-python-web-servers
[gunicorn]: http://gunicorn.org
