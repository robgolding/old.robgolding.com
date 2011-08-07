--- 
layout: post
title: phpBB3 WSOD (White Screen of Death)
tags: []

---

I don't know whether this is a popular or well-heard-of problem or not, but I
have come across this twice now. The phpBB forums over at maxms.net have
suffered the second "White Screen of Death" incident since I installed them.
Firstly, I disabled Gzip compression to fix the issue. This time though, it
wasn't so easy.

{% img http://cdn.robgolding.com/uploads/2008/11/phpbb3_bod.png %}

I ended up being forced to reinstall the same version of phpBB to another
directory on the server, into a new database. Then I renamed the new DB to
something different, and changed the old one to the new one's name. Phew! So in
the end, I had a new install of phpBB, but with the same data as before - and it
worked perfectly.

So if anyone else comes across this issue, firstly try disabling HTTP
compression on any device in front of the web-server (which for me was MS ISA
Server 2006). If that doesn't work, it looks like it has to be a reinstall - the
longest process will be renaming the DB's, however, so be careful.

Anyhow, the forums are now back up and running - at [forums.maxms.net][forums].
Hopefully this seemingly endless list of problems to fix is over now.

[forums]: http://forums.maxms.net "forums.maxms.net"
