--- 
layout: post
title: Roaming Firefox Profiles
tags: []

---

{% img http://cdn.robgolding.com/uploads/2009/04/firefox-logo.png "Firefox" %}

As a sort of follow-on from my last post, I thought I'd write a little about
the latest little addition to my system - roaming Firefox profiles. This is
something I had always considered to be almost impossible to achieve, without
complicated logon and logoff scripts that synchronise the correct folder(s) to
give the same effect as a 'redirected' profile. Well I discovered a much easier
way to achieve actual *real* profile redirection, when browsing around the
features offered by Group Policy Client Side Extensions.

The basic idea is that Firefox has a file called **profiles.ini**, which takes
care of all the configured profiles, and where they are stored. I used this file
to change the default profile location to within the user's home directory on
the file server. I had to use a home drive, mapped to the root of my users'
folder redirection directory on the server, as I presumed UNC paths were
unsupported in the .ini file. I did this with a GP Preference drive map using
the **%USERNAME%** variable, and an amazing feature of GP Preferences - parsing
and even _altering_ ini files. You can specify which section of the ini file you
are interested in, and which key you want changing. How useful! My policy looks
like this:

{% img http://cdn.robgolding.com/uploads/2009/04/firefox-gp-policy.png "Firefox Group Policy" %}

You can probably work out what's going on here, but I'll give a quick overview.
Basically, you specify the ini file to edit - in this case it's
**%APPDATA%\Mozilla\Firefox\profiles.ini**. Using the **%APPDATA%** variable
means that it will always resolve to the correct location in the user's local
profile, whether they are on XP or Vista. Then we specify the section of the
ini file - I'm interested in **Profile0**. This is the only profile present by
default, but allows users to have multiple profiles if they wish without
overwriting their settings when they log off. Finally, you specify the key to
change, and what to change it to. I rename the default profile to Firefox, and
change it's location to **H:\Firefox**. Also, this path is not relative, so
I have to change **IsRelative** to **0**.

OK, so now we have the Firefox profile location sorted, we need to make sure
that folder exists - or Firefox will just overwrite our changes and make it's
own folder in the default location. It's easy to use GP Preferences for this as
well - as there's a **Folders** preference category. So I just made a new
folders preference for **\\zeus\UserData\%USERNAME%\Firefox**, with the action
of **create** (*zeus* is my main DC and File Server). I used the UNC path to be
sure that the folder is created, even if the drive map hadn't come into effect
when this preference was applied. Also, a little trick I had to pull here was
ticking the checkbox titled **Run in logged-on user's security context** on the
**common** tab. This is because only the user has permissions on their home
directory, so this preference needed to run in the context of that user for it
to work successfully (without access denied errors).

Once this was finished, the system started to work flawlessly. I copied the
contents of existing Firefox profiles to the newly created directories, and they
were picked up by Firefox with no problems. New users get blank profiles as
expected, but they are stored on the file server instead of the local machine.
One little issue I have encountered is that a user can't logon at more than one
machine, and start Firefox - as the program can't lock particular files in the
profile. This just results in a message saying this Firefox is already running
though, which is pretty much correct (and I can't see why this would ever cause
problems for the user).

The last trick I employed, to make things a little speedier and to reduce
unnecessary file server traffic, was to disable disk caching on the roaming
Firefox profiles. To do this, I used a file policy in GP Preferences to copy a
tiny user.js (Firefox's preference file override) which contained only one line:

{% codeblock firefox-pref.js %}
user_pref("browser.cache.disk.enable", false);
{% endcodeblock %}

This turns off disk caching completely, which will not only save space on the
file server, but should speed things up as well. I hosted this file elsewhere on
the file server, and told the file policy to simply copy it into place, within
the user's Firefox profile. Here's the preference:

{% img http://cdn.robgolding.com/uploads/2009/04/firefox-userjs-gp-pref.png "Firefox UserJS Group Policy" %}

So there you have it, my technique for enabling roaming Firefox profiles. If
you've achieved the same through a different method, or have any ideas on this
this could be improved, then I'd love to hear how - feel free to comment on
this post.
