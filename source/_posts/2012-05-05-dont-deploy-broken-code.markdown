---
layout: post
title: "Don't Deploy Broken Code"
date: 2012-05-05 19:43
comments: true
categories: 
---

It's been rather quiet around here lately, and that's mainly due to the fact
that I have a new job. I'm working as a Django application developer for a
company based in Leicester.

My job involves taking care of pretty much all the web-related technologies
I have blogged about previously, and it challenges me every day.

Since I started in February, I have put into place the software and processes
I described in my 3-part series about Django in Production. In this post,
though, I'd like to describe an extra nifty little trick I added to the
deployment process: checking the project's build status in Jenkins before
allowing a deploy to continue.

By performing this check before every deploy, we can reduce the risk that
potentially broken code ever touches the production environment. Of course, its
effectiveness is very much reliant on the quality and coverage of the unit
tests but, as they say, every little helps!

<!--more-->

As I describe in [Django in Production: Part 3][dip-part3], Fabric makes
a great tool for automatic deployments (though there is
[an argument][native-deployments] for *not* using this process, excellently
described by Hynek Schlawack). To help reduce the risk of making an
embarrassing mistake, an additional step can be added to a Fabric script which
prevents code being deployed unless the build is currently succeeding:

{% codeblock check_build_status.py %}
def check_build_status():
    import json, urllib2
    from fabric.colors import green, red
    from fabric.utils import abort
    data = json.loads(urllib2.urlopen(env.jenkins_api_url).read())
    project = env.jenkins_project
    for job in data['jobs']:
        if job['name'] == project:
            status = job['color'] == 'blue'
            if status:
                print(green('Build \'%s\' is passing' % project))
                return
            else:
                print(red('Build \'%s\' is FAILING!' % project))
    else:
        print(red('Could not find project \'%s\' in Jenkins' % project))
    abort('Aborting deploy.')
{% endcodeblock %}

This can easily be hooked into your deploy process by simply calling the
`check_build_status()` function - as it will automatically abort the deploy if
the build isn't passing or the project can't be found. I included it in the
function which runs `git pull`, so I can still perform tasks like reloading
the application server when the build is broken.

[dip-part3]: /blog/2012/01/14/django-in-production-part-3---automation-and-monitoring/
[native-deployments]: http://hynek.me/articles/python-app-deployment-with-native-packages/
