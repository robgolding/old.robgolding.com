---
layout: post
title: "Django Class-Based View Mixins: Part 2"
date: 2012-11-17 20:39
comments: true
tags:
- django
- programming
---

It's been a long time since my last post (for a number of reasons) but I'll
pick up where I [left off][part1] in the series about class-based view mixins
in Django.

Part 2: Sorting & Filtering
---------------------------

In this post, I'd like to share some mixins that I've found useful for adding
functionality to the generic ``ListView`` -- sorting and filtering.

To implement sorting in a Django view, you'd normally have to do something like
this:

{% codeblock views.py %}
class WidgetListView(ListView):

    def get_queryset(self):
        sort_by = self.request.GET.get('sort_by')
        order = self.request.GET.get('order')
        sort_field = 'id'
        if sort_by == 'name':
            sort_field = 'widget_name'
        qs = super(WidgetListView, self).get_queryset().order_by(sort_field)
        if order == 'desc':
            qs = qs.reverse()
        return qs

    ...
{% endcodeblock %}

Code like this can get repetitive if same functionality is required in several
views, so let's try and impove on this a bit with a mixin:

{% codeblock views.py %}
class WidgetListView(ListView, SortMixin):
    default_sort_params = ('name', 'asc')

    def sort_queryset(self, qs, sort_by, order):
        if sort_by == 'name':
            qs = qs.order_by('widget_name')
        if order == 'desc':
            qs = qs.reverse()
        return qs

    ...
{% endcodeblock %}

With the ``SortMixin``, the ``sort_by`` and ``ordering`` parameters are
extracted from the request and passed to the ``sort_queryset()`` method, so the
implementation is much cleaner, and the sorting functionality is logcally
separate from the rest of the view.

The way that sorting works, though, is still completely up to you. The
``sort_queryset()`` method has complete control over the result that is passed
to the template.

Filtering comes with similar problems. Without a mixin, the view might look
something like this:

{% codeblock views.py %}
class WidgetListView(ListView):
    def get_queryset(self):
        qs = super(WidgetListView, self).get_queryset()
        filter_param = self.request.GET.get('filter')
        if filter_param == 'draft':
            qs = qs.filter(published=None)
        return qs

    ...
{% endcodeblock %}

Again, we can abstract this repetitive logic into a mixin:

{% codeblock views.py %}
class WidgetListView(ListView, FilterMixin):
    default_filter_param = 'all'

    def filter_queryset(self, qs, filter_param):
        if filter_param == 'draft':
            qs = qs.filter(published=None)
        return qs

    ...
{% endcodeblock %}

Similarly to the ``SortMixin``, the ``filter_param`` is extracted from the
request and passed to the ``filter_queryset()`` method, which is responsible
for filtering the queryset that will be rendered in the template.

The real value of these mixins, though, is that they can easily be combined to
provide a ``ListView`` which has both sorting and filtering, whilst maintaining
separate logic for each in their own methods. This makes the code easier to
read, extend, and perhaps most importantly, easier to test.

Here's a full example which combines both of these mixins into one view:

{% codeblock views.py %}
class WidgetListView(ListView, SortMixin, FilterMixin):
    default_sort_params = ('name', 'asc')
    default_filter_param = 'all'

    def sort_queryset(self, qs, sort_by, order):
        if sort_by == 'name':
            qs = qs.order_by('widget_name')
        if order == 'desc':
            qs = qs.reverse()
        return qs

    def filter_queryset(self, qs, filter_param):
        if filter_param == 'draft':
            qs = qs.filter(published=None)
        return qs
{% endcodeblock %}

Both of these mixins pass the relevent parameters to the template through
``get_context_data()``, so you can display the currently active sort field or
filter mode with no hassle.

The code for these mixins is currently available as a [GitHub Gist][gist]. In
the near future, I'll be rolling all the mixins in this series into
a Django app that can be installed and reused.

[part1]: /blog/2012/07/12/django-class-based-view-mixins-part-1/
[gist]: https://gist.github.com/4097500
