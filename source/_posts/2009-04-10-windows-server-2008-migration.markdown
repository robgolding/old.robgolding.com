--- 
layout: post
title: Windows Server 2008 Migration
tags: []

---

{% img
http://cdn.robgolding.com/uploads/2009/04/windows-server-2008.jpg
"windows-server-2008" %}



Thanks to the MSDNAA program, I'm able to try out the latest version of Windows
Server in the lab. I opted to migrate my domain accross to a new machine,
instead of performing an in-place upgrade. Personally I feel this is a much
safer bet, and tend to migrate domain controllers whenever I'm doing something
pretty major to a DC.

So far everything looks good, I've upped the forest and domain functional level
to Server 2008, so I can now take advantage of some of the new features - though
I'm yet to find out what they all are! The best thing so far (by a mile I'd like
to add) is the addition of Group Policy Preferences. Although it's annoying
having to install the Client-Side Extensions on every machine in the domain
(that is if WSUS isn't in use), the gains faw outweigh this bit of pain. I only
wish an MSI could have been released, so that it could easily be pushed out
using the existing Group Policy infrastructure. Never mind, eh?

{% img http://cdn.robgolding.com/uploads/2009/04/gp-preferences.png
"gp-preferences" %}

Anyway, on with the good! The new GP Preferences allow an administrator to
define, amongst others, drive maps for client machines, printer connections and
power options. As you may be thinking, this just about does away with the need
for logon scripts! Most, if not all of the common tasks that are performed with
logon scripts can now be done from a group policy object.

There are also a lot of changes to the way Active Directory works. In Server
2008, Active Directory Domain Services can be installed on a machine, without
actually making it a DC. What this means is that a standard server build can be
'sysprepped' with the files required for promoting the server to a DC, without
actually doing the promotion. Also, Read-Only Domain Controllers (RODCs) have
been introduced as a new feature. Essentially, an RODC just caches queries from
a normal DC, usually located at another site - apparently allowing for faster
logon times at remote sites with slow links. After discussion with a colleague,
however, the benefits of such a system are maybe not quite as advertised. For
example, only one RODC can be installed per site - so larger sites can't benefit
from the redundancy and load balancing offered by multiple DCs, if RODCs are
used. Also, the much-touted security advantages of using an RODC aren't as they
seem either, as the database can be just as easily written to, just through
another "normal" DC.

More on this later!
