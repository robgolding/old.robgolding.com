--- 
layout: post
title: Using Django Authentication with Twisted Perspective Broker
tags: 
- django
- python
- backtrac
- twisted
---
For my [third year project](http://www.robgolding.com/blog/2010/05/25/3rd-year-project-back-to-backtrac/ "3rd Year Project: Back to Backtrac"), I have hooked Twisted's Perspective Broker authentication into Django, so that a networked application can authenticate itself against Django's central user database (``django.contrib.auth``). The process is pretty quick and easy, thanks to Twisted's pluggable nature, and Django's pure simplicity.

<!--more-->
## The Checker

Firstly, we create a class which will actually do the "checking", to allow access (or not) given a username/password combination. That looks something like this:
<pre lang="python">from zope.interface import implements
from twisted.python import failure, log
from twisted.cred import portal, checkers, error, credentials
from twisted.internet import defer

from django.contrib.auth.models import User, check_password

class DjangoAuthChecker:
    implements(checkers.ICredentialsChecker)
    credentialInterfaces = (credentials.IUsernamePassword,
                            credentials.IUsernameHashedPassword)

    def _passwordMatch(self, matched, user):
        if matched:
            return user
        else:
            return failure.Failure(error.UnauthorizedLogin())

    def requestAvatarId(self, credentials):
        try:
            user = User.objects.get(username=credentials.username)
            return defer.maybeDeferred(
                check_password,
                credentials.username,
                user.password).addCallback(self._passwordMatch, user)
        except User.DoesNotExist:
            return defer.fail(error.UnauthorizedLogin())</pre>
This code is relatively self-explanatory, once you understand Twisted's way of doing things. An authenticated "user" in Twisted (though this might not actually be a person) is called an Avatar. The ``requestAvatarId`` method therefore takes a set of credentials, and returns the Django user object that corresponds to those credentials (or a failure if necessary). This return value is wrapped up inside a deferred, which is something that I won't go into detail about in this post. For more information on Deferred objects, this is one area where the [Twisted documentation](http://twistedmatrix.com/documents/10.1.0/core/howto/defer.html "Twisted Deferred Documentation") is quite detailed.
## The Realm

The next thing we need to do is create a "realm", which will essentially convert the Django user object into an "avatar" - which is the object that the server will use to communicate with the client:
<pre lang="python">class MyRealm(object):
    implements(portal.IRealm)

    def requestAvatar(self, user, mind, *interfaces):
        assert pb.IPerspective in interfaces
        avatar = MyAvatar(user)
        avatar.attached(mind)
        return pb.IPerspective, avatar, lambda a=avatar:a.detached(mind)

class MyAvatar(pb.Avatar):
    def __init__(self, user):
        self.user = user

    def attached(self, mind):
        self.remote = mind
        print 'User %s connected' % (self.user,)

    def detached(self, mind):
        self.remote = None
        print 'User %s disconnected' % (self.user,)</pre>
As you can probably tell, the realm has no responsibility for authentication. It simply accepts a user object (usually a username in normal Twisted apps), and returns an avatar. Here, I've written a very simple avatar that will print a message when each user connects and disconnects. The "mind" object allows the server to call remote methods on the client, and is documented (though not in great detail) in the Twisted [Perspective Broker Authentication documentation](http://twistedmatrix.com/documents/10.1.0/core/howto/pb-cred.html "PerspectiveBroker Authentication").
## Linking Things Together

That's pretty much it for the authentication code. All that's left now is to actually link this into a Perspective Broker:
<pre lang="python">checker = DjangoAuthChecker()
realm = MyRealm()
p = portal.Portal(realm, [checker])

reactor.listenTCP(self.port, PBServerFactory(p))
reactor.run()</pre>
## Client Login

To login to a Perspective Broker with authentication enabled is, you'll be glad to know, very simple:
<pre lang="python">factory = pb.PBClientFactory()
reactor.connectTCP('127.0.0.1', 8123, factory)
d = factory.login(
    cred.credentials.UsernamePassword('username', 'password'),
    client=self
)
d.addCallback(connected)
d.addErrback(error)
reactor.run()</pre>
Hopefully it's obvious what is going on here. Once we "login" to the factory providing the connection, a pair of callbacks are added to the deferred - the ``connected`` function will be called if we are successfully logged in, and the ``error`` function will be called if there is some kind of error (like an ``UnauthorizedLogin``, for example). The success callback will be given the perspective as the only argument, so it can begin to call methods on the server in the usual way.

If you've solved this particular puzzle in some other way, or if it's just been of use - then I'd love to hear from you. In any case, this is an excellent example of how simple integration can be when you're using well documented open source systems. Cheers!
