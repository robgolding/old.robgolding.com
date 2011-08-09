--- 
layout: post
title: The Trials and Tribulations of Django + Git
tags: 
- django
- git
- programming
---

I just finished my last exam today - Web Programming and Scripting - which
explains the distinct lack of activity around here in recent times. Thankfully
I could end my exam season on a high, as web programming is, well, what I do
- so it wasn't too much of a challenge!

Something strange happens to me every time exams come around. I seem to pick up
new projects, and just run with them. This time, I've become involved with
a small group of people at university, writing a portal-style information
system for universities. I suppose most people call this behavior
procrastination, but I'm quite deeply in denial about that.

{% img http://media.djangoproject.com/img/badges/djangowish126x70.gif "Django Badge" %}

Ever since my post about the [Backtrac Backup System][backtrac-post], I've been
really enjoying using [Django][django]. Something about it just makes
developing for the web, well, exciting. That can only be good, right? I am the
designated server administrator for this latest project, mostly due to the fact
that I am the only one with a server to administer, and some of the things I've
learned so far seem worthy of a mention here. Firstly, we as developers were
- how can I put it - stepping on each other toes somewhat. The project at this
point had no version control, so we were just editing a bunch of files over
SFTP. Obviously, some sort of Source Control Management was in order. I did
some research, and decided that Git was a nice, modern alternative to the
ever-popular SVN. It also meant that my server was constantly backed up by
everyone on the team - but that's just a bonus!

So, I installed Git, and started a repository. A lot of effort went into
learning how the system works, and more importantly, how to make it work for
us.  Directed Acyclic Graphs thankfully made some sense to me, so I could just
about understand the documentation. I wrote some custom hooks, and a C Program
to synchronise the web server. I was happy, and absolutely certain that this
was the solution to all our woes. I was mistaken. Git just didn't work the way
I had hoped. The custom hooks were throwing permission errors all over the
place, and my development team (read: my friend Rob Miles) was locked out of
the repository. We made the decision yesterday to scrap Git, and go back to the
previous system of editing the files over SFTP. We are always in constant
communication when developing for the project, so it's not too big a deal, but
I feel that I failed as an administrator. You see, as an admin your job isn't
just to play with cool toys and loud servers - your primary purpose is to give
the users what they need to work, and that is most certainly not what
I achieved.

I'm glad I took the time to learn Git - and I'm sure it will help me later in
life, in some way or another. It's just unfortunate that it didn't work out the
way I had hoped for our project.

[backtrac-post]: http://www.robgolding.com/index.php/2009/03/06/my-latest-project-backtrac-backup-system/
[django]: http://www.djangoproject.com/
