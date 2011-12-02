---
layout: post
title: "Django in Production: Part 2 - Background Tasks"
date: 2011-11-27 20:58
comments: true
tags: 
- django
- programming
---

*This is the second part in a series about Django in Production. If you haven't
already, [read part 1 here][part1].*

In my last post, I described in detail the core stack which powers a Django
application in production. In this second part, I'll talk about something which
has become commonplace in modern web applications - background tasks.

Often websites perform complex tasks which take a long time to finish. These
tasks don't always need to be done before the response is sent to the browser,
so can be executed elsewhere. In cases like this, tasks can be performed
asynchronously, by one or more "workers" which consume a task queue.

<!--more-->

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
[program:app_name_celery_worker]
command=/path/to/env/bin/python manage.py celeryd -E -l info
directory=/path/to/app/
user=www-data
autostart=true
autorestart=true
stdout_logfile=/path/to/app/django_logs/celeryd.log
redirect_stderr=true
{% endcodeblock %}

In fact, Celery even comes with some
[example Supervisor configuration][celery-supervisor] files, which can be
adapted to suit the particular needs of a production setup.

### Worker Nodes

In development, a Celery worker would typically be spun up in another terminal
window, alongside the development server. This same technique can't be assumed
though, for a production setup.

At first, perhaps, the task queue worker(s) might be running on the same server
as the application itself. There comes a point, though, where it makes sense to
have at least one server dedicated to running asynchronous tasks. This can then
be scaled out to suit, by adding more and more worker servers as required.

RabbitMQ
--------

Celery uses message passing to queue tasks for the workers to consume. This is
provided by an altogether separate application - the [message broker][broker].

Celery's most commonly used message broker, and that with the most support, is
[RabbitMQ][rabbitmq]. The details of how to setup RabbitMQ are a little out of
the scope of this post, but the [server documentation][rabbitmq-docs] are
complete and provide a good starting point.

### Virtual Hosts

In a large application with more than one worker node, the message broker would
typically sit on a dedicated host (or even be clustered over multiple hosts).
In this case, multiple applications might share the same message broker.

RabbitMQ supports this type of configuration through the use of *virtual hosts*
(or VHosts). Much like virtual hosts used in Apache, these allow a single
broker to be "split" into multiple, distinct parts. Access control can be
configured over virtual hosts, so any single application is prevented from
affecting another's application through the use of a shared broker.

Even if your message broker is only used by one application, that application
should be configured with its own virtual host and credentials.

Monitoring
----------

It's difficult to know exactly what a task queue like Celery is doing, as it
doesn't present a pretty web interface for us to watch. It can, however, be
monitored - and this is something I'll be talking about in detail later on in
the series.

[part1]: /blog/2011/11/12/django-in-production-part-1---the-stack/
[celery]: http://celeryproject.org/
[celerydocs]: http://django-celery.readthedocs.org/en/latest/getting-started/first-steps-with-django.html
[celery-supervisor]: https://github.com/ask/django-celery/blob/master/contrib/supervisord/celeryd.conf
[broker]: http://en.wikipedia.org/wiki/Message_broker
[rabbitmq]: http://www.rabbitmq.com/
[rabbitmq-docs]: http://www.rabbitmq.com/admin-guide.html
