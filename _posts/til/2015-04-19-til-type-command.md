---
layout: post
title:  "TIL#1: type, Windows alternative to cat"
date:   2015-04-19 15:00:08
categories: TIL
---
While working on a database job recently I learned that the *type* command can be used to output the contents of a file. This is similar to the *cat* command in UNIX.

	c:\Nikhil>type sample.txt
	Hi this is a text file
	c:\Nikhil>

This can be very useful in a variety of scenarios. For example, the database job I was working on read the contents of a parameter file and used it inside a SQL query via a combination of *type* and *xp_cmdshell*:

{% highlight sql linenos %}
DECLARE @paramFromFile VARCHAR(256)
CREATE TABLE #paramTable (fileoutput VARCHAR(255) null)

INSERT #paramTable EXEC @rc = master..xp_cmdshell 'type D:\jobfolder\parameter.txt'
SELECT @paramFromFile = fileoutput FROM #paramTable WHERE fileoutput is not null
{% endhighlight %}