---
title: Helpers in EmberJS
tags:
  - EmberJS
  - Helpers
  - Handlebars
  - Ember-CLI
date: 2014-11-04 19:00:00
alias: blog/html/2014/11/04/helpers_in_emberjs.html
---

EmberJS uses Handlebars as its template engine.
To extend it, you create what Ember calls helpers.
There are several ways in Ember to tackle a problem, but
some are more optimal than others.

I needed to create tags that use the **mailto** and **tel" attributes.
So achieve this I created helpers so my templates are a little more
semantic to read and understand what is happening.

The first helper I created was a mailto helper. Using Ember-CLI is a nice way
to get started. Remember that helpers kind of require a multipart name, so make sure
not to create one with a single name as it tends to generate errors in EmberJS. Another tip
is to make sure that you wrap your html tag in the Ember.Handlebars.SafeString method so it
doesn’t escape the html when rendering.

Getting Started:

Lets create the mailto helper to start.

'''
$ ember g helper mailtoLink
'''

Now navigate to the helper file and add this code.

{% gist f27f6ee75de18b9a31af90fe4a700057 mailToLink.js %}

Now create the telLink helper to start.

'''
$ ember g helper telLink
'''

Now navigate to the helper file and add this code.

{% gist f27f6ee75de18b9a31af90fe4a700057 telLink.js %}

Now to use these you can just call these in your templates like so:

{% gist f27f6ee75de18b9a31af90fe4a700057 template.html %}

Ember makes lots of things really easy, but sometimes it not very clear how to
do others. Hope you find this helpful.