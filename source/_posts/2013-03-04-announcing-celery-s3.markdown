---
layout: post
title: "Announcing Celery-S3"
date: 2013-03-04 13:22
comments: true
tags: 
- celery
- programming
---

TL;DR: Check out [celery-s3][celery-s3], it lets you store Celery task results
in S3.

Celery has good support for a variety of different message brokers -- RabbitMQ,
Redis, SQS, etc. -- but support for result storage is somewhat more limited.

This is particularly apparent if you're running Celery on an EC2 server and
wish to take advantage of the distributed services AWS provides. SQS works as
a message broker, but there's nowhere to store results (and using the AMQP
result store with SQS results in a [queue for each result][sqs-amqp-issue]).

[Celery-S3][celery-s3] lets you store Celery results in an S3 bucket, which
means you can run a fully-functioning Celery installation on AWS with nothing
but a Python install.

[celery-s3]: http://robgolding63.github.com/celery-s3
[sqs-amqp-issue]: http://docs.celeryproject.org/en/latest/getting-started/brokers/sqs.html#results
