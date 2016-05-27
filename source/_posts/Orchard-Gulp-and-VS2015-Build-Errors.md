---
title: 'Orchard, Gulp, and VS2015 Build Errors'
tags:
  - Orchard
  - Gulp
  - VS2015
date: 2015-09-28 20:00:00
---

# Orchard, Gulp, and VS2015 Build Errors

Recently, my team and I, have started using Visual Studio 2015 while working
on our Orchard project. Orchard has started using Gulp to manage building some
of the javascript used in the Layouts and Dynamic Forms modules. Since VS 2015
detects and automatically watches Gulp files, if you have a parameter set at the
top of the Gulp file, node modules start automatically downloading and all JS
and CSS start getting processed. This is great, until you recieve the following
error when publishing.

**The “CollectFilesinFolder” task failed unexpectedly. System.IO.PathTooLongException**

This error is thrown during the build due to the node_modules having crazy
file path lengths. The easiest way to circumvent this issue is to configure the
Layouts and Dynamic Forms modules project file to exclude the node_modules
folder.  This is quick and easy by adding the ExlcudeFoldersFromDeployment tag
in the first property group of each project file.

<script src="https://gist.github.com/phillipsj/ec49732c325b7de55d19.js">&amp;amp;amp;nbsp;</script>

Once that has been added to those project files, publishing now works. Also with
this change my GIT deployments to an Azure Website magically started working
too. There seems to be a fix in place according to this Github [issue](https://github.com/OrchardCMS/Orchard/issues/5649), I am curious
if it is the same solution or not.