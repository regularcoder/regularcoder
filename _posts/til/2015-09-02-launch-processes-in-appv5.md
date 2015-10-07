---
layout: post
title:  "TIL#6: Launch processes in App-V 5.0 virtualized environment"
date:   2015-09-02 23:20:00
categories: TIL
---

There are times when you might need to run a process inside the App-V bubble. For ex, to check if an ODBC data source has been set up. [This link](https://support.microsoft.com/en-us/kb/2848278) explains how you can do that in App-V 5.0. I’m reiterating the steps here and including some things I found elsewhere and by myself:

1. 	Click on Start menu -> Accessories -> Windows PowerShell -> Windows PowerShell.
![PowerShell launch]({{ site.url }}/img/appv_process/powershell_launch.png)

2. 	Type these commands:
	
	$AppVName = Get-AppvClientPackage *\*APPNAME\**
	
	Start-AppvVirtualProcess -AppvClientObject $AppVName *C:\Windows\SysWow64\odbcad32.exe*

	Replace *APPNAME* with a keyword from your application’s name.

3. If you face an error like “the execution of scripts is disabled on this sytem” try running *Set-ExecutionPolicy RemoteSigned*.

Bonus tip, you can find useful information about the internals of App-V 5.0 packages in : C:\Users\\*[user id]*\AppData\Roaming\Microsoft\AppV\Client\Catalog\Packages.