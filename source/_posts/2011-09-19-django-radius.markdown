---
layout: post
title: "django-radius"
date: 2011-09-19 16:48
comments: true
tags:
- django
- programming
---

I've been working on a little Django app for a while now, which solves
a problem I often come across in Django projects.
[django-radius][django-radius] allows a developer to use a 3rd party RADIUS
server to provide authentication in their Django application. Not only this,
though, it also allows the use of *multiple* RADIUS servers, which can be
specified in whichever way makes sense.

This might sound a little contrived, so I'll give an example. Imagine a web
service, such as a helpdesk management system, whose users are organised into
groups. Each user belongs to an organisation, which buys into the service on
their behalf. Basecamp and Zendesk operate this model, to name just two. To
login, a user must visit a particular URL - so that the system may determine
which organisation that user belongs to (*http://yourcompany.zendesk.com* for
example).

Now imagine that this service wishes to provide external authentication to its
customer's systems, using RADIUS. This means that there's no need for users to
have *yet another* password on *yet another* system, and is generally a Good
Thing&#8482;.

In this scenario, the developers would have to find some way of validating the
user's credentials using the correct RADIUS server, based on the URL they used
to access the site with. For this type of problem,
[django-radius][django-radius] is the perfect solution.

This is my first real attempt at doing things properly in a Django app, and
quite some effort has gone into documenting both the usage and the code. I'll
leave it to those that are interested enough to read the [README][readme] and
find out how it works.

Hopefully this app will save a little time in someone else's project, and I'd
love to hear from anyone with comments or suggestions on how it can be
improved.

[django-radius]: http://github.com/robgolding63/django-radius/ "django-radius"
[readme]: https://github.com/robgolding63/django-radius/blob/master/README.md "django-radius README"
