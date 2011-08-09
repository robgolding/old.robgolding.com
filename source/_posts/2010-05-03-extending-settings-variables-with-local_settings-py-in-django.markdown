--- 
layout: post
title: Extending Settings Variables with local_settings.py in Django
tags: 
- django
- programming
- python
---

I discovered this hacky way to use the local_settings trick to extend and/or
override values in the main Django settings file today. Some projects use
a "reverse" version of the local settings trick (which is explained below),
whereby the *main* settings file becomes `settings_local.py` or something
similar, which first imports `settings.py`, and then extends or overrides the
values as required.

<!--more-->

I didn't want to change the name of the project settings file to
`settings_local.py`, however, as it would mean changing the WSGI file on every
server that the project runs on.

The Local Settings Trick
------------------------

It's a well-known trick to use a file called `local_settings.py` or something
similar, with a piece of code at the bottom of the main settings file:

{% codeblock settings.py %}
try:
    from local_settings import *
except:
    pass
{% endcodeblock %}

This allows you to override the value of settings variables. It won't work,
however, if you wish to _extend_ a settings variable (for example, adding an
app to `INSTALLED_APPS`). For this, I have found that the following ugly hack
seems to do the job.

The Ugly Hack
-------------

For this hack, replace the snippet at the bottom of the main settings file with
the following code:

{% codeblock settings.py %}
try:
    LOCAL_SETTINGS
except NameError:
    try:
        from local_settings import *
    except ImportError:
        pass
{% endcodeblock %}

What this is doing is effectively checking for the presence of a variable called
`LOCAL_SETTINGS`. The local settings file then contains this code:

{% codeblock local_settings.py %}
LOCAL_SETTINGS = True
from settings import *
{% endcodeblock %}

This means that the local settings file will only be imported once, and it has
all the variables in the main settings file available to extend at will. For
example, to enable the Django debug toolbar:

{% codeblock local_settings.py %}
MIDDLEWARE_CLASSES +=
('debug_toolbar.middleware.DebugToolbarMiddleware',)
INTERNAL_IPS = ('127.0.0.1',)
INSTALLED_APPS += ('debug_toolbar',)
{% endcodeblock %}

The code is ugly (and results in the main settings file being parsed _twice_)
but it works!
