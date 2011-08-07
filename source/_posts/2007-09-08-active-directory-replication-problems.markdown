--- 
layout: post
title: Active Directory Replication Problems
tags: []

---

One word, or at least one acronym: GUID.

Background: I manage a multi-tree forest, with two trees, one in each of two
sites. They are connected by a slow site-to-site VPN link, over which all AD
replication takes place.

The domain controller at the forest-root-domain needed rebuilding, as the
operating system was installed on a flaky single disk, and was due to be moved
to a RAID1 array. So I thought it best to promote another DC, transfer all FSMO
roles, rebuild the first, and transfer the roles back. This process went
swimmingly, and the first DC was back online in no time.

However, when it came to the second site, it seemed that no replication
whatsoever was taking place. After delving into AD with tools such as adsiedit
and replmon,  I discovered that the second DC had not "heard" about the rebuild
of the first. This meant that the GUID had not been updated to hold the value of
the newly installed server. The fact that I had used the same name as before
didn't help the situation at all.

In the end, it was clear that I would have to either restore the original DC
from a System State backup, or rebuild the second domain from scratch. I chose
the latter, as it was a small domain, and wouldn't take very long. Now the
process is complete, and we have a fully functioning forest again (after lots of
metadata cleanup and /forceremoval's!).

I won't forget this one in a hurry - allow time for big changes to replicate
before making more big changes!
