--- 
layout: post
title: "Linux-Windows Integration (\xE0 la Likewise-Open & Winbind)"
tags: []

---

Recently, I've been doing a lot of work trying to integrate Linux and Windows
machines on a single network. The project begun with a need to allow windows
users access to file shares using Samba, and Linux users access to the same
data using NFS. As you may know, Samba (or more specifically the SMB protocol)
uses a username/password combination to authenticate users, while NFS uses user
IDs and group IDs (UIDs & GIDs) on the local machines to achieve the same end.
Therefore, I needed a way to consistently authenticate Windows domain users on
Linux machines, whilst maintaining a consistent conversion from SID to UID, and
vice-versa.

Domain authentication can be achieved relatively simply, by using either
[Winbind][winbind] (part of the Samba project) or the new kid on the block,
[Likewise][likewise]. Likewise-Open offers a simple procedure for joining
domains, and the new version comes packaged with it's own version of Kerberos.

It also hashes the Windows SID into a UNIX UID in a consistent manner - so the
UIDs are always the same across your entire environment. Sounds perfect, right?
Well, no. Not quite. The problem with Likewise-Open, is that it's difficult to
integrate with Samba. Though it does ship with a "compatibility module" called
**lwi_compat**, which allows Samba to hook into Likewise's authentication
module, I found this quite difficult to get working, and I only achieved
partial success through guesswork - as the documentation didn't actually help
much, given they only officially support Samba 3.0.x (while Ubuntu now uses
3.3.x).

When I did get it going, however, it only recognised the Windows users'
**primary** group, not any of the other groups they were members of. This meant
that my (possibly overly) complex system of ACLs and user directories just
didn't work at all. So, on to Plan B...

When I read through the short (but sweet) Ubuntu Wiki article entitled
[ActiveDirectoryWinbindHowto][wiki], I felt like somewhat of a fool after
reading a small, illusive section called _Adding more than one Linux machine to
a Windows network_. Bingo! This section described a problem whereby the
traditional Winbind domain authentication method would lead to inconsistent
UIDs across the network, and thus cause headaches when trying to achieve
anything like what I was aiming for. It suggested using a method of mapping
SIDs to UIDs called _RID_. I assume this stands for _Relative ID_, which is
another kind of ID Active Directory uses to track users within a domain. These
**can** possibly clash from domain to domain, so it is advised not to use this
method when your network contains a trust between multiple Windows domains, but
for the simpler setup (like my own) it's a godsend.

This meant that I could use RID mapping within Winbind, which is part of Samba
itself (so no troubles integrating those two), and achieve a consistent SID-UID
mapping scheme across the network, allowing me to finally enable access to the
file shares via. NFS. Amazingly, NFS "Just Worked" straight away, and I've
written some nice wrapper programs around **chown**,** getfacl** and
**setfacl** to set the correct owner and permissions on entire directory trees,
which saves a lot of time when your UIDs are changing as often as mine were!
I'm also using **autofs** to automatically map user's home directories on the
Linux machines, which has proven itself to be very useful. I just used static
_fstab_ entries to map the other "general" file shares, like _software_ and
_media_ - as I couldn't seem to get autofs direct maps working (apparently they
are only partially working in Ubuntu anyway, but it seems as though they are
completely broken to me).

On a side note, I've also just finished developing a Python-based rsync backup
program, which allows me to write a _very_ small script to backup remote
servers using rsync over SSH, and tar up the contents of all the servers into
one archive. This is really useful, as I have a lot of disparate locations on
different servers that all need to be pulled onto the backup drive every night.
Now though, I should really concentrate on some revision for the exams I have
after Christmas!

Happy holidays, everyone! (That's Merry Christmas and a happy new year, but
just between you and me).

[winbind]: http://www.samba.org/samba/docs/man/Samba-HOWTO-Collection/winbind.html "Winbind"
[likewise]: http://www.likewise.com/products/likewise_open/ "Likewise Open"
[wiki]: https://help.ubuntu.com/community/ActiveDirectoryWinbindHowto "ActiveDirectoryWinbindHowto"
