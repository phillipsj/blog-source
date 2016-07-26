---
title: 'Using C# 6 features in Cake'
tags:
  - Open Source
  - Cake
---

I like a lot of features available in C# 6, but my favorite is string interpolation. I think it makes ecstatically pleasing code. It is much cleaner and more expressive.

{% codeblock lang:csharp %}

// String.Format example
var messageWithFormat = string.Format("I'm sorry, {0} I'm afraid I can't do that.", "Dave");

// String Interpolation
var messageWithInterpolation = $"I'm sorry, {"Dave"} I'm afraid I can't do that.";

{% endcodeblock %}

However, by default you cannot use string interpolation in your Cake file as those bits don't come with Roslyn just yet. This is an extremely easy fix. All you need to do is pass the experimental flag.

{% codeblock lang:shell %}
$ .\build.ps1 -experimental
{% endcodeblock %}

This just gets old and repetitive and I don't know about you, but I like staying as close to the leading edge as possible, so I just make a small change to my bootstrapper file to always pass the experimental flag. Roughly on like 188 of your bootstrapper file, just replace the *$UseExperimental* variable with the *-experimental* flag. 

{% codeblock lang:powershell %}
# Previous 
Invoke-Expression "& `"$CAKE_EXE`" `"$Script`" -target=`"$Target`" -configuration=`"$Configuration`" -verbosity=`"$Verbosity`" $UseMono $UseDryRun $UseExperimental $ScriptArgs"

# Altered
Invoke-Expression "& `"$CAKE_EXE`" `"$Script`" -target=`"$Target`" -configuration=`"$Configuration`" -verbosity=`"$Verbosity`" $UseMono $UseDryRun -experimental $ScriptArgs"
{% endcodeblock %}
