---
layout: post
title:  "TIL#3: IIS logs have GMT timestamps"
date:   2015-08-28 19:45:08
categories: TIL
---

I have always wondered why IIS logs (for ex: those in C:\windows\system32\LogFiles\HTTPERR) have timestamps that are different from the server’s system time.

It turns out that IIS logs follow the ‘W3C Extended Log File Format’ which specifies that dates and times should always be in GMT. More info can be found [here](https://support.microsoft.com/en-us/kb/271196).