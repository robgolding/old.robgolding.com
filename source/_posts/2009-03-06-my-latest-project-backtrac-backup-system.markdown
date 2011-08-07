--- 
layout: post
title: "My Latest Project: BackTrac Backup System"
tags: 
- django
- programming
---

My life has been pretty busy as of late, mostly with a new project I am working
on called BackTrac. I am developing a network backup solution, written entirely
in Python - using the Django framework as a front-end web interface.This came at
quite a good time, as I've been asked to give a presentation on any highly
technical topic, on which I know my stuff. The plan is to use BackTrac as a
base, and to explain the technical concepts behind the system that make it work.
The things I am going on concentrate on are:

	- Pyhon in general

	- XML-RPC

	- Filesystem hardlinks

	- The MVC concept, and Django


This should be enough content to fill a 15-minute slot, I hope.

Now I'll explain a little bit about how BackTrac works, for the benefit of those
not coming to the presentation.

What I wanted to achieve with BackTrac was a smart backup system, that doesn't
necessarily have to be the fastest. I wanted a very detailed web interface, with
scheduling capabilities and log view. I decided on Django for this, seeing as I
had already decided on Python for the system itself.

The nodes in the system use XML-RPC to communicate, and SMB for the actual
file-copying. The real bonus of this system is that it takes advantage of a
little-understood feature, present in most modern filesystems, called hardlinks.
Hardlinks enable the user to essentially point to the same piece of data on a
disk from more than one position. Essentially the concept is that if a file has
been backed up before, why back it up again? Just create a link or "shortcut" (a
hardlink) to the previously backed-up file. This is the basis for BackTrac.

Also, Django is turning out to be a real treat. This is the first time I have
used an MVC like Django, and I'm really enjoying it. Web development is exciting
again! I do agree with one point however, that Django makes the easy things
easy, and the hard things impossible. Not quite impossible in my case, but I've
had to do some pretty strange things to get Django to do what I want.

Finally, I've had the BackTrac project approved on Sourceforge.net, so watch out
- I'll be doing the initial import soon and getting some web content online.
First though, I have to decide on the best way to distribute the application.
This is a bit more complicated than it might otherwise be, because there are
three different aspects of the system - the client application, the server
application, and the Django project. On this topic or any other, as usual,
opinions are most welcome.
