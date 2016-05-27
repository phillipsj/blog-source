---
title: Authorize .NET DPM Helper
tags:
  - ASP .NET MVC
  - Authorize .NET
date: 2015-02-25 19:00:00
---

# Authorize .NET DPM Helper

I recently started diving into learning about payment gateways and payment
processing. This is a new area for me and there a lot to learn. After Authorize
.NET was settled on as our payment gateway, I went about the task of learning
their .NET SDK. There are way to many options and I do not understand the pros
and cons of each method, but after reading their documentation I settled on
using their Direct Post Method (DPM) as it limited our exposure to sensitive
information and the featureset is perfect for what we are currently doing.

However, their example is a little rudimentary and just writes the HTML for the
form using string concatenation and would be hard to maintain and customize. So
I was curious as to what it would take to create an HTML helper for ASP .NET MVC
that would be similiar to Html.BeginForm, so that way I could build by form
using razor while still using their open and end form methods.

[<span class="problematic" id="id2">*</span>](#id1)I decided to try gists compared to the default highlighting in Tinkerer.

Here is their [example](https://developer.authorize.net/integration/fifteenminutes/csharp/) before:

<script src="https://gist.github.com/phillipsj/168f011bc6d6b5e0632f.js">&amp;amp;amp;nbsp;</script>

After lots of googling and a couple of really helpful StackOverflow posts, I was
able to determine that I needed to create a class that implemented IDisposable
and make that class responsible for opening the form and ending the form. After
that all that was needed was to create the helper extension.

<script src="https://gist.github.com/phillipsj/219e37df1cc1efef51ba.js">&amp;amp;amp;nbsp;</script>

Now with the HTML Helper, your form can be created like this:

<script src="https://gist.github.com/phillipsj/541f16ab8cae65e07994.js">&amp;amp;amp;nbsp;</script>

This is much cleaner and easier to create a custom form. This code is up on
Github under the project [authorize-net-helpers](https://github.com/phillipsj/authorize-net-helpers). I am planning to get a package
up on Nuget this weekend, of which I have no clue how to really do.

Thanks,

Jamie