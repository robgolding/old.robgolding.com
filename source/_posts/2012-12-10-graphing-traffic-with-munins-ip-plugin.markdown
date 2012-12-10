---
layout: post
title: "Graphing Traffic with Munin's IP Plugin"
date: 2012-12-10 20:59
comments: true
tags:
- linux
- iptables
- munin
---

This is just a short post about using the Munin ip_ plugin to monitor traffic
to and from particular hosts using iptables.

There are a [number][post1] [of][post2] [posts][post3] that describe how to add
the necessary rules to iptables for counting traffic to each host that you're
interested in. Unfortunately, they all suggest using rules which are the wrong
way around. The correct rules should look like this (to monitor traffic to the
host at ``192.168.0.1``):

{% codeblock %}
-A INPUT -s 192.168.0.1
-A OUTPUT -d 192.168.0.1
{% endcodeblock %}

All the resources I found quoted the rules above with the ``INPUT`` and
``OUTPUT`` chains switched, meaning no traffic would ever be recorded.

[post1]: http://wiki.yobi.be/wiki/Munin#ip__plugin
[post2]: http://wiki.xdroop.com/space/munin/ip_+plugin
[post3]: http://baldric.net/2009/11/15/system-monitoring-with-munin/
