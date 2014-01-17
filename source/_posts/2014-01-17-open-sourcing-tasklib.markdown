---
layout: post
title: "Open-Sourcing tasklib"
date: 2014-01-17 21:40
comments: true
categories: 
- programming
---

The past few months have been seriously busy, mostly due to launching [the new
Police.uk][new-police.uk]. In fact it's been so long since my last post that it
took me hours to get Octopress working again. I have found time to work on
a new Python library, though: [tasklib][tasklib].

It came about as the core part of a project to create a web-based interface for
[Task Warrior][taskwarrior], which I now use every day (though it's still very
much a work in progress).

<!--more-->

tasklib has an API that's very similar to Django's ORM, so developers who are
familiar with that will find it really easy to use.

There are a couple of Python libraries out there already which do the same
thing, but I wanted to achieve a few things with this project:

* A clean, simple API
* Great documentation
* Unit tests
* Keeping to the Taskwarrior [3rd-party applicationguidelines][guidelines]
  (don't try and parse the `pending.data` file!)

I've used this as an opportunity to do things the Right Way&trade;: keeping
to the [Git Flow][flow] workflow, and automated testing on [Travis CI][travis].
tasklib is tested on versions `2.2.0` and `2.3.0 beta2` of Task Warrior, and on
Python `2.6`, `2.7`, `3.2` and `3.3`.

[new-police.uk]: http://www.police.uk/news/welcome/
[tasklib]: http://tasklib.readthedocs.org/en/latest/
[taskwarrior]: http://taskwarrior.org/projects/show/taskwarrior
[guidelines]: http://taskwarrior.org/projects/taskwarrior/wiki/3rd-Party_Application_Guidelines
[flow]: http://nvie.com/posts/a-successful-git-branching-model/
[travis]: https://travis-ci.org/robgolding63/tasklib
