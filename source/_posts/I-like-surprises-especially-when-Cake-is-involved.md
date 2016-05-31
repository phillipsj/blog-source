---
title: 'I like surprises, especially when Cake is involved!'
tags:
  - Open Source
  - Cake
date: 2015-11-30 19:00:00
alias: blog/html/2015/11/30/i_like_surprises_especially_when_cake_is_involved.html
---

So I was working today to consume a Cake addin that isn’t fully baked. It is
an addin that we needed. So I cloned the repo and pushed a copy of the package
to a [MyGet.org](https://www.myget.org) to consume as part of our build. I added a custom NuGet.config
file to my Cake tools directory, but Cake would not install the addin. So I just
added it to the packages.config, still no dice.  I had to use the older syntax:

{% gist 556e127e16ed712dbf0b OldSyntax.cs %}

I wasn’t satisfied with this solution, so I jumped on the Cake Gitter, who doesn’t
lover Gitter, and starting asking some questions. I was soon digging into the
source and discovered that as part of parsing the addin directive if you put the
source after the package name it woud use it. So now I could use the #addin syntax
like so:

{% gist 9f6f8cac2a1bd05f0e9a syntax.cs %}

So all you have to due is specify a source that can be consumed by NuGet.exe and
you can use the #addin syntax.

Such a pleasant surprise, Cake doesn’t dissapoint.