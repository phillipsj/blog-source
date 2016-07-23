---
title: 'Cake: Automating an existing project'
tags:
  - Open Source
  - Cake
  - Tutorials
---

Hi, my name is Jamie and I am a [Cake](http://cakebuild.net/) addin author that doesn't build my addins using Cake. This seems kind of wrong too me and this is a bonus for you. I am going to walk you through how to use Cake to build your project, run your tests, create a [NuGet](https://www.nuget.org/) package, then publish to NuGet. I am going to walk you through performing this for my [Cake.XdtTransform](https://github.com/phillipsj/Cake.XdtTransform) project. The project is hosted on GitHub so you can see the final build. I am going to be using the wonderful extension created for [VsCode](https://code.visualstudio.com/) and will be doing most of my work in it, I will discuss the alternatives if you are not using VsCode. If you are going to continue with VsCode, then I would install the Cake Extension that has been created.

** Steps 1-2 can be skipped if you go grab those files from the [example](https://github.com/cake-build/example) project.

## Step 1:

By default all projects that use Cake starts with two files, the bootstrapper script, *build.ps1*, if on windows, or *build.sh*, on linux. The other file that is needed is the cake file, typically called *build.cake*.  In VsCode, open the project that you want to automate with Cake, open the command palette and run the Cake: Install Bootstrapper command, the select the bootstrapper file type that fits your system, in this example I have selected *Powershell*.  You should now see the *build.ps1* file in your project directory. At this point, there is not much more you are going to need to do with the bootstrapper file.

## Step 2:

Now you need to create your *build.cake* file. Once you have created the file we need to start putting in the basic plumbing that will be required. What is going to be shown below is typically the norm for most cake files, however, it isn't the only way it can be done. 

### Setting up arguments

Now you need to grab any arguments you want to pass to your cake file. The first argument we are going to grab that is passed in is the *target* argument. This will be the task that you want Cake to execute when it runs. It is set to *Default* if nothing is passed in. The second argument is the *configuration* that you want to target, it is set to "*Release* if nothing is passed. 

{% codeblock lang:csharp %}

//////////////////////////////////////////////////////////////////////
// ARGUMENTS
//////////////////////////////////////////////////////////////////////

var target = Argument("target", "Default");
var configuration = Argument("configuration", "Release");

{% endcodeblock %}

** A special note about arguments. If you open the bootstrapper file you will see that the arguments are parsed out by powershell then passed to your *build.cake* file. If you need to extend the arguments you will need to modify the bootstrapper file.

### Let's run Cake

At this point, lets open a powershell window and run:

{% codeblock lang:shell-script %}
$ .\build.ps1
{% endcodeblock %}

You should see the following output:

{% codeblock lang:shell-script %}
Preparing to run build script...
Running build script...
Analyzing build script...
Processing build script...
Downloading and installing Roslyn...
Installing packages (using https://packages.nuget.org/api/v2)...
Copying files...
Copying Roslyn.Compilers.CSharp.dll...
Copying Roslyn.Compilers.dll...
Deleting installation directory...
Compiling build script...
{% endcodeblock %}

You will also notice that a tools folder has been added to your project with a Cake folder, nuget.exe, and a packages.config.  This the bootstrapper getting nuget and configuring it, and then install Cake. This is pretty awesome as it doesn't require anything to be committed to your repository.


