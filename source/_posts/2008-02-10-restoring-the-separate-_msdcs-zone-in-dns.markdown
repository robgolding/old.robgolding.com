--- 
layout: post
title: Restoring the Separate _msdcs Zone
tags: []

---

Okay, so if you're anything like me - and like things to be done properly first
time (and also to look neat), then you'll know what I mean  when I talk about
the separate _msdcs zone in DNS on a Windows AD DNS Server. Of course, you have
to be a nerd, like me, to know what I'm talking about here also - but that's
assumed seeing as you're reading this blog.  I digress...

If you have ever reconfigured said DNS server, and recreated the DNS zones from
scratch, you'll know that the neat zone that keeps all the SRV records separate
from the oh-so-important A records, disappears - and gets put in a folder under
the usual domain root.

Well, I have a solution to the ever so pressing issue. Obviously the only way
anyone is going to risk breaking their whole Active Directory network will be
if, like me, they are so _totally_ OCD about this kind of thing.

So, if you're interested, I've written a short article on how to restore this
behaviour, and published it as always on maxms.net. If you think it might help
you out, then here's the link:

_Edit: link no longer available_

But remember, follow that article at your own risk!
