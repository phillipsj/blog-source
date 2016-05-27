---
title: Helpers in EmberJS
tags:
  - EmberJS
  - Helpers
  - Handlebars
  - Ember-CLI
date: 2014-11-03 19:00:00
---

# Helpers in EmberJS

EmberJS uses Handlebars as its template engine.
To extend it, you create what Ember calls helpers.
There are several ways in Ember to tackle a problem, but
some are more optimal than others.

I needed to create tags that use the “mailto” and “tel” attributes.
So achieve this I created helpers so my templates are a little more
semantic to read and understand what is happening.

The first helper I created was a mailto helper. Using Ember-CLI is a nice way
to get started. Remember that helpers kind of require a multipart name, so make sure
not to create one with a single name as it tends to generate errors in EmberJS. Another tip
is to make sure that you wrap your html tag in the Ember.Handlebars.SafeString method so it
doesn’t escape the html when rendering.

Getting Started:

Lets create the mailto helper to start:

<div class="highlight-none"><div class="highlight"><pre>$ ember g helper mailtoLink
</pre></div>
</div>

Now navigate to the helper file and add this code.

<div class="highlight-javascript"><div class="highlight"><pre><span class="kr">import</span> <span class="nx">Ember</span> <span class="nx">from</span> <span class="s1">'ember'</span><span class="p">;</span>

<span class="kr">export</span> <span class="kd">function</span> <span class="nx">mailtoLink</span><span class="p">(</span><span class="nx">input</span><span class="p">)</span> <span class="p">{</span>
   <span class="kd">var</span> <span class="nx">mailTo</span> <span class="o">=</span> <span class="s1">'&lt;a href="mailto:'</span> <span class="o">+</span> <span class="nx">input</span> <span class="o">+</span> <span class="s1">'"&gt;'</span><span class="p">;</span>
   <span class="nx">mailTo</span> <span class="o">+=</span> <span class="nx">input</span> <span class="o">+</span> <span class="s1">'&lt;/a&gt;'</span><span class="p">;</span>
   <span class="k">return</span> <span class="k">new</span> <span class="nx">Ember</span><span class="p">.</span><span class="nx">Handlebars</span><span class="p">.</span><span class="nx">SafeString</span><span class="p">(</span><span class="nx">mailTo</span><span class="p">);</span>
<span class="p">}</span>

<span class="kr">export</span> <span class="k">default</span> <span class="nx">Ember</span><span class="p">.</span><span class="nx">Handlebars</span><span class="p">.</span><span class="nx">makeBoundHelper</span><span class="p">(</span><span class="nx">mailtoLink</span><span class="p">);</span>
</pre></div>
</div>

Now create the telLink helper to start:

<div class="highlight-none"><div class="highlight"><pre>$ ember g helper telLink
</pre></div>
</div>

Now navigate to the helper file and add this code.

<div class="highlight-javascript"><div class="highlight"><pre><span class="kr">import</span> <span class="nx">Ember</span> <span class="nx">from</span> <span class="s1">'ember'</span><span class="p">;</span>

<span class="kr">export</span> <span class="kd">function</span> <span class="nx">telLink</span><span class="p">(</span><span class="nx">input</span><span class="p">)</span> <span class="p">{</span>
  <span class="kd">var</span> <span class="nx">tel</span> <span class="o">=</span> <span class="s1">'&lt;a href="tel:'</span> <span class="o">+</span> <span class="nx">input</span> <span class="o">+</span> <span class="s1">'"&gt;'</span><span class="p">;</span>
  <span class="nx">tel</span> <span class="o">+=</span> <span class="nx">input</span> <span class="o">+</span> <span class="s1">'&lt;/a&gt;'</span><span class="p">;</span>
  <span class="k">return</span> <span class="k">new</span> <span class="nx">Ember</span><span class="p">.</span><span class="nx">Handlebars</span><span class="p">.</span><span class="nx">SafeString</span><span class="p">(</span><span class="nx">tel</span><span class="p">);</span>
<span class="p">}</span>

<span class="kr">export</span> <span class="k">default</span> <span class="nx">Ember</span><span class="p">.</span><span class="nx">Handlebars</span><span class="p">.</span><span class="nx">makeBoundHelper</span><span class="p">(</span><span class="nx">telLink</span><span class="p">);</span>
</pre></div>
</div>

Now to use these you can just call these in your templates like so:

<div class="highlight-html"><div class="highlight"><pre><span class="nt">&lt;p&gt;</span>Email: {{mailto-link email}}<span class="nt">&lt;/p&gt;</span>
<span class="nt">&lt;p&gt;</span>Phone: {{tel-link phone}}<span class="nt">&lt;/p&gt;</span>
</pre></div>
</div>

Ember makes lots of things really easy, but sometimes it not very clear how to
do others. Hope you find this helpful.