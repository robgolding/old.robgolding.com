--- 
layout: post
title: Django Model Templates (sans-Denormalisation)
tags: 
- django
- programming
- myuni
- python
---
I came across an interesting problem recently, while trying to model the structure of a university course in Django.

The model needed to represent the notion of a university module, which can be taught over a number of semesters and/or years, by different people, and with different students each time round. Some information remained common to each of these modules however, such as the code, name, and type of the module (single semester or full-year).

<!--more-->

One solution that crossed my mind was to use a sort of template, and copy the common information over each time the module was taught. This would mean duplicating the common fields over a template model and the module's model, and copying the data every time a new "Module" object gets created. This just didn't sit well with me though, as I really don't like denomalising data when it's not absolutely necessary.

The solution I have settled on (for now, anyway) works something like this: I use the model template idea, but with a foreign key from the Module to it's template. Then, to access the fields on the template directly from the Module model, I use a combination of the Python _property_ and a smart lambda function.

Here's a simplified version of the model to illustrate what I'm talking about:
<pre lang="python">class ModuleDefinition(models.Model):
    code = models.CharField(max_length=6, unique=True)
    name = models.CharField(max_length=200)
    credits = models.IntegerField(max_length=4)
    level = models.IntegerField(max_length=2)
    type = models.CharField(max_length=30, choices=MODULE_TYPE_CHOICES)

class Module(models.Model):
    code = property(lambda s: s._get_definition_property('code'))
    name = property(lambda s: s._get_definition_property('name'))
    credits = property(lambda s: s._get_definition_property('credits'))
    level = property(lambda s: s._get_definition_property('level'))

    definition = models.ForeignKey('ModuleDefinition')
    semester = models.CharField(max_length=20, choices=SEMESTER_CHOICES)
    year = models.CharField(max_length=20, choices=YEAR_CHOICES)
    convener = models.ForeignKey('auth.User', related_name='modules_convened')
    students = models.ManyToManyField('auth.User', blank=True, related_name='modules_taken')

    def _get_definition_property(self, property):
        return self.definition.__getattribute__(property)</pre>
In this model I've called the template a "definition", but the idea is the same. The __get_definition_property()_ method allows me to get a property from the parent definition programmatically, which is used in the lambda functions to add the fields from the definition to the model itself, as read-only properties. This allows me to access them as if they were a field stored on the model itself (i.e. _module.code_, _module.name_, etc.) which is handy in the templates where _module.definition.code_ would get very old, very fast.

I'd be really interested to hear how others have tackled the same issue, as I'm sure it's not just restricted to university modules. Maybe soon I'll be making a post about how I model a timetable in Django...after all, how hard can it be!?

Oh, and if you're wondering, this code is for the [MyUni](http://bitbucket.org/robgolding63/myuni/ "MyUni") project that is occupying me recently. I'm restricting myself to just **thinking** about how the models could work (and maybe writing little bit of code) until [Rob](http://robertskmiles.com "Robert S. K. Miles") and [Ben](http://benjenkinson.com "Ben Jenkinson") come back to university.
