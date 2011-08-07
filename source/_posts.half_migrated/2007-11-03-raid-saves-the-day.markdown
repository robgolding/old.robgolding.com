--- 
layout: post
title: RAID Saves the Day
tags: []

---

_Error occurred on Primary Master device on adapter 0. Primary Master - CDB 2a 00 01 58 aa 5d 00 00 01 00._

This is what I was greeted with in my inbox, as I started the day yesterday. It
looks like at some time around 4am, the primary drive on my main server, Zeus,
had been having a few problems to say the least. The drive gave about a hundred
of the above errors, and then dropped off the RAID1 array. I opened up the case
to see what was going on, and it would seem that heat was the killer, if
anything. I shutdown the server, moved the drives around a bit to allow for
better airflow, stuck an extra fan in, and booted it back up. The drive came
back to life, and is rebuilding as I type this.

I am now, however, looking at a new SATA controller and 2x80GB Western Digital
drives. Hopefully these will run cooler, faster, and allow for better airflow
than their IDE counterparts.
