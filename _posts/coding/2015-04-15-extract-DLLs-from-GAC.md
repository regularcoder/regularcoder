---
layout: post
title:  "Extract DLLs from GAC"
date:   2015-04-15 21:59:08
categories: Windows .net
---

You might want to copy a DLL from GAC (Global Assembly Cache) in case that is the only copy available (for ex: no one on my team had the Office 2003 DLLs and it was only on the GAC of a dev webserver). When trying to retrieve a DLL from GAC if you browse to C:\Windows\assembly you can see it but won't be able to copy it to another directory. 

1. Browse to C:\Windows\assembly and take note of the Processor Architecture of the DLL you want to extract: ![Assembly]({{ site.url }}/img/extract_dlls_from_GAC/c_windows_assembly.png)

2. Next navigate to %windir%\assembly\GAC (the %windir% is very important). Then browse until you find the DLL:
