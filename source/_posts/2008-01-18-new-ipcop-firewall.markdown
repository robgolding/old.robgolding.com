--- 
layout: post
title: New IPCop Firewall
tags: []

---

My latest project, to replace the bulky overpowered ISA firewall on my home
network with a lean mean IPCop machine, was declared a great success a few days
ago.

{% img left http://cdn.robgolding.com/uploads/2008/01/ipcop.jpg %}

I am familiar with IPCop, as I used to use it a long time ago. Since then it
has matured somewhat, but the feature set is pretty much the same as I remember.
The new machine is a 400MHz PII, with 192mb RAM. It is sitting in the place of a
Sempron 3000+ with 1GB RAM. Amazing, it's doing the same job with a fraction of
the power. And also, it uses a third of the electricity - 30W in total. Good
news given the rise in energy prices!

The main challenge so far, which I still haven't overcome, is how to get
RoadWarrior VPN working, using the windows built-in VPN client, with L2TP/IPSec.
This used to be trivial with ISA Server, but this isn't quite the case with a
linux firewall. I have been looking at other distributions such as monowall and
pfSense, niether of which seem to spell out their ability to achieve this
clearly. I am playing with a few of these on Virtual Machines, so hopefully I
will come accross a way to do this before long - I'm starting to miss my
RoadWarrior VPN server. How sad, eh?
