---
title: 'Ember Helpers: Testing'
tags:
  - EmberJS
  - Helpers
  - Handlebars
  - Ember-CLI
date: 2014-11-16 19:00:00
---

# Ember Helpers: Testing

Following up with my last post, I forgot to address one of the key
development practices that I am finding that Ember makes extremely
easy, testing.  So I am going to show you how to test the helpers
that were created in the previous post.

In the previous post we ran the following command:

<div class="highlight-none"><div class="highlight"><pre>$ ember g helper &lt;helper name&gt;
</pre></div>
</div>

This command generates both the helper and its corresponding test. Open
the corresponding test file for the mailToLink helper.  We are going to
write a test that makes sure it generates the correct link.

<div class="highlight-javascript"><div class="highlight"><pre><span class="nx">test</span><span class="p">(</span><span class="s1">'generates correct link'</span><span class="p">,</span> <span class="kd">function</span><span class="p">(){</span>
  <span class="kd">var</span> <span class="nx">result</span> <span class="o">=</span> <span class="nx">mailtoLink</span><span class="p">(</span><span class="s1">'test@test'</span><span class="p">);</span>
  <span class="nx">equal</span><span class="p">(</span><span class="nx">result</span><span class="p">,</span> <span class="s1">'&lt;a href="mailto:test@test"&gt;test@test&lt;/a&gt;'</span><span class="p">);</span>
<span class="p">});</span>
</pre></div>
</div>

See how easy that was, the test for the telLink helper is just as easy:

<div class="highlight-javascript"><div class="highlight"><pre><span class="nx">test</span><span class="p">(</span><span class="s1">'generates correct link'</span><span class="p">,</span> <span class="kd">function</span><span class="p">(){</span>
  <span class="kd">var</span> <span class="nx">result</span> <span class="o">=</span> <span class="nx">telLink</span><span class="p">(</span><span class="s1">'8675309'</span><span class="p">);</span>
  <span class="nx">equal</span><span class="p">(</span><span class="nx">result</span><span class="p">,</span> <span class="s1">'&lt;a href="tel:8675309"&gt;8675309&lt;/a&gt;'</span><span class="p">);</span>
<span class="p">});</span>
</pre></div>
</div>

Really basic tests, but since Ember allows you to easily build your application
in a modular fashion it should keep that code you do write very succinct and simple.