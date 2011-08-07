--- 
layout: post
title: Mercurial on University of Nottingham Computer Science Servers
tags: []

---
{% img left http://cdn.robgolding.com/uploads/2010/03/mercurial.png Mercurial %}

For a while now, I've been using Subversion to keep track of personal projects
and coursework at university. SVN is installed on the UNIX servers as Computer
Science, so it's a relatively trivial process to get up and running with
a repository when I start a new assignment.

Recently though, I've been looking in to Mercurial as a more modern
alternative. Some things about Subversion are really starting to annoy me, like
the inability to ignore files on a repository-wide level easily (I work with
Python all the time, so .pyc files can really get on my nerves!). Mercurial
seems slicker, and I'm interested in the concept of
[DVCS](http://en.wikipedia.org/wiki/Distributed_revision_control "Distributed Revision Control"), as opposed to the classic client-server way of thinking.

However, Mercurial is not installed on the (rather ancient, now) Solaris
servers at university. I really admire the job that the sysadmins do, so I am
in no way condemning them for it not being installed. Instead, I set out to get
it compiled and working myself.

Installing it on the servers, it seemed, was the easy part. I followed
the
[installation instructions](http://mercurial.selenic.com/wiki/UnixInstall#Solaris_10_.28Sparc.29 "Installation Instructions for Solaris 10 (Sparc)") (making sure to include the
`LD_LIBRARY_PATH` variable) and all was well.

Cloning from the server, however, wasn't so easy. The problem lies in the fact
that Mercurial is now installed for me, and me alone. The hg binary lives in my
home directory, not on the server's main path (i.e. the `/usr/bin/` directory).
Therefore, I needed to tell the client exactly what command to run on the
server. If anyone else is having the same trouble, the command that I finally
came up with looks like this:

{% codeblock fixhg.sh %}
hg --config ui.remotecmd="LD_LIBRARY_PATH=/usr/sfw/lib PYTHONPATH=~/lib/python ~/bin/hg"
{% endcodeblock %}

I then aliased this command to `hgtuck` to save my sanity (the server is named
tuck, after Friar Tuck in the Robin Hood legends), and I now have a perfectly
working Mercurial install!

If anyone else is looking to do something similar, then I hope this little tip
saves you some time!
