--- 
layout: post
title: The Hosts File in Ubuntu
tags: []

---
In my last post, I talked about configuring Ubuntu for use with different proxy servers, and something became quite evident. It would have been nice to have a way of aliasing the IPs of those servers, so instead of typing the number out, one could simply type "proxy", for example.

This can be easily achieved using the **hosts** file, which is present on both linux and windows operating systems. Here I will discuss how to use the hosts file in Ubuntu linux.

First, you must gain access to the file. It can only be written to as root, so the **sudo** command must be used, in conjunction with your favourite editor. For example:
<pre lang="bash">sudo gedit /etc/hosts</pre>
As you can see, the hosts file is located at _/etc/hosts_.

To add entries to the hosts file, simply add another line at the end, following the president set by the one or two lines alreay present. The syntax is basic, and consists of the IP to be aliased, then the name that you would like to alias it with. For example, the proxy in my last post could be entered as follows:
<pre>128.243.253.119 proxy</pre>
As it happens, this is actually a surprisingly effective method of blocking malicious websites, and even adverts. Quite simply, any domain that you would not like your browser to access, can be added to the hosts file with an IP of **127.0.0.1**. This is the IP address for the local machine you are currently working on. For a huge list of such websites, check out [www.someonewhocares.org/hosts](http://www.someonewhocares.org/hosts). Simply copy and paste the file on that site into your hosts file, and you're set. It should be noted that this will work on windows, Mac or linux. On Windows XP/Vista the hosts file is located in:
<pre>C:\Windows\system32\drivers\etc\hosts</pre>
Obviously if your Windows installation is on a different drive, simply replace C: with the relevant letter.
