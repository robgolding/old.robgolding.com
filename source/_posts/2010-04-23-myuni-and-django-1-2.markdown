--- 
layout: post
title: MyUni and Django 1.2
tags: 
- django
- programming
- myuni
- mercurial
---

[MyUni][myuni] is hailed as my "latest venture" on the front page of my
website. In fact, it was conceived early last year, when I was in my first year
at Nottingham University, by [Rob Miles][rskm] and [Ben Jenkinson][bxj]. We
were only just starting to get to grips with this new fad, called
[Django][django].

<!--more-->

The project has gone through several iterations since then, never really
reaching any form of completeness. This time round, we are using proper version
control (thanks to [Mercurial][mercurial] and [BitBucket][bitbucket]), and
trying to adhere to Django best practices as much as possible.

Straight from the repository website:

{% blockquote http://bitbucket.org/robgolding63/myuni %}
MyUni is a portal-style web application for Universities, offering students
a central location to access all the relevant information about their course,
modules, assignments, and staff members.
{% endblockquote %}

The project came about because of a passionate hatred for the WebCT system that
our university is using, and a general feeling that things could be (and should
be) better.

Django 1.2 will bring some really exciting [new features][django1.2], which
I am sure would make developing and using MyUni a better experience. One of the
things I'm most looking forward to using is the messages framework. I plan to
use it for a bunch of things, possibly including form error messages...maybe.

Now, if only the release date would stay in the same place for more than a week!

[myuni]: http://bitbucket.org/robgolding63/myuni/wiki "MyUni"
[rskm]: http://robertskmiles.com "Robert S. K. Miles"
[bxj]: http://benjenkinson.com "Ben Jenkinson"
[django]: http://djangoproject.com "Django Project"
[mercurial]: http://selenic.com/mercurial "Mercurial SCM"
[bitbucket]: http://bitbucket.org "BitBucket"
[django1.2]: http://docs.djangoproject.com/en/dev/releases/1.2/ "Django 1.2 Release Notes"
