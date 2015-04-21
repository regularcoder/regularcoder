---
layout: post
title:  "pushd and popd"
date:   2015-04-02 21:59:08
categories: DOS
---
While going through a colleague's C# code yesterday I came across something interesting - pushd and popd.

{% highlight c# linenos %}
//map the UNC path
sw.WriteLine("pushd " + copyTo);
//create another internal folder
sw.WriteLine("cd..");
sw.WriteLine("mkdir Internal_" + DateTime.Now.ToString("MMM"));
//delete the mapped drive                         
sw.WriteLine("popd");
{% endhighlight %}

The code above is writing commands StandardInput property of a "cmd.exe" Process instance. It is immediately apparent that pushd allows us to run commands on a shared directory as if it were a local directory. 

This can be very useful but I wanted to dig a bit further into what else these commands can do.

## Basics
As the the names imply, pushd and popd create a stack of directories that you can push onto and then pop off. Every time you pushd a directory it puts it on top of the stack and switches to it. You can keep pushing (and automatically switching) directories and then pop (and automatically switch) them one by one.

An example will make it clear:

		C:\>pushd C:\Nikhil\angularjs
		C:\Nikhil\angularjs>pushd C:\Nikhil\actautosql
		C:\Nikhil\actautosql>pushd C:\Nikhil\BCP
		C:\Nikhil\BCP>popd
		C:\Nikhil\actautosql>popd
		C:\Nikhil\angularjs>popd
		C:\>

## pushd with UNC paths
UNC (Universal Naming Convention) paths can be temporarily mapped as shared drives. As I mentioned earlier this allows us to run DOS commands as if we were inside the path. You can type 'net use' in the DOS command prompt to see the temporary shared drive.

Whenever you perform a pop the shared drive is destroyed.

I played around with the pushd and popd commands are here are some things I found out:

1. pushd doesn't like root paths like \\INFILE07 or \\UKSERVER08. It will complain that 'The network path was not found'

2. On the other hand, it *really* likes second level directories. What this means is that:
		
		pushd \\INFILE07\2014_LOGS\IIS\ will map to something like Z:\IIS\
		pushd \\UKSERVER08\APPDOCS\ will map to something like Y:\
		pushd \\UKSERVER06\INFASERVER\TTYL\WLOGS\2015 will map to something like X:\TTYL\WORKFLOW_LOGS\2015

3. It creates drives in reverse alphabetical order (skipping letters that are already taken). So you will get drives like Z:, Y:, X:, etc.