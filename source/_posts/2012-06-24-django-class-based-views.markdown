---
layout: post
title: "Django Class-Based View Mixins"
date: 2012-06-24 21:01
comments: true
categories: 
---

Django class-based views have been getting quite a bit of
[attention][cbv-mistake] [recently][cbv-not-mistake].
I'm not going to add my voice to the debate, but one thing that can be said is
that the documentation for class-based (generic) views is not on a par with
what we've come to expect from the Django project. This was highlighted by
a [Stack Overflow question][so-q] I answered yesterday.

In this post, I'd like to share some of the [mixins][wiki-mixins] I tend to
write often, to serve somewhat as a reference point, and in the hope that it
will be useful to people like *Vlad*. All of the code below serves to extend
the functionality of Django's [class-based generic views][generic-views-docs].

Authentication & Permissions
----------------------------

One of the issues I came up against almost immediately was restricting views to
authenticated users. With functional views, the code looks something like this:

{% codeblock auth_view.py %}
@login_required
def secret_view(request, *args, **kwargs):
    return HttpResponse('The answer is 42')
{% endcodeblock %}

However, with class-based views, you're forced into writing the following:

{% codeblock auth_cbv.py %}
class SecretView(View):

    @method_decorator(login_required)
    def dispatch(self, request, *args, **kwargs):
        return super(SecretView, self).dispatch(request, *args, **kwargs)

    def get(self, request, *args, **kwargs):
        return HttpResponse('The answer is 42')
{% endcodeblock %}

(if, like me, you disklike including this kind of logic
[in the URLconf][decorate-urlconf])

Once you have more than a couple of views (one, strictly) that require this
behaviour, your ``views.py`` starts to look very *wet* indeed!

Before settling on the idea of mixins, my first attempt involved the use of
a "parent" view for the project, inheriting from
``django.views.generic.base.View``, which would contain the decorated version
of ``dispatch()`` and anything else that might be required.

I quickly learned that this technique wasn't ideal, though, when I started
getting an unfamiliar exception: ``TypeError: MRO conflict among bases``.
I still feel unqualified to explain exactly how and why this error occurs, but
at the very least it has forced me to (somewhat) understand Python's
[Method Resolution Order][mro-docs].

In any case, a neater and more Pythonic (Djangonic?) solution to this problem
is to use mixins. In fact, ``django-braces`` provides a number of them for this
very purpose, including ``LoginRequiredMixin`` and ``PermissionRequiredMixin``.
A mixin which restricts a view to only logged in users might look something
like this:

{% codeblock mixins.py %}
class LoginRequiredMixin(object):
    @method_decorator(login_required)
    def dispatch(self, request, *args, **kwargs):
        return super(LoginRequiredMixin, self).dispatch(
            self, request, *args, **kwargs)
{% endcodeblock %}

This might seem like a really simple class (in truth, it is), but it hinges on
the way ``super`` works. You may have noticed that, because this class inherits
directly from ``object``, it's parent class will have no ``dispatch()`` method
to call.

It's out of the scope of this post to discuss the intricacies, but I would
strongly recommend reading [this article][super-harmful] to understand them.
The important thing to remember when using a mixin like this - which overrides
the implementation of a method defined in a sibling class
- is to put it *first* in the list of base classes:

{% codeblock views.py %}
class SecretView(LoginRequiredMixin, View):
    def get(self, request, *args, **kwargs):
        return HttpResponse('The answer is 42')
{% endcodeblock %}

I often a few mixins similar to this, for restricting views based on
``is_staff``, ``is_superuser``, and best of all, the user's permissions:

{% codeblock mixins.py %}
    class PermissionsRequiredMixin(object):
        """
        View mixin which verifies that the logged in user has the specified
        permission.

        Settings:

        `required_permissions` - list/tuple of required permissions

        Example Usage:

            class SomeView(PermissionsRequiredMixin, ListView):
                ...
                # required
                required_permissions = (
                    'app1.permission_a',
                    'app2.permission_b',
                )
                ...
        """
        required_permissions = ()

        @method_decorator(login_required)
        def dispatch(self, request, *args, **kwargs):
            self.request = request
            self._assign_screening_attrs(request)

            if not self.user.has_perms(self.required_permissions):
                messages.error(
                    self.request,
                    'You do not have the permission required to perform the '
                    'requested operation.')
                return redirect(settings.LOGIN_URL)

            return super(PermissionsRequiredMixin, self).dispatch(
                request, *args, **kwargs)
{% endcodeblock %}

This mixin, based on the ``PermissionRequiredMixin`` in *django-braces*, allows
a series of permissions to be required for a particular view. Importantly, the
``dispatch()`` method checks these permissions *before* the view is executed.
If it checked after, a data-altering operation could be executed even though
the user who requested it was not authorized to perform that operation
(although they wouldn't get to see the result).

Sorting & Filtering
-------------------


[cbv-mistake]: http://lukeplant.me.uk/blog/posts/djangos-cbvs-were-a-mistake/
[cbv-not-mistake]: http://www.boredomandlaziness.org/2012/05/djangos-cbvs-are-not-mistake-but.html
[so-q]: http://stackoverflow.com/questions/11171813/how-to-use-named-group-with-generic-view
[super-harmful]: https://fuhm.net/super-harmful/
[wiki-mixins]: http://en.wikipedia.org/wiki/Mixin
[generic-views-docs]: https://docs.djangoproject.com/en/1.4/topics/class-based-views/
[decorate-urlconf]: https://docs.djangoproject.com/en/1.4/topics/class-based-views/#decorating-in-urlconf
[mro-docs]: http://www.python.org/getit/releases/2.3/mro/
