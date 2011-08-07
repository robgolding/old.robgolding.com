--- 
layout: post
title: Upgrading to Ubuntu 8.10 (Intrepid Ibex)
tags: []

---

The next incarnation
of the Ubuntu linux distribution came out at the end of this week, and whilst
typing this post, I am upgrading my laptop's operating system. Unfortunately, I
was unable to upgrade in the way that is described on the website,
instead my update manager seemed to think that the system was completely
up-to-date, even after multiple refreshes. Therefore, I'm just upgrading the
old-school way, by running the following commands:

{% codeblock upgrade.sh %}
sudo apt-get update
sudo do-release-upgrade
{% endcodeblock %}

So, hopefully the improvements to gnome, and the new Dust theme will go down a
treat when the upgrade's finished. Personally I'm looking forward to the new BBC
iPlayer integration with Totem, Ubuntu's media player - and hoping for some
improvements to Microsoft Exchange connectivity within Evolution, the mail
client.

I shall keep the world up-to-date, as this journey continues!
