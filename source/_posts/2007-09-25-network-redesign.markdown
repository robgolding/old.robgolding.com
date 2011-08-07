--- 
layout: post
title: Network Redesign
tags: []

---

Okay, so the network that I have been managing for some time now has just
undergone a pretty big redesign. Its actually a home network, but it spans 2
sites  my house and my friends house. They are joined by a site-to-site VPN
connection, which gives us a load of benefits like easily sharing photos,
programs, and an AD/Exchange forest.

{% img left http://cdn.robgolding.com/uploads/2007/09/network-diagram-public-thumbnail.png %}

Up until recently, the network was running with just one physical server at
each site, we shall call them Site A and Site B, each with [VMware Server][vms]
installed. Both servers were configured almost identically, with the host
machine running AD and Exchange, and a VM running ISA Server 2006 for the
firewall/VPN. Another VM was used for hosting some websites in the Perimeter
network.

The redesign saw one new server in at Site A and two new servers at Site
B although the main server at Site A has been upgraded significantly. The new
servers were installed to take the firewall away from the Virtual Machine to
a physical one  as this is much more secure. Also, the second new server at
Site B hosts Exchange, while this is now on a VM at Site A.

This network doesnt support many clients or users, but it used mostly for
educational purposes. For that it is perfect. We have a multi-tree forest AD
configuration, with one domain for each site (or each house!), and one Exchange
organisation spans the entire forest, with one Exchange server at each site.
This also helps if one server/network is down, as the other will pick up the
email for both sites  so we have a failsafe if one network is having problems.

I have published a public version of the network diagram, with external IP
addresses/names removed, just in case anyone might find it interesting. Just
click the thumbnail for a fullsize version.

As you may have noticed, I've used the names of gods from Greek and Roman
mythology for the servers. The web servers are the oldest ones there so they
haven't been renamed yet. Maybe an exiting project for the future!

Both networks now have a 20mb/784kb internet connection (up/down), so the VPN
link is essentialy 784kb/sec both ways. That's pretty good for things like AD
replication, but not brilliant for sharing files and photos.

The active directory is the aspect of the network I am most proud of. Since the
rebuild it has been working flawlessly, although I am forever looking at ways
to expand the directory. The DC at each site hosts a DNS zone for both domains,
which provides redundancy for DNS if one DC is down, and both servers hold
a copy of the Global Catalog. This allows for fast directory searches from both
sites, and gives each Exchange server a GC to look to.

The forest is split logically, as well as physically, into sites. This allowed
me to easily alter the replication schedule for the Domain Controllers,
although I decided to leave this at hourly intervals, as I saw no reason to
alter this value.

Hopefully the AD forest and network infrastructure will provide a solid base to
expand on, and I will post about any major additions to the network. At present
the clients consist of XP and Vista machines, but we are soon to aquire a new
desktop, which will be running Vista, that will make a nice addition to AD.

[vms]: http://www.vmware.com/server "VMware Server"
