--- 
layout: post
title: VMware Server 2.0 Released
tags: []

---

It's arrived! The final release of VMware Server 2.0 was announced last week.
Will you be upgrading?

I'm not going to make the leap just yet. I've just upgraded my system (see
previous post), and so far it's been up with not a single hickup since
installation. I don't expect to be ripping up all my hard work for a while yet.

One thing that is interesting my at the moment - what are people's opinions on
the new console? Personally, I think the new console is "sort of OK". They have
the right idea - being able to manage the machine from a remote location without
installing client software first - afterall it is called VMware **Server**. This
makes sense in theory, but the console is so sluggish and, in my experience so
far, buggy - that it just doesn't work yet. Starting and stopping virtual
machines is slow, and the remote VM console takes a good number of seconds to
launch. The interface is vastly improved from the beta that I covered in a post
a long time ago, but still not perfect in my opinion.

Also, the console has its own Tomcat web server running on the host machine,
which eats up a large amount of RAM. This is annoying at best, and disastrous at
worst - when you need all the RAM you have for the Virtual Machines this
software is supposed to be managing!

Anyway, I may install this on a test bed somewhere, but certainly not on my
production system for some time. I'd love to know if I'm in the majority or the
minority on this one.
