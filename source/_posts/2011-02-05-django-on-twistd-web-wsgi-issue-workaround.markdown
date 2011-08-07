--- 
layout: post
title: Django on twistd.web.wsgi - Issue Workaround
tags: 
- django
- python
- wsgi
- backtrac
- twisted
---

My last few posts have generally been about my final year project - [Backtrac
Backup System](https://github.com/robgolding63/backtrac "Backtrac on GitHub").
One of the recent challenges I faced was getting the Django-based web interface
to run under the Twisted WSGI server. In this post, I'll describe a major issue
with this process, and how I worked around it.

<!--more-->
## TAC or TAP?

When Twisted is installed, it places an executable program called **twistd** on
your path. The purpose of this program is to run a twisted application as a
daemon, and manage all of the tricky daemonization code for you. This includes
things like forking, PID files, and logging.

There are two ways to describe your application two the Twisted daemon: TAC
files and TAP files. I'm pretty sure that TAC stands for _Twisted Application
Configuration_, whereas TAP stands for _Twisted Application Plugin_.

The difference between the two, it seems, is that TAC files pass an
``Application`` object to twistd, while TAP files pass a ``ServiceMaker`` object
(whose purpose is to return a ``Service`` object, when ``makeService`` is
called).

Also, given that TAP files define a``ServiceMaker`` object which Twisted will
call a method on, we can accept a parameter called _options_, which contains the
parsed command-line arguments passed to the application. This excludes any
arguments that were passed to **twistd** itself. As far as I can tell, this is
the main different between TAC and TAP files. It also results in TAP files
generally being longer and more verbose than their TAC counterparts.

For my application, I needed to accept an argument on the command-line, so I was
tied to using a TAP file.
## The Plugin

There are loads of
[examples](http://www.clemesha.org/blog/Django-on-Twisted-using-latest-twisted-w
eb-wsgi "Example: Django on Twisted Web") of TAC files to run Django projects
out there, but none for TAP files (at least that I could find, with my excellent
Google-fu). So, I sat down with the Twisted API documentation, and started to
write one!
<pre lang="python">class ServerServiceMaker(object):
    implements(IServiceMaker, IPlugin)
    tapname = 'backtracweb'
    description = 'Backtrac web server'
    options = Options

    def makeService(self, options):
        # make a new ThreadPool and start it
        pool = threadpool.ThreadPool()
        pool.start()

        # create WSGI resource using the thread pool + Django handler
        resource = wsgi.WSGIResource(reactor, pool, WSGIHandler())

        # create the site and a TCPServer service to serve it with
        site = server.Site(resource)
        web_service = internet.TCPServer(port, site)

        return web_service

serviceMaker = ServerServiceMaker()</pre>
## Issue #4347

Now, this plugin works flawlessly when the ``-n`` flag is passed to **twistd**
(don't daemonize). This means the server stays attached to the terminal, like
the Django development server. When this flag is removed, however (as it would
be when the app is used for real), the server doesn't respond to requests.
Strangely, it is still _running_, and listening on the correct port - but it
simply hangs when a request is made.

It turns out that this issue has already been [reported on Twisted's Trac
instance](http://twistedmatrix.com/trac/ticket/4347 "Creating threadpool and
then daemonizing leads to uninformative hangs or crashes"). **11 months ago**.
It's actually caused by the thread pool being started before the daemonization
code is executed. This consists of forking, and killing off the parent twice -
which in turn kills off the thread pool. Whoops!
## The Workaround

One of the comments on the ticket suggests creating a custom service
(implementing the ``IService`` interface) which starts a given thread pool when
it is started. This means that the thread pool would be started _after_ the
program has daemonized itself, thus solving the issue. So, I got to work:
<pre lang="python">class ThreadPoolService(service.Service):
    def __init__(self, pool):
        self.pool = pool

    def startService(self):
        service.Service.startService(self)
        self.pool.start()

    def stopService(self):
        service.Service.stopService(self)
        self.pool.stop()</pre>
This is a dead simple class, and it should work once it's hooked into Twisted.
Therein lies the problem though - how to combine these two services into just
one - to return from the ``makeService`` method.

The answer came from a very helpful fellow in answer to my [Stack Overflow
question](http://stackoverflow.com/questions/4875326/twisted-creating-a-threadpo
ol-and-then-daemonizing-leads-to-uninformative-hangs "Creating a ThreadPool and
then daemonizing leads to uninformative hangs"): use a ``MultiService``. From
here, everything fell right into place. The [final TAP
file](https://github.com/robgolding63/backtrac/blob/master/twisted/plugins/backt
racweb_plugin.py "backtracweb Twisted plugin") includes a few more details that
deal with command-line arguments and config file parsing, and it works
flawlessly. I've also added a snippet from the linked example, which allows the
static media to be served up by Twisted as well. Generally, I've been pleasantly
surprised at the speed and stability of the Twisted web server.

For me, this was a frustrating issue, but I'm happy that I was able to work
round it to achieve a successful result. I hope this post allows others to do
the same.
