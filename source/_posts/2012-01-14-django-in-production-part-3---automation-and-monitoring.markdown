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

There are many ways to monitor a Django application, but one that is
particularly useful is [Graphite][graphite].

### Graphite

{% blockquote %}
Graphite provides real-time visualization and storage of numeric time-series
data on an enterprise level.
{% endblockquote %}

{% img http://cdn.robgolding.com/uploads/2012/01/14/graphite.png %}

The key thing is not to be scared by the word *enterprise*. Graphite is
a relatively simple 3-part system: **Whisper** is an efficient, pure-Python
implementation of a round-robin database, and **Carbon** is a daemon which
manages the Whisper database and provides caching. Finally, the Graphite
"webapp" provides a Django frontend to the data stored in the Whisper database.

Graphite's web interface is, admittedly, hard to use. Its redeeming feature
is a powerful URL-based API which allows you to compose graphs programatically
- which is in some ways easier than navigating the difficult menu system. Once
mastered, though, Graphite can produce some amazingly detailed stats about the
very deepest internals of your application.

Clearly, the key to great graphs is lots (and *lots*) of data. In fact,
37signals report that their servers recorded an incredible
[100,000,000 measurements][svn-statsd] in the first 10 days of 2012. Those
measurements were made using a [statsd][statsd], which is a Node.js daemon for
collecting statistics over a simple UDP protocol, and sending them to Graphite.

### Statsd

The great thing about statsd is that it uses UDP to collect statistics. This
means that a client can be  written in almost any language, and when the server
isn't running the client is *completely unaffected*. After all, statistics are
important, but not important enough to knock the entire application offline
when they aren't being collected.

So far, there's been nothing Django-specific (save for the fact that Graphite
is written in Django). In fact, these systems can be used to monitor just about
anything.

[django-statsd][django-statsd] provides a very useful set of basic statistics
for Django applications, though, and is highly recommended.  Strangely, it's
installed from PyPi with `pip install django-statsd-mozilla`, as the name was
already taken by another app.

Once installed, just enable a couple of middleware classes which record every
request/response and their execution time to statsd. For more specific stats,
though, the client library is very easy to use:

{% codeblock stats.py %}
from django_statsd.clients import statsd
statsd.incr('response.200')
{% endcodeblock %}

Interestingly, it also integrates with django-debug-toolbar, and can
"monkeypatch" Django to enable model/cache statistics - but I'll leave that as
as exercise for the reader.

Automation
----------

Often, in their quest for the least effort possible, programmers automate too
much. In the case of deploying applications, though, which tends to be a simple
but repetitive task, I think we're justified.

### Fabric

If you haven't already, you should spend some time reading about
[Fabric][fabric]. Essentially, it's a Python library that allows SSH
commands to be scripted, whether they are on one or many remote hosts.

{% blockquote %}
Fabric is a Python library and command-line tool for streamlining the use of
SSH for application deployment or systems administration tasks.
{% endblockquote %}

Fabric uses a file at the top-level of a project, called `fabfile.py`, to
define the functions which can be used on the command line.

There are some very complete examples of "fabfiles" which can be used for
Django deployment, such as [this one by Gareth Rushgrove][gr-fabfile].

For my projects, which involve only one server, I use a rather more simplified
version:

{% gist 382545 fabfile.py %}

As you can probably tell from the code, this performs the following operations
at the push of a button:

- Updates the server's Git repository
- Installs any new requirements with `pip`
- Synchronises the database to create any new tables
- Executes any South migrations that haven't yet been run
- Links any static media
- Reloads the webserver (see [part 1][part1] for the Gunicorn setup)

Once this file is in place, executing `fab prod deploy` from anywhere in the
project's source tree will automatically deploy the latest code to the web
server in seconds. With public key authentication, you're not even prompted for
a password. As far as I'm concerned, that's well worth the effort!

[part1]: /blog/2011/11/12/django-in-production-part-1---the-stack/
[part2]: /blog/2011/11/27/django-in-production-part-2---background-tasks/
[graphite]: http://graphite.wikidot.com
[svn-statsd]: http://37signals.com/svn/posts/3076-i-heard-you-like-numbers/
[statsd]: https://github.com/etsy/statsd/
[django-statsd]: https://github.com/andymckay/django-statsd/
[fabric]: http://fabfile.org/
[gr-fabfile]: http://morethanseven.net/2009/07/27/fabric-django-git-apache-mod_wsgi-virtualenv-and-p.html
