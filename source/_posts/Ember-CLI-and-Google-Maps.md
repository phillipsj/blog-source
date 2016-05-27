---
title: Ember-CLI and Google Maps
tags:
  - EmberJS
  - Ember-CLI
  - Mapping
  - GIS
date: 2014-11-18 19:00:00
---

# Ember-CLI and Google Maps

This evening I started working on a project that I realize Ember is the perfect
fit. The ability to create a custom data adapter to read from the Socrata Data Portal
solves some issues for me and ember data makes that easy. So I did a little search on
Ember Addons and found that there are several addons available.  I picked [ember-google-map](https://github.com/ember-admin/ember-cli-map)
since it seem to have the most features and appears very intuitve to use.  Once I ran the npm command
to install it, I ran ember serve and bam.  I was hit with the following error:

<div class="highlight-none"><div class="highlight"><pre>Refused to apply inline style because it violates the following Content Security Policy
directive: "style-src 'self'". Either the 'unsafe-inline' keyword, a hash ('sha256-...'),
or a nonce ('nonce-...') is required to enable inline execution.
</pre></div>
</div>

After a little google-fu I discovered this github [issue](https://github.com/stefanpenner/ember-cli/issues/2174) which lead me to this ember addon, [ember-cli-content-security-policy](https://github.com/rwjblue/ember-cli-content-security-policy). After adding the
config section below, It resolved all 136 errors I was recieving in Chrome. It appears that this issue is only isolated to running the node express
server for development. I guess I will find out soon enough.

<div class="highlight-javascript"><div class="highlight"><pre><span class="nx">ENV</span><span class="p">.</span><span class="nx">contentSecurityPolicy</span> <span class="o">=</span> <span class="p">{</span>
  <span class="s1">'default-src'</span><span class="o">:</span> <span class="s2">"'none'"</span><span class="p">,</span>
  <span class="c1">// Allow scripts</span>
  <span class="s1">'script-src'</span><span class="o">:</span> <span class="s2">"'self' 'unsafe-eval' http://*.googleapis.com http://maps.gstatic.com"</span><span class="p">,</span>
  <span class="s1">'font-src'</span><span class="o">:</span> <span class="s2">"'self' http://fonts.gstatic.com"</span><span class="p">,</span> <span class="c1">// Allow fonts</span>
  <span class="s1">'connect-src'</span><span class="o">:</span> <span class="s2">"'self' http://maps.gstatic.com"</span><span class="p">,</span> <span class="c1">// Allow data (ajax/websocket)</span>
  <span class="s1">'img-src'</span><span class="o">:</span> <span class="s2">"'self' http://*.googleapis.com http://maps.gstatic.com"</span><span class="p">,</span>
  <span class="c1">// Allow inline styles and loaded CSS</span>
  <span class="s1">'style-src'</span><span class="o">:</span> <span class="s2">"'self' 'unsafe-inline' http://fonts.googleapis.com http://maps.gstatic.com"</span>
<span class="p">};</span>
</pre></div>
</div>

I hope this post is found useful by others. I have a few other ideas I want to try and I am sure this is going to manifest itself again.