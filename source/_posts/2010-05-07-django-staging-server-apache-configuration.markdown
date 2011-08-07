--- 
layout: post
title: "Django Staging Server: Apache Configuration"
tags: 
- django
- programming
- python
- web development
- apache
- wsgi
---

In this post, I'd like to talk a little about how my staging server (called
Kaylee, after the
[character](http://en.wikipedia.org/wiki/List_of_Firefly_characters#Kaylee_Frye
"Kaylee") from [Firefly](http://en.wikipedia.org/wiki/Firefly_(TV_series)
"Firefly")), is configured to run the multiple
[Django](http://www.djangoproject.com/ "Django Project") projects that I have on
the go. A lot of the other articles I have read on this subject use
``mod_python`` but my server is configured to use [WSGI](http://wsgi.org/wsgi/
"WSGI"), so that's what I will be going over in this post.

<!--more-->
### Beginnings

Firstly, I use [virtualenv](http://virtualenv.openplans.org "virtualenv") to
keep the Python environments for the different projects separate, but that's a
topic in itself. This isn't a requirement, but it helps if you have dependencies
that you don't want installed system-wide, or projects that use different
versions of a particular package.

You can read more about virtualenv and Django
[here](http://www.saltycrane.com/blog/2009/05/notes-using-pip-and-virtualenv-dja
ngo/ "Notes on using pip and virtualenv with Django").

The directory structure for my ``webapps`` directory (which lives inside
``/home/``) looks something like this:
<pre>webapps/
|-- apache
|   |-- coral.conf
|   |-- coral.wsgi
|   |-- myuni.conf
|   `-- myuni.wsgi
|-- coral
|   |-- env
|   `-- repository
|-- myuni
|   |-- env
|   `-- repository</pre>
The two projects shown here are [Coral Issue
Tracker](http://bitbucket.org/robgolding63/coral "Coral Issue Tracker") and
[MyUni](http://bitbucket.org/robgolding63/myuni "MyUni"). The directory for each
project contains the virtual env for that project, and the repository. Alongside
the project folders is a directory called ``apache``, which holds two files for
each project: the WSGI file (``&lt;project-name&gt;.wsgi``) and the Apache
configuration file (``&lt;project-name&gt;.conf``).
### Apache Configuration

Then, in the main Apache configuration, I use a directive that includes all
these files automatically:
<pre lang="apache">Include    /home/webapps/apache/*.conf</pre>
The individual project configuration files then contain a set of simple
directives almost straight from the Django [deployment
documentation](http://docs.djangoproject.com/en/dev/howto/deployment/modwsgi/
"Using Django with Apache and mod_wsgi"). For example, the MyUni configuration
looks like this:
<pre lang="apache">WSGIScriptAlias  /myuni       
/home/webapps/apache/myuni.wsgi
Alias            /myuni/docs   /home/webapps/myuni/repository/docs/_build/html
Alias            /myuni/media  /home/webapps/myuni/repository/myuni/static</pre>
### WSGI Files

The WSGI files are also pretty simple, though there are a couple of things worth
explaining. Here is the WSGI file for the MyUni project:
<pre lang="python">import os
import sys
import site

os.environ['PYTHON_EGG_CACHE'] = '/tmp/myuni/egg_cache'

root_dir = '/home/webapps/myuni/env'

# with mod_wsgi &gt;= 2.4, this line will add this path in front of the python
path
site.addsitedir(os.path.join(root_dir, 'lib/python2.6/site-packages'))

sys.path.append('/home/webapps/myuni/repository')

os.environ['DJANGO_SETTINGS_MODULE'] = 'myuni.settings'

import django.core.handlers.wsgi

application = django.core.handlers.wsgi.WSGIHandler()</pre>
The ``site.addsitedir()`` call in the WSGI file adds the library path in the
virtual environment to the python path, by inspecting it to find all the
packages that are installed. I then append the path to the project's repository
to the path also, as is standard practice in this type of deployment.

The only other thing worth mentioning here, is the ``PYTHON_EGG_CACHE`` variable
that I add to the environment. This is required if you are using Python modules
that are packaged up as eggs, so that Apache can uncompress them in a directory
to which it can write (``/tmp/`` on Ubuntu is 777 by default). If you are not
using eggs, you can skip this line out altogether. Alternatively, you can
uncompress the eggs first:
<pre lang="bash">$ unzip [package].egg</pre>
You can read more about this issue, and the workaround,
[here](http://stackoverflow.com/questions/1855219/django-python-egg-cache-access
-denied-error "Django: PYTHON_EGG_CACHE, access denied error").
### Summary

That's pretty much all there is to it. This setup also makes it pretty easy to
deploy new versions of my projects, for which I have started using
[Fabric](http://docs.fabfile.org/0.9.0/ "Fabric v0.9"). I'll save that for
another post though.

If you do things differently, then I'd love to hear about how - so please leave
a comment!
