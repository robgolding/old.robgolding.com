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

Often websites perform complex tasks which take a long time to finish. These
tasks don't always need to be done before the response is sent to the browser,
so can be executed elsewhere. In cases like this, tasks can be performed
asynchronously, by one or more "workers" which consume a task queue.

Celery
------

The most common task queue used with Django, at least in my experience, is
called [Celery][celery].

> Celery is an asynchronous task queue/job queue based on distributed
> message passing. It is focused on real-time operation, but supports
> scheduling as well.

Celery isn't dependant on Django in any way - it's a completely separate
application. It does, however, integrate extremely well with Django projects.
By installing the `django-celery` package and then adding `djcelery` to the
list of `INSTALLED_APPS`, Celery can be run and managed through Django itself
- thanks to a set of handy management commands.

This post isn't about how to get Celery working, so I'll skip over the details.
For a more information on using Celery with Django, the
[project's documentation][celerydocs] is excellent.

### Celery In Production

Running Celery locally is easy: a simple `python manage.py celeryd` does the
trick. In production, though, something a little more robust is needed.

Thankfully, it's not difficult at all. Just like Gunicorn, Celery can be
overseen by Supervisor.

*If you didn't read [Part 1 of Django in Production (The Stack)][part1], now
would be a good time to do that.*

To add Celery to our Supervisor configuration, the following is all that's
required:

{% codeblock %}
[program:app_name_celery]
command=/path/to/env/bin/python manage.py celeryd -E -l info
directory=/path/to/app/
user=www-data
autostart=true
autorestart=true
stdout_logfile=/path/to/app/django_logs/celeryd.log
redirect_stderr=true
{% endcodeblock %}

In fact, Celery even comes with some
[example Supervisor configuration][celery-supervisor] files.

[part1]: /blog/2011/11/12/django-in-production-part-1---the-stack/
[celery]: http://celeryproject.org
[celerydocs]: http://django-celery.readthedocs.org/en/latest/getting-started/first-steps-with-django.html
[celery-supervisor]: https://github.com/ask/django-celery/blob/master/contrib/supervisord/celeryd.conf
