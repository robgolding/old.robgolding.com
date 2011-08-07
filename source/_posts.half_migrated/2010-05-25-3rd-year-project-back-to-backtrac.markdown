--- 
layout: post
title: "3rd Year Project: Back to Backtrac"
tags: 
- django
- university
- life
- backtrac
---
After contacting the university regarding my 3rd year project/dissertation, my ideal choice has been confirmed. I'll be making a distributed backup system, based around a [Django](http://djangoproject.com "Django Project") web interface and [AMQP](http://www.amqp.org/confluence/display/AMQP/About+AMQP "Advanced Message Queueing Protocol").

<!--more-->Backtrac is a pretty old project, and one whose name I am likely to change once I'm working on it. It was started in my first semester of university last year, when I didn't have enough work to keep me busy. I didn't know enough about Django back then, though, so it was left unfinished and only half-working.

Now I think I know enough to pull this off, and it's a chance to do something I really enjoy - while getting marks for it in the process! If everything goes to plan I'll be supervised by [Dr. Julie Greensmith](http://www.cs.nott.ac.uk/~jqg/ "Dr. Julie Greensmith"), which I'm really looking forward to.

I'll be using an app called [Celery](http://celeryproject.org/ "Celery Distributed Task Queue") to handle the background tasks, which will be simple "pings" to the client machines, telling them to start a backup. Celery uses AMQP, provided by a broker such as [RabbitMQ](http://www.rabbitmq.com/ "RabbitMQ Enterprise Messaging System") (which is written in Erlang). It's usually used for processing intensive computational tasks, but can also be put to use for jobs that need executing on a periodic basis, like backups.

I can tell you're getting too excited, so I'll stop. Rest assured, though, I will be writing about the discoveries I make along the way to finishing this ambitious project.
