--- 
layout: post
title: Exchange 2007 Autodiscover Issues
tags: []

---

{% img http://cdn.robgolding.com/uploads/2009/07/exchange2007logo.jpg "Exchange 2007" %}

Over the past week, I've been upgrading my e-mail system to Exchange 2007 (I
was previously using Exchange '03). For me, it's very useful to have the
systems that I write about, and consult for, installed at home in
a "semi-production" manner. It means that I have a system to work on, and I am
concerned with keeping it up and running as smoothly as possible - which
introduces me to intricacies that I would not otherwise encounter, if I were
just running a little test lab.

For example, when setting up Outlook Anywhere (the rebranded RPC/HTTP feature of
Exchange, allowing Outlook users to connect from outside the organisation) I
discovered a lot of "Sync Issues" appearing in my Inbox. The messages all had a
common theme:

{% codeblock %}
11:19:07 Synchronizer Version 12.0.6315
11:19:07 Synchronizing Mailbox 'User'
11:19:07 Synchronizing Hierarchy
11:19:07 Done
11:19:09 Microsoft Exchange offline address book
11:19:09  Not downloading Offline address book files. A server (URL) could not
be located.
11:19:09 0X8004010F
{% endcodeblock %}

Clearly, something was wrong with the Offline Address Book. I was only getting
these messages when using Outlook Anywhere, however, so this issue was
obviously specific to RPC/HTTP.

Looking up the error code, I found that the problem I was experiencing was
**very** common, but that nowhere seemed to have the ultimate repair. The
information available was sparse, and I had to put together my own solution
- which I will document below.

First, I registered an extra DNS (A) record for my email domain, called
"autodiscover". I must be clear here, that this is for the mail domain, not for
the domain used to access your OWA site. For example (and we'll go with the
Microsoft classic here), if your users have addresses such as_
user1@contoso.com_, _user2@contoso.com_ and you access your OWA via
_https://mail.contoso.com/owa_, then you need to register an A record for
**autodiscover.contoso.com**.

Next, I prepared a new certificate request, that would hopefully end up with me
obtaining a certificate that I could use to replace the current one, which would
be valid for both _mail.contoso.com_ and _autodiscover.contoso.com_ (to continue
with our example) - so that my Outlook clients could successfully access the
autodiscover service, and download the OAB. To do this, I used the following EMS
command:

{% codeblock newcert.ps1 %}
New-ExchangeCertificate -domainname mail.contoso.com, exchange.contoso.local, autodiscover.contoso.com -Friendlyname "Contoso Exchange

CAS SAN Certificate" -generaterequest:$true -keysize 1024 -path c:\certrequest.req -privatekeyexportable:$true subjectname "c=GB o=contoso inc, CN=mail.contoso.com"
{% endcodeblock %}

This command requires a little explanation. The_ -domainname _switch is used to
specify a list of addresses for which this server is valid. This is called
a **SAN **(Subject Alternative Name). Not all CA's support SANs, but Windows
Server 2008's CA Services does, which I will come back to later. Next, we give
the certificate a "Friendly Name", which is just a reference for you, the
administrator. Then we specify that we are looking to generate and save
a request, and that we want to be able to export the private key. The **Subject
Name** is important, but also slightly confusing. You must specify your country
code (US, GB, ES), your organisation name, and Common Name (**CN**) - which is
the most important one. This must be the URL used to access the SSL service
using a web browser, so mine was _mail.contoso.com_.

Once this request is saved, I passed it on to my CA to get the certificate
issued. If you're using a 3rd party CA (like VeriSign), then you'll have to
check first whether they support SANs. I use self-signed certificates, and my
CA is running Windows Server 2008, which does support SANs, so I issued the
request internally. This is done by accessing the CertSrv website, at
`http://servername/certsrv`, and clicking the "Request a Certificate" link.
Then, I chose "Advanced Request", and pasted the request file's contents into
the box, and picked the "Web Server" template.

This presented me with a downloadable certificate, which I saved locally in CER
format on the exchange server. Then I used the following command to import the
certificate:

{% codeblock importcert.ps1 %}
Import-ExchangeCertificate path <certificate>
{% endcodeblock %}

Once the certificate was imported, I enabled it for use with exchange. A similar
command is used for this:

{% codeblock enablecert.ps1 %}
Enable-ExchangeCertificate
{% endcodeblock %}

This prompted for a list of services, where I entered _IMAP, IIS, SMTP_ as these
are the default installed services. Only IIS actually gets used here, so I
shouldn't worry too much about this one. If you're not sure, then just enter the
same as me. Lastly, it asked for a thumbprint, which I copied and pasted from
the output of the import command. Finally, after accepting the confirmation, the
certificate was enabled.

And that was it. Both OWA and Outlook Anywhere are now working perfectly, and
hopefully this post will help at least one other lost soul with the same
problem!
