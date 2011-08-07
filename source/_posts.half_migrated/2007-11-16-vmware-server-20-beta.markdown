--- 
layout: post
title: VMware Server 2.0 Beta
tags: []

---



Seeing as Im getting into virtualization more and more recently, I decided to give the new beta of VMware Server 2.0 a go. I have a virtual machine for testing purposes on my desktop machine (a Dell Dimension 9200, E6600, 2GB RAM)  so this should prove a perfect test bed for the software.




![VMware Logo](http://cdn.robgolding.com/uploads/2007/11/vmware.png "VMware Logo")I have a particular personal interest in VMware. The server that so many of my posts have been about as of late runs VMware - it and a couple of virtual machines provide me with my email, directory, file servers and websites. Because of this, Im quite excited about the next release of VMware  and hopefully the transition wont be too difficult.




Starting with the interface, which is the first noticeable difference one the installation is completed  there is nothing that has been kept from version 1  a complete overhaul. Its now based totally in the browser  on port 8222 (8333 for https)  using its own installation of Tomcat to be precise. This brought back bad memories of managing Microsoft Virtual Server 2005 on a clients system  but once I started using the new interface, I began to like it more and more. Its definitely an improvement over Microsofts attempt, and with the browser plug-in for Virtual Machine Remote Control (VMRC), it provides all the functionality of the previous console and more; even in firefox!




I can definitely say that the performance of VMware itself has improved in the new version, although I am running it on a Vista installation. The only real reason for this noticeable change is that the previous release would hang for a number of seconds when a virtual machine was started or stopped in Vista. It feels as though this is an issue that has been addressed in the beta, and so hopefully performance overall will have had the same attention.




Just a few shots of the new UI, first we have the login screen for the web-based management interface. My desktop is joined to the domain at my house, and the only account I could use to login was the builtin domain administrator account. Strange.




[![VMware 2.0 Login](http://cdn.robgolding.com/uploads/2007/11/vmware-login.thumbnail.png)](http://cdn.robgolding.com/uploads/2007/11/vmware-login.png "VMware 2.0 Login")




Next, the summary screen which shows some info about the host machine, which will be nice on a production server as it details the RAM usage. This will be something I look forward to using on my main server Zeus:




[![VMware 2.0 Summary Screen](http://cdn.robgolding.com/uploads/2007/11/vmware-summary.thumbnail.png)](http://cdn.robgolding.com/uploads/2007/11/vmware-summary.png "VMware 2.0 Summary Screen")




Finally, a shot of the only VM I have on this machine. In this case the VM is stopped, and it shows the hardware configured for the machine, and the 1x2.338GHz CPU looks promising - I could assign another!




[![VMware 2.0 Virtual Machine](http://cdn.robgolding.com/uploads/2007/11/vmware-vm.thumbnail.png)](http://cdn.robgolding.com/uploads/2007/11/vmware-vm.png "VMware 2.0 Virtual Machine")




That's all I have on this for now, but if I find any more interesting stuff I'll be sure to post an update. If you're interested you can grab a copy from here: [http://www.vmware.com/beta/server/](http://www.vmware.com/beta/server/ "Vmware Server 2.0 Beta").

