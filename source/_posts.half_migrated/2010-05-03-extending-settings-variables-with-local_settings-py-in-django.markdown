--- 
layout: post
title: Extending Settings Variables with local_settings.py in Django
tags: 
- django
- programming
- python
---
I discovered this hacky way to use the local_settings trick to extend and/or override values in the main Django settings file today. Some projects use a "reverse" version of the local_settings trick (which is explained below), whereby the **main** settings file becomes _settings_local.py_ or something similar, which first imports _settings.py_, and then extends or overrides the values as required.<!--more-->

I didn't want to change the name of the project settings file to _settings_local_, however, as it would mean changing the WSGI file on every server that the project runs on.
### The local_settings Trick

It's a well-known trick to use a file called _local_settings.py_ or something similar, with a piece of code at the bottom of the main settings file:
<pre lang="python">try:
    from local_settings import *
except:
    pass</pre>
to override the value of settings variables. This will not work, however, if you wish to _extend_ a settings variable (for example, adding an app to INSTALLED_APPS). For this, I have found that the following ugly hack seems to do the job.
### The Ugly Hack

Replace the snippet at the bottom of the main settings file with the following code
<pre lang="python">try:
    LOCAL_SETTINGS
except NameError:
    try:
        from local_settings import *
    except ImportError:
        pass</pre>
What this is doing is effectively checking for the presence of a variable called LOCAL_SETTINGS. The local_settings file then contains this code:
<pre lang="python">LOCAL_SETTINGS = True
from settings import *</pre>
This means that the local_settings file will only be imported once, and it has all the variables in the main settings file available to extend at will. For example, to enable the Django debug toolbar:
<pre lang="python">MIDDLEWARE_CLASSES += ('debug_toolbar.middleware.DebugToolbarMiddleware',)
INTERNAL_IPS = ('127.0.0.1',)
INSTALLED_APPS += ('debug_toolbar',)</pre>
The code is ugly (and results in the main settings file being parsed _twice_) but it works!
