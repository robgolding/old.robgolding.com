---
layout: post
title: "Django Class-Based Views"
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

In this post, I'd like to share some of the techniques and practices that
I have settled upon after working with class-based views for some time, in the
hope that it will be useful to people like *Vlad*.

Mixins & Inheritance
--------------------

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

Once you have more than a couple of views (one, strictly) that require this
behaviour, your ``views.py`` starts to look very *wet* indeed!

My immediate thought was to use a number of "parent" views for the project,
(inheriting from ``django.views.generic.base.View``, which would contain the
decorated version of ``dispatch()``, and anything else that might be required.
For example, one might require that the user be authenticated, and another
that they are a member of staff (``user.is_staff == True``).

I quickly learned that this technique wasn't ideal, though, when I started getting an unfamiliar exception: ``TypeError: MRO conflict among bases``

The issue I encountered with this technique, though, is that by inheriting from multiple 
view in combination with other generic views (such as ``FormView``, for
example) using generic views in combination with

A neat solution to this slippery problem is to use mixins. ``django-braces``
provides a few useful ones, including ``LoginRequiredMixin`` and
``PermissionRequiredMixin``. A mixin which provides the same functionality as
the above code would look like this:

{% codeblock mixins.py %}
class LoginRequiredMixin(object):
    @method_decorator(login_required)
    def dispatch(self, request, *args, **kwargs):
        return super(LoginRequiredMixin, self).dispatch(
            self, request, *args, **kwargs)
{% endcodeblock %}

This might seem like a really simple class (in truth, it is), but it hinges on
the way ``super`` works. It's out of the scope of this post to discuss the
intricacies, but I would strongly recommend reading
[this article][super-harmful] to understand them. The important thing to
remember when using a mixin like this, which overrides the implementation of a
method from "above", is to put it *first* in the list of base classes:

{% codeblock views.py %}
class SecretView(LoginRequiredMixin, View):
    def get(self, request, *args, **kwargs):
        return HttpResponse('The answer is 42')
{% endcodeblock %}


[cbv-mistake]: http://lukeplant.me.uk/blog/posts/djangos-cbvs-were-a-mistake/
[cbv-not-mistake]: http://www.boredomandlaziness.org/2012/05/djangos-cbvs-are-not-mistake-but.html
[so-q]: http://stackoverflow.com/questions/11171813/how-to-use-named-group-with-generic-view
[super-harmful]: https://fuhm.net/super-harmful/
