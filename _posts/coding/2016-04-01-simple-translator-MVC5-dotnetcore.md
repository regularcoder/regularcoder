---
layout: post
title:  "Creating a translation app with ASP.NET 5"
date:   2016-04-01 10:00:00
categories: ASP.NET
comments: true
---

This tutorial will explain how to create an ASP.NET 5 website running on .NET Core. The translation will be performed using Microsoft's Translator Azure web service. At no point do I use Visual Studio - just command line tools and free editors. 

Here is a preview of what the final product looks like:

![Subfolders]({{ site.url }}/img/simpletranslator/hi_to_en.png)

As much as I love Visual Studio I feel drawn towards lightweight alternatives since installing VS comes with a lot of extraneous stuff.

##Install .NET Core from command line

###Install DNVM
DNVM is the .NET Version Manager - a command line tool that allows you to manage different .NET runtimes. Following the instructions [here](http://docs.asp.net/en/latest/getting-started/installing-on-windows.html#install-asp-net-5-from-the-command-line) on Microsoft's ASP.net website run this command to install DNVM:

        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "&{$Branch='dev';iex ((new-object net.webclient).DownloadString('https://raw.githubusercontent.com/aspnet/Home/dev/dnvminstall.ps1'))}
        
The command above will create a folder in your user profile (usually *C:\\Users\\[userid]\\.dnx\\bin*) which has the *dnvm* command. It is basically a big PowerShell script.

You should now be able to type *dnvm help* in your command prompt and see this:
![Subfolders]({{ site.url }}/img/simpletranslator/dnvm_help.png)

###Install .NET Core
.NET Core is a lean, mean, stripped-down version of the full .NET Framework. It is aimed primarily towards running ASP.NET 5 and a lot of functionality from the Base Class Libraries has been left out. If additional functionality is needed it has to be pulled in via packages.

[This](https://docs.asp.net/en/latest/getting-started/choosing-the-right-dotnet.html) page has an excellent description of what .NET Core is:

> .NET Core 5 is a modular runtime and library implementation that includes a subset of the .NET Framework. .NET Core is supported on Windows, Mac and Linux. .NET Core consists of a set of libraries, called “CoreFX”, and a small, optimized runtime, called “CoreCLR”. .NET Core is open-source.

> The API factoring in .NET Core was updated to enable better componentization. This means that existing libraries built for the .NET Framework generally need to be recompiled to run on .NET Core. 

Sadly, what this also means is that your code that targets the "full" versions of .NET Framework might not work in .NET Core because the libraries have been moved around or even removed entirely.

We can now use DNVM to install an execution environment (DNX) for .NET Core. The command to run is:

        dnvm upgrade -r coreclr
        
Here -r stands for the runtime flavor we are installing. When the command finishes running it will download DLLs for the .NET Core framework and also add two important executables to the current path:

DNU (.NET Development Utility) - You will use this primarily to automatically download dependencies that your project has.

DNX (.NET Execution environment) - You will use this primarily to build and run your project.

Try typing *dnx* and *dnu*, they should both return output explaining their usage.

##Create an ASP.NET Core project

###Install Yeoman for scaffolding
Since we do not have Visual Studio to create project files and directory structure for us we need to use a 'scaffolding' tool called [Yeoman](http://yeoman.io/) to kickstart our new project.

Yeoman has to be installed via [Node.js](https://nodejs.org/) and [npm](https://www.npmjs.com/). If you don't have them already installed visit the [nodejs.org](https://nodejs.org/) website and install Node.js for Windows. The process is pretty straightforward and it will come with *npm* pre-installed.

To verify installation, launch a command prompt and type *npm*. Install Yeoman and its dependencies by running:
        
        npm install -g yo grunt-cli generator-aspnet bower
        
This will run for a while since there are a **lot** of dependencies. If all goes well you will see something like this showing the entire tree of package dependencies:
![Subfolders]({{ site.url }}/img/simpletranslator/yo_install_success.png)

### Set up an ASP.NET website using Yeoman
Yeoman is run by using the *yo* command. The argument you pass into *yo* is a generator. There are numerous [generators](http://yeoman.io/generators/) available for Angular, React, etc. We installed only the ASP.net generator when we ran *npm install* above. So, to create a basic ASP.NET web application run: 

        yo aspnet
        
![Subfolders]({{ site.url }}/img/simpletranslator/yo_basic_aspnet.png)
        
Choose *Web Application Basic* and then answer the remaining questions. At the end you will be left with a basic project structure that looks like this:
![Subfolders]({{ site.url }}/img/simpletranslator/project_structure_start.png)

Let's take a second to marvel at what we see. Microsoft has wholeheartedly embraced open-source technologies like:

1. Using JSON for project file instead of their custom solution files
2. Using Markdown for the readme file
3. [Docker](https://www.docker.com/) "An open platform for distributed applications for developers and sysadmins"
4. [Git](https://git-scm.com/) "Free and open source distributed version control system"
5. [Bower](http://bower.io/) "A package manager for the web"
6. [Gulp](http://gulpjs.com/) "Streaming build system"

Go into the project directory (TranslatorTutorial in my case) and take a look at the project.json file:

![Subfolders]({{ site.url }}/img/simpletranslator/project_json_start.png)

Some things to note here:

* *dependencies* are similar to .NET DLL references. They are fetched via NuGet.
* *Kestrel* is a web server for ASP.NET Core and we launch it via the *web* command.
* *dnx451* and *dnxcore50* are target frameworks. The shortforms are known as TFM (Target Framework Monikers) and a full list can be seen [here](http://docs.nuget.org/create/targetframeworks)

##Run the empty project using Kestrel
Type the command below to download all the dependencies mentioned in the project.json file.

        dnu restore

It will run for a while and use NuGet to download DLLs targetting the framework mentioned in the project.json file. At the end it will show you a summary of where it downloaded the DLLs to and which NuGet feed it used to get them.
        
![Subfolders]({{ site.url }}/img/simpletranslator/dnu_restore_initial.png)

Next, type *dnx web* to run the website using the in-built Kestrel web server.