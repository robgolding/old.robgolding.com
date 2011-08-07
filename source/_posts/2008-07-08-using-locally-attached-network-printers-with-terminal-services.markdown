--- 
layout: post
title: Using Locally-Attached Network Printers with Terminal Services
tags: []

---

If you work with Microsoft's Terminal Services on a regular basis, you've
probably come accross a printing issue at some point. The point of this post is
to provide a potential solution to the problem whereby locally attached network
printers cannot be used from a remote terminal services session. By locally
attached, I mean directly connected via. IP address, and not by way of a print
server. This is often the case with consumer grade network printers, or wireless
equivalents.

The concept is simple - attach the network printer as normal, then share it.
Then, connect to the share on the local machine - thus using the printer as it
is connect via a print server - with the server being the local machine. Here
are the steps required to achieve this:

- First attach the printer as normal. For the type of connection I am describing
  this is via. a TCP/IP port. So choose **New Printer** in Printers and
  Faxes, and then **Local printer attached to this computer**.

- When asked the type of port, choose TCP/IP, and enter the IP address of the
  network printer.

- Once the printer is connected, share it. This can be done from the wizard, or
  by using the **Sharing and Security** panel afterwards.

- Then, choose **New Printer** again, this time selecting a network printer.
  Then enter **\\<machine_name>\<share_name>** as the path to the
  printer. Obviously machine name is the name of your computer, and share name
  is the name with which you shared the printer in the previous step.

- Note: This step will not introduce a new printer item to your list of
  printers, so don't be alarmed when nothing new appears. Just connect to the
  terminal server as in the last step.

- Once this is done, connect to your terminal services session and wait for a
  minute or two for the printer to appear. Note that version 6 of the 
  **Microsoft Terminal Services Client** may be required for this to work.

Although it seems hacky, and untidy, this solution seems to work well, so I hope
this will help at least one person out there. Happy printing.
