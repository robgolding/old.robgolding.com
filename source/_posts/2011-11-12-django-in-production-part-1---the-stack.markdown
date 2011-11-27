---
layout: post
title: "Django in Production: Part 1 - The Stack"
date: 2011-11-12 22:35
comments: true
tags:
- django
- programming
---

Everyone has their preferred way of doing things, and this is more and more
true when there are many options available. In the Django world, this
translates to everyone having their favourite web server, database, proxy, and
so on.

In spite of this, I'm going to spend some time over the next few posts
describing how I deploy Django applications in production from a high-level
perspective. In the first part of this series, I'll talk about the core stack
which serves as the basis of the application.

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

#### Serving Your App

Running the Gunicorn server couldn't be simpler. Similar in nature to the
Django development server, you simply execute `python manage.py run_gunicorn`
from within your project's directory. In production, however, there's an even
better method.

Gunicorn installs an executable script to your environment, called
`gunicorn_django`. To use it, you specify which settings module to use on the
command line: `gunicorn_django path/to/project/settings`. We can use this to
serve an application from a process control system such as
Supervisor.

Supervisor
----------

On a production server, the application needs to start and stop with the
operating system. To achieve this, a *process control system* is usually
required (particularly for daemons that don't do this job themselves, like
Gunicorn's). [Supervisor][supervisor] is a particularly good example, and is
pretty simple to setup. On Ubuntu, install it with `sudo apt-get install
supervisor`. Then place a configuration file in
`/etc/supervisor/conf.d/app_name.conf` with the required options. An example
configuration, shown below, shows how a Django application might be served by
Gunicorn under the `www-data` user from its own [virtualenv][virtualenv], and
save all its output to a log file. The server also starts automatically, and
is restarted if it crashes.

{% codeblock %}
[program:app_name]
command=/path/to/env/bin/gunicorn_django app_name/settings
directory=/path/to/app/
user=www-data
autostart=true
autorestart=true
stdout_logfile=/path/to/app/django_logs/supervisord.log
redirect_stderr=true
{% endcodeblock %}

nginx
-----

Due to the simplicity of Gunicorn, it isn't designed to be exposed to the world
as a "bare" server. Instead, it's recommend that a reverse proxy is placed in
front - which does the job of communicating with the client (browser) directly.

It's down to the way that worker processes are used, so a (perhaps
deliberately) slow client can easily perform a Denial of Service (DoS) attack,
which ends up taking down the entire application. For a more detailed
explanation, see the [deploy page][gunicorn-deploy] in Gunicorn's
documentation.

As it happens, [nginx][nginx] does a fine job of acting as a reverse proxy.
It's also extremely fast, stable, and easy to configure. The following
configuration sets up nginx to serve an application running on port 8000 (the
default for Gunicorn):

{% codeblock %}
upstream app_server {
    server localhost:8000;
}

server {
    listen          80;
    client_max_body_size    4G;
    keepalive_timeout       5;

    access_log  /var/log/nginx/app_name.access.log;

    location /static/ {
        alias   /path/to/app/staticfiles/;
    }

    location / {
        proxy_pass          http://app_server;
        proxy_redirect      off;
        proxy_set_header    Host            $host;
        proxy_set_header    X-Real-IP       $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
{% endcodeblock %}

You might have noticed, too, that this configuration serves static media from
a directory called `staticfiles`, within the application's folder. I would
recommend that the Django contrib app (by the same name) is used to manage
the static media for the project - and this is easily done by executing `python
manage.py collectstatic` on the server.

MySQL
-----

The last thing I'd like to mention in this first part of the series is the
database server. The Django community in general, I've found, tends to prefer
[PostgreSQL][postgres] - though my leaning is towards [MySQL][mysql]. The
decision here isn't really too important, so I'm not going to go into the pros
and cons of each. In the end, as is often the case, it simply comes down to
personal preference.

#### Database Migrations

When deploying code to an application server, it will often be necessary to
perform database migrations. In this case, [South][south] is the perfect tool
for the job. Also, the relevant libraries must be present on the server for
your particular database. For MySQL, this is installable from pip as a module
named `mysql-python`.

To begin with, it's difficult to get used to the workflow of creating schema
migrations, applying them to the database, and then committing the tested files
to the database. Once the initial hurdles are cleared, though, it makes an
awful lot of sense. In a future post I'll talk about how to automate the
process of migrating a production database using this very technique.

In the next post in this series, I'll be talking about executing long-running
background tasks with [Celery][celery].

[benchmark]: http://nichol.as/benchmark-of-python-web-servers
[gunicorn]: http://gunicorn.org
[gunicorn-deploy]: http://gunicorn.org/deploy.html
[supervisor]: http://supervisord.org
[virtualenv]: http://www.virtualenv.org/en/latest/index.html
[nginx]: http://nginx.org
[postgres]: http://www.postgresql.org
[mysql]: http://www.mysql.com
[south]: http://south.aeracode.org
[celery]: http://celeryproject.org
