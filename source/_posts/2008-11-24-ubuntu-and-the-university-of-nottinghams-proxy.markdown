--- 
layout: post
title: Ubuntu and The University of Nottingham's Proxy
tags: []

---

{% img http://cdn.robgolding.com/uploads/2008/11/nottingham-ubuntu.png %}

I have recently enrolled on a Computer Science course at The University of
Nottingham, and as such have had to make sure my machines correctly use their
proxy server for web access. This post outlines the process of configuring
Ubuntu for exactly that purpose - and could be applied to any network with a
similar layout.

Network Proxy
-------------

First and foremost, Ubuntu has a setting in gnome for the Network Proxy, which
should set gnome's proxy - but I can't see as it affects anything at all -
still, better to be safe than sorry.

At Nottingham University, the recommended configuration is a proxy
auto-configuration script (_proxy.pac_) which is downloaded by the client and
parsed to configure the appropriate proxy server. In this case, it is located
at `http://wwwcache.nottingham.ac.uk/proxy.pac`. This URL is entered into the
correct field of the gnome Network Proxy settings dialogue.

{% img http://cdn.robgolding.com/uploads/2008/11/network-proxy-300x241.png "Network proxy" %}

Terminal Proxy
--------------

Secondly, the terminal has a proxy configuration option, so that programs that
run inside the terminal making HTTP requests can access the internet - namely
**wget **and **aptitude**. This is slightly more difficult to configure than
the previous, and is achieved like so:

The terminal proxy is set using a variable called _http_proxy_, which is set
using the `export` command, i.e.
`export http_proxy="http://proxy_server_ip:port"`

In this case, the proxy server's IP and port for the SNS (Student Network
Service) is `128.243.253.119:8080`.

This change can be made permanent by editing `/etc/bash.bashrc`, and adding the
above line to the end of the file. Otherwise, the change is only effective in
the terminal window currently open by the user, and disappears when it is
closed.

Synaptic
--------

Lastly, Synaptic Package Manager must have the proxy set, in order to update
your installation using the in-built Update Manager or Synaptic GUI. This is
done by opening Synaptic, and choosing Settings, Preferences, and setting the
above proxy information using the Network tab. Unfortunately Synaptic cannot
read auto-configuration scripts, so the IP and port must be manually entered
here.

{% img http://cdn.robgolding.com/uploads/2008/11/synaptic-proxy-300x266.png "Synaptic proxy" %}

Obviously once all this is done, Firefox must be configured to use the correct
proxy - but I trust you know how to do that! This turns out to be quite a pain,
so I'll be looking at ways to do this in one fell swoop. If anyone has any
suggestions, then please let me know.

*Note: These changes can be made much less painful by adding an entry to the
hosts file for each of the proxy IPs you need to configure - so you only need
to type, for example, `proxy`, instead of the entire IP. I will document this
process shortly in a separate post.*
