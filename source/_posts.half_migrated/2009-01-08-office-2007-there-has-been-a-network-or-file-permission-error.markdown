--- 
layout: post
title: "Office 2007: \"There has been a network or file permission error.\""
tags: []

---
Recently I have been having a strange issue on my desktop PC, whereby saving an Office document (Word, Excel etc.) inside the My Documents folder - which is redirected to my file server - gives the following error:
<blockquote>There has been a network or file permission error. The network connection may be lost.
<var>(&lt;filename&gt;</var>)</blockquote>
I've been trying to find out the cause of this for some time now, while working round it by simply saving the file to my desktop and copying it over to My Documents.

Anyway, I have just found the problem. I had a little plugin installed that allowed the indexing service on Windows Vista to index a network location, meaning I could search the My Documents folder quickly. This was the cause of the issue, and removing the index solved the problem. I wanted to write the solution up here in the hope that this helps someone in the same situation.


Note: There have been other solutions to this problem cited, including anti-virus programs, and network congestion.The KB article for this issue is located [here](http://support.microsoft.com/kb/291156 "KB291156").

