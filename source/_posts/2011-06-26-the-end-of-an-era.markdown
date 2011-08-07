--- 
layout: post
title: The End of an Era
tags: 
- django
- university
- programming
---

Well, here we are. Three years at [The University of
Nottingham](http://cs.nott.ac.uk "The University of Nottingham Computer Science
Department"), and what do I have to show for it? Well, this is what:


{% img http://cdn.robgolding.com/uploads/2011/06/degree-result1.png
"Degree Result" %}




That's right! I have a _recommendation_ for a first in Computer Science. It
turns out both my supervisor and second marker were fond of my dissertation -
[Backtrac Backup
System](http://www.robgolding.com/blog/2010/05/25/3rd-year-project-back-to-backt
rac/ "3rd Year Project: Back to Backtrac") - and would like to see it used in a
commercial setting. I also had my work sponsored by [Servat
Ltd.](http://servat.co.uk/ "Servat Ltd. of Nottingham"), which went down very
well with the powers that be.

I've enjoyed my time at university, but I'm ready to move on now. Or at least,
I'd better be, after turning down a couple of _very_ good offers for PhD
studentships.

So what's next?

Why, I'm glad you asked! I'm currently working on a (secret) project with
[Marcus Whybrow](http://marcuswhybrow.net "Marcus Whybrow"), which will
hopefully shape up to be something quite special. It is, of course, a Django
project, and it's teaching us both about the finer details of developing and
deploying a production system. There's a few posts in the pipeline about the
things we've discovered this far, and we've hardly got going yet!

This time around, we've decided to go with a brand new web server all the cool
kids are using: [gunicorn](http://gunicorn.org/ "Green Unicorn"). It's sitting
behind an [nginx](http://nginx.net/ "nginx") reverse proxy, which also serves up
the static media. Also, [Fabric](http://fabfile.org "Fabric") is shaping up to
be a super time-saving tool, well worth writing about at some point in the near
future.

So, well done to all those who have completed their degrees this year. I look
forward to working with you in what people can't seem to stop calling the "real
world".
