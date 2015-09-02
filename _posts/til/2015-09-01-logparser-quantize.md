---
layout: post
title:  "TIL#5 - LogParser's QUANTIZE function"
date:   2015-09-01 21:20:00
categories: TIL
---

LogParser is a fantastic command-line tool from Microsoft that lets you run SQL queries against log files. Download [here](http://www.microsoft.com/en-us/download/details.aspx?id=24659).

LogParser's *QUANTIZE* function can be used to truncate time values by rounding them to the nearest multiple of number of seconds passed in. For example, *QUANTIZE(time, 60)* returns:

![60 seconds]({{ site.url }}/img/logparser_quantize/60.png)
Time has been rounded to the nearest minute (60 seconds). Note that here *time* is a column name in IIS logs.

Another illustrative example with *QUANTIZE(time, 600)* rounds up to every 10 minutes:

![600 seconds]({{ site.url }}/img/logparser_quantize/600.png)
Here we can see that rounding takes place **downward** so 1:37:08 becomes 1:30:00 instead of 1:40:00.

*QUANTIZE(time, 3600)* rounds up to every hour:

![3600 seconds]({{ site.url }}/img/logparser_quantize/3600.png)

This can be very useful if you want to put time into 'buckets'. For ex, to figure out how many 404 errors occur every hour you can run:

{% highlight sql %}
logparser "SELECT quantize(time, 3600) as qnt_time, count(*) as NUM_404 from u_ex*.log where sc-status = 404 group by qnt_time order by qnt_time"
{% endhighlight %}

Which gives you:
![Number of 404 errors per hour]({{ site.url }}/img/logparser_quantize/404perhour.png)