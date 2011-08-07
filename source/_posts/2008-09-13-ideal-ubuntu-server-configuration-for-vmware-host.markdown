--- 
layout: post
title: Ideal Ubuntu Server Configuration for VMware Host
tags: []

---

I have recently upgraded my home server, shelling out on a new Core 2 Duo CPU
and 4GB of RAM for the machine. Having this much RAM means that in order to use
it, I had to install a 64-bit O/S. I chose Ubuntu Server 8.04 - and VMware
Server to host my virtual machines. I have put together a Word Document with
some notes on the issues and tips I came accros on the way, which could prove
invaluable to anyone taking the same approach as me. One of the main sources for
my research and tinkering ideas was a post on the VMware Community forums -
[http://communities.vmware.com/thread/146002][thread] - linked to within the
document.

This post made extremely interesting and informative reading - if one can
understand the material in that post, then a lot of load issues can be easily
resolved especially IOWait issues (my particular concern). My issue turned out
to be a mixture of the settings above, and Postfix misbehaving.

See the PDF [here][pdf].

[thread]: http://communities.vmware.com/thread/146002
[pdf]: http://cdn.robgolding.com/uploads/2008/10/ideal-ubuntu-server-configuration-for-vmware-host.pdf
