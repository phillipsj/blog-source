---
title: Orchard and Azure AD Woes!
tags:
  - Orchard
  - Azure
  - Owin
  - Open Source
date: 2015-10-07 20:00:00
---

# Orchard and Azure AD Woes!

So my team and I have been using Orchard CMS to build the next version of our
product. Since we have been using Azure AD for authentication for our applicaions
it only made sense to wire Orchard and Azure AD together. The typical way that
it is suggested to be done is to Microsoft.Owin.Security.OpenIdConnect middleware.

So we wired this up for Orchard, which was not a straight forward feat given the
sparse documentation around OWIN integration. This all worked great locally and
on our dev server. We decided to finally push a versiont to Azure to work through
our [Hudson Bay Start](http://www.stickyminds.com/article/hudsons-bay-start), it would be awful to be close to a deadline and find
any suprises.

Well, I was hopeful as I have deployed stock Orchard to Azure several times getting
a feel for how it is going to work. The calamity struck, we were ramping up to
have some folks from another part of our business assist with creating content,
however, I enabled our custom modules and click the signin link and BAM! I
was greated with the following error:

**The data protection operation was unsuccessful. This may have been caused by not
having the user profile loaded for the current thread’s user context, which may
be the case when the thread is impersonating.**

<script src="https://gist.github.com/phillipsj/ffa9fbdafad35f46aaf8.js">&amp;amp;amp;nbsp;</script>

I quickly started googling and found several suggestions. I tried several different
approaches, until I pulled out [dotPeek](https://www.jetbrains.com/decompiler/) and dug a little through
_Microsoft.Owin.Security_ and noticed that I needed a different IDataProtectionProvider
that doesn’t use DPAPI. I finally got to a [this](http://stackoverflow.com/questions/23455579/generating-reset-password-token-does-not-work-in-azure-website) on StackOverflow.  The suggestion
there was to implement the following:

<script src="https://gist.github.com/phillipsj/aa374e21ab5bc9b0ea54.js">&amp;amp;amp;nbsp;</script>

Now that I had this implementation, I tried several of the other posts on StackOverflow
trying to use Autofac to inject the dependency in, however it was working.

Again, I turned to dotPeek and was taking a look at what was going on with this
method:

<div class="highlight-python"><div class="highlight"><pre><span class="n">app</span><span class="o">.</span><span class="n">GetDataProtectionProvider</span><span class="p">()</span>
</pre></div>
</div>

While looking at the method, I noticed this other extension method.:

<div class="highlight-python"><div class="highlight"><pre>app.SetDataProtectionProvider(IDataProtectionProvider provider)
</pre></div>
</div>

So I added the following to my OwinMiddleware class in my Orchard Module
and **IT WORKED!**

<script src="https://gist.github.com/phillipsj/b4b6ba3367b48bb37cc6.js">&amp;amp;amp;nbsp;</script>

This has been a two day issue that we have beent trying to determine what the cause.
I will be able to sleep soundly tonight.

Hope someone else finds this helpful.