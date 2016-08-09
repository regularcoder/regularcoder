---
layout: post
title:  "TIL#11: clip - copy from DOS"
date:   2016-08-09 15:00:00
categories: TIL DOS
---

I came across an interesting MS-DOS command called *clip* that allows you to copy command output onto the Windows clipboard (usually I do this by selecting with my mouse).

The output of *clip /?* has everything you need to know:
    
    CLIP

    Description:
        Redirects output of command line tools to the Windows clipboard.
        This text output can then be pasted into other programs.

    Parameter List:
        /?                  Displays this help message.

    Examples:
        DIR | CLIP          Places a copy of the current directory
                            listing into the Windows clipboard.

        CLIP < README.TXT   Places a copy of the text from readme.txt
                            on to the Windows clipboard.