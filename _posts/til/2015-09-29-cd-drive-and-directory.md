---
layout: post
title:  "TIL#7: Change drive and directory at same time"
date:   2015-09-29 15:00:00
categories: TIL DOS
---

DOS's CD (Change Directory) command will not usually switch drives. For example this is what happens if you do a CD across drives (C to D, in this case):

![Example of CD not changing drives]({{ site.url }}/img/cd_d/cd_c_d.png)

As you can see, the command prompt remains in C:\ and it will remain there until you type "D:".

If you would like to switch drives in addition to switching directories you need to use the /D switch as follows:

> cd /D "D:\Program Files (x86)"