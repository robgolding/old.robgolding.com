--- 
layout: post
title: Out with Exchange, in with WSUS!
tags: []

---

Up until yesterday, my Exchange server was my DC - they were one and the same.
As anyone will tell you, this isn't a particularly desirable configuration. For
one thing, if you want to demote the DC, you have to uninstall Exchange first,
and that means lots of migration and replication and...well it's not very nice!

So, I finally took the big step, and migrated all the Exchange data off the DC,
and uninstalled it. So now I have a seperate Exchange server, which means tons
more RAM free on the DC.

This also allowed me to install something I've been looking at for quite some
time now - WSUS 3. I now realise how useful this piece of software can be. I am
only managing about 5 computers, plus a few servers, but this makes keeping the
machines up-to-date so much easier. Plus, you get lovely graphs like this:

[{% img http://robgolding.com/wp-content/uploads/2007/09/wsus.png %}][wsus]

Also worth mentioning, is that the Exchange server I've been telling you about
actually runs as a virtual machine under VMware on the DC. It has 2GB of RAM,
and seems to be coping fine, but with 1GB assigned to the Exchange VM, and 384mb
assigned to another VM I have running on there (a web server), task manager
seems to be having problems getting the memory details correct:

[{% img http://robgolding.com/wp-content/uploads/2007/09/vm-ram.png %}][ram]

So we have 1GB + 384mb + whatever else is running on there (WSUS, DC, DHCP etc),
and we end up with 1.0GB (or there abouts). Something's not right. Alas, the
server seems to be handling the load fine, and with a gig of RAM apparently
free, I have space to expand in the future. Brilliant.

[wsus]: http://robgolding.com/wp-content/uploads/2007/09/wsus.png "WSUS Graphs"
[ram]: http://robgolding.com/wp-content/uploads/2007/09/vm-ram.png "Virtual Machine RAM Usage"
