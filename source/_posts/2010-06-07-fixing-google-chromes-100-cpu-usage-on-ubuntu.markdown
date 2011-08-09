--- 
layout: post
title: Fixing Google Chrome's 100% CPU Usage on Ubuntu
tags: 
- ubuntu
- google-chrome
- linux
---

I've been battling with this issue for a couple of months now, ever since
I upgraded to Ubuntu 10.04 on my main desktop machine. Google Chrome would
constantly max out one core, with no indication of what it was doing, or why.

Today, I finally solved the issue. It turns out that something to do with the
`m4` package was causing the error, with the following output:

{% codeblock %}
MozPlugger: Error: Failed to execute m4.
Mozplugger: M4 parsing of config generated error
{% endcodeblock %}

So, I checked out the package in question, and found that it wasn't installed.
To my great relief (and some frustration), a simple `sudo apt-get install m4`
fixed the problem instantly.

I only hope that this post solves this extremely annoying issue for someone
else.
