---
layout: post
title: "Django Class-Based View Mixins: Part 1"
date: 2012-06-24 21:01
comments: true
categories: 
---

Django class-based views have been getting quite a bit of
[attention][cbv-mistake] [recently][cbv-not-mistake].
I'm not going to add my voice to the debate, but one thing that can be said is
that the documentation for class-based (generic) views is not on a par with
what we've come to expect from the Django project. This was highlighted by
a [Stack Overflow question][so-q] I answered recently.

I enjoyed writing the previous series on [Django in Production][dip], and
fitting this all into one post would be somewhat of a marathon, so this marks
the start of another.

For the next couple of posts, I'm going to share some of the
[mixins][wiki-mixins] and other labour-saving tricks I've come across whilst
using class-based views in my day-to-day work. Who knows, it might serve as
a contribution to the community as we start to figure out the best way of using
these new fangled things, but at the very least I hope it will be useful to
people like *Vlad*.

Part 1: Authentication & Authorization
--------------------------------------

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

(if, like me, you dislike including this kind of logic
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
is to use mixins. In fact, [django-braces][django-braces] provides a number of
them for this very purpose, including ``LoginRequiredMixin`` and
``PermissionRequiredMixin``. A mixin which restricts a view to only logged in
users might look something like this:

{% gist 3092600 login_required.py %}

This might seem like a really simple class (in truth, it is), but it hinges on
the way ``super`` works. You may have noticed that, because this class inherits
directly from ``object``, it's parent class will have no ``dispatch()`` method
to call.

It's out of the scope of this post to discuss the intricacies, but I would
strongly recommend reading [this article][super-harmful] to understand them.
The important thing to remember when using a mixin like this - which overrides
the implementation of a method defined in a sibling class - is to put it
*first* in the list of base classes:

{% codeblock views.py %}
class SecretView(LoginRequiredMixin, View):
    def get(self, request, *args, **kwargs):
        return HttpResponse('The answer is 42')
{% endcodeblock %}

I've used a few mixins similar to this, for restricting views based on
``is_staff``, ``is_superuser``, and best of all, the user's permissions:

{% gist 3092600 permissions_required.py %}

This mixin, based on the ``PermissionRequiredMixin`` in
[django-braces][django-braces], allows a set of permissions to be required for
a particular view. Importantly, the ``dispatch()`` method checks these
permissions *before* the view is executed.  If it checked after,
a data-altering operation could be executed even though the user who requested
it was not authorized to perform that operation (although they wouldn't get to
see the result).

Using this mixin to control access is easy:

{% codeblock views.py %}
class PrivateView(PermissionsRequiredMixin, View):
    required_permissions = (
        'app.view_private',
    )

    def get(self, request, *args, **kwargs):
        return HttpResponse('Permission granted!')
{% endcodeblock %}

To see all these examples and more, [check out the gist][gist].

In the next post of this series, I'll be sharing some mixins and template tags
which make *filtering and sorting* a breeze.

[cbv-mistake]: http://lukeplant.me.uk/blog/posts/djangos-cbvs-were-a-mistake/
[cbv-not-mistake]: http://www.boredomandlaziness.org/2012/05/djangos-cbvs-are-not-mistake-but.html
[so-q]: http://stackoverflow.com/questions/11171813/how-to-use-named-group-with-generic-view
[dip]: /blog/2011/11/12/django-in-production-part-1---the-stack/
[super-harmful]: https://fuhm.net/super-harmful/
[django-braces]: https://github.com/brack3t/django-braces
[wiki-mixins]: http://en.wikipedia.org/wiki/Mixin
[generic-views-docs]: https://docs.djangoproject.com/en/1.4/topics/class-based-views/
[decorate-urlconf]: https://docs.djangoproject.com/en/1.4/topics/class-based-views/#decorating-in-urlconf
[mro-docs]: http://www.python.org/getit/releases/2.3/mro/
[gist]: https://gist.github.com/3092600
