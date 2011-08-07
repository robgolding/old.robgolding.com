--- 
layout: post
title: Server Cooling and Network Rewire
tags: []

---

OK, so seeing as the last post is about worrying errors reported by the RAID
controller on my main server, Zeus, I've decided to do something about the heat
that caused the errors in the first place.

The drive caddy in Zeus holds 4 drives, but they are far too close together, so
I've spread them out somewhat, and added another fan, although it took some
pursuation to get it in (by pursuation I mean using tinsnips to cut a hole in
the front of the server). This is proving to work really well, now I just need
to figure out how to get another exhaust fan in there somewhere.

Also, I have recabled the switch with colour-coded Cat5. We have red cables for
the internet VLAN, yellow for the perimeter VLAN, and blue for the internal
network. This has also been a good time to start wiring for gigabit, so I've ran
the first cable to my desktop from the switch. All I need now is a few gigabic
NICs and I'll be all set.

Update: I've got some pictures up now. We have zeus with its new ghetto fan mod
at the front, blowing nice cool air over a terabyte worth of data in the first
two. Then the new switch and the lovely neat wiring job in the last one. On a
side note, I'm really happy with the switch, it's proving worth the cost after
all. Looks pretty good as well sitting in the cabinet :). Now, if only I could
get round to painting the inside of that server cupboard. It's shameful!

[{% img http://cdn.robgolding.com/uploads/2007/11/zeus_big.thumbnail.jpg %}][1]

[{% img http://cdn.robgolding.com/uploads/2007/11/zeus-close_big.thumbnail.jpg %}][2]

[{% img http://cdn.robgolding.com/uploads/2007/11/switch_big.thumbnail.jpg %}][3]

[1]: http://cdn.robgolding.com/uploads/2007/11/zeus_big.jpg "Zeus with new fan installed"
[2]: http://cdn.robgolding.com/uploads/2007/11/zeus-close_big.jpg "Zeus with new fan installed (closeup)"
[3]: http://cdn.robgolding.com/uploads/2007/11/switch_big.jpg "New switch wiring with colour-coded CAT5"
