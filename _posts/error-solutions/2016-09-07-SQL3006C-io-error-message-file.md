---
layout: post
title:  "SQL3006C  An I/O error occurred while opening the message file"
date:   2016-09-07 21:00:00
categories: error-solutions db2
---

While running an EXPORT command in DB2 for the first time on my Windows machine today I got this error:

> SQL3006C  An I/O error occurred while opening the message file.

I couldn't find a solution online so I decided to use [Process Monitor](https://technet.microsoft.com/en-us/sysinternals/processmonitor.aspx) from Sysinternals to see exactly what was causing the error. Process Monitor is a Windows event monitoring tool that returns extremely detailed data about file reads, registry access, thread activity, etc. The level of detail is astonishing - thousands of events per second happen in the background even if your computer is "idle".

## Analysis
Since the volume of logging data is so much the first step I took was to create a filter to exclude anything that wasn't related to *db2.exe*:
![ProcMon filter]({{ site.url }}/img/SQL3006C/filter.png)

I then re-ran the EXPORT command and saw a *NAME NOT FOUND* error when trying to access the *tmp* folder:
![NAME NOT FOUND]({{ site.url }}/img/SQL3006C/namenotfound.png)

So that is the cause of the error (for me atleast).

## Resolution
The error went away I created a *tmp* folder in *C:\ProgramData\IBM\DB2\DB2COPY_1\DB2\*.