--- 
layout: post
title: Backtrac Implementation - The Major Decisions
tags: 
- django
- university
- programming
- backtrac
- amqp
- piston
---

Thinking a lot about my [3rd year project][back-to-backtrac], I've been
considering how to implement a backup system based around a Django server. So
far, I have reached the conclusion that the system will consist of three main
parts:

- The Django server (yet to be named)
- The client daemon (_backtracd_)
- The backup library itself (_backuplib_)

<!--more-->

As I mentioned in the previous post, the Django server will be based around
[Celery][celery]. This will offer me the ability to schedule a task for
a certain date and time in the future, which is a huge part of the
implementation taken care of.

One thing I am yet to figure out, however, is how the server will communicate
with the client daemons. At the moment I'm thinking [XML-RPC][xmlrpc], but it
seems to me that it will end up a sort of mish-mash of different technologies
- as the Django server will be exposing a JSON API (probably via
[Piston][piston]). I have also considered running Django on the clients too,
so everything can use JSON/Piston, but this seems at the moment like an
unnecessary burden.

Another thing to consider is the _data_ that is to be transmitted between the
nodes. Should I use a bunch of fields in a JSON object to represent the
success/failure of a job, and how much data was backed up, etc.? Another option
is to pickle a Result object of some sort - representing a result as a Python
instance to be interpreted by the server. These are the things I love to think
about when designing a system, and I'll certainly have plenty to write about in
the inevitable report.

Something I _can_ work on in the meantime, however, is
**[backuplib][backuplib]** - the client library that will actually copy the
data. At the moment, it's just a wrapper around rsync/tar. That probably won't
change, but it will need to get a bit smarter in the way it handles failures
(and restores, of course) if it's going to make for the basis a decent backup
system.

[back-to-backtrac]: http://www.robgolding.com/blog/2010/05/25/3rd-year-project-back-to-backtrac/ "3rd Year Project: Back to Backtrac"
[celery]: http://celeryproject.org/ "Celery Project"
[xmlrpc]: http://www.xmlrpc.com/ "XML-RPC"
[piston]: http://bitbucket.org/jespern/django-piston/ "django-piston"
[backuplib]: http://github.com/robgolding63/backuplib "backuplib"
