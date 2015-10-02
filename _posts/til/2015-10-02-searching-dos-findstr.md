---
layout: post
title:  "TIL#8: Searching in DOS using findstr"
date:   2015-10-02 15:00:00
categories: TIL, DOS
---

While searching for an alternative to the Windows 7 search box that I could download and use for reliable searching I came across the “findstr” command.

Usage examples follow.

Search all files in current directory for the word RELIABLE

> findstr “RELIABLE” \*.\*

Search all txt files in current directory and subdirectories (that’s what the S switch is for) for the number 9007

> findstr /S “9007″ \*.txt

findstr will not show you the full path of file results. It only shows paths relative to the search directory:

![Partial file path]({{ site.url }}/img/findstr/part_file_path.png)

A way around this is to include %CD% in the latter part of the command, like so:

> findstr “text-align” “%CD%\\\*.\*”

![Full file path]({{ site.url }}/img/findstr/full_file_path.png)

You can read additional details on [this](https://technet.microsoft.com/en-us/library/bb490907.aspx) Microsoft TechNet page.

## Notes

* 	findstr works best on ASCII and text files. The Windows 7 GUI search works much better on other formats like Word or Excel.
*	You can run findstr on network shares by first running pushd *network path*