--- 
layout: post
title: Configuring Share Permissions
tags: []

---

When I was "starting out" in the IT field, I always used to setup shared folders
in a certain way - I would set the Share Permissions to **Everyone - Full
Control**, then use the NTFS Permissions to control access to the share - which
always seemed like the most simple and secure way to do things.

{% img left http://cdn.robgolding.com/uploads/2007/09/share-permissions.png "Share Permissions" %}

However, when talking to the Network Administrator at my college, I was informed
that setting the "Full Control" item was extremely bad from a security
standpoint, as it allowed any user in the specified group (in this case,
Everyone) - to change options in regard to the share configuration - like the
permissions themselves.

So from this point on, I changed my habits to setting the Share Permissions to
**Authenticated Users - Change**, and then using the NTFS permissions, as
before, to control access to the data. Today, however, I decided to do my
homework.

A quote from [this article][article] states the following:

_"The recommended permissions have been tested, and work correctly; but there
are alternative approaches. For example, some experienced administrators prefer
always to set share permissions to Full Control for Everyone, and to rely
entirely on NTFS permissions to restrict access."_

To me, it sounds like Microsoft have no reservations about using this method,
and certainly don't mention any security risks at all. So, hearing anyone else's
opinion would be very useful, but seeing as it works well - for now I will carry
on using the **Authenticated Users - Change** permission object.

[article]: http://technet2.microsoft.com/windowsserver/en/library/86987829-3f74-412f-abb8-c8b22b07257d1033.mspx?mfr=true "Permissions on a file server"
