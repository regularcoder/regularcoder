---
layout: post
title:  "Error 4064: Cannot open user default database"
date:   2015-09-25 16:00:00
categories: error-solutions
---

A user complained of this error when attempting to connect to SQL Server from SQL Server Management Studio:
![Connect error]({{ site.url }}/img/error_4064/connect_error.png)

His security access was set up correctly via LDAP groups so he should have been able to connect. Looking at the error it was apparent that the default database did not exist. This happened because the database was deleted and the user’s login still pointed to the non-existent database.

## Resolution
Hit options:
![Login screen]({{ site.url }}/img/error_4064/login_screen.png)

Go to Connection Properties tab and put a database name that exists:
![Connection properties]({{ site.url }}/img/error_4064/connection_properties.png)

And then hit Connect.

Now the user was able to log in without any problems. To permanently change the default database so that the user doesn’t have to specify the database the next time:
{% highlight sql %}
exec sp_defaultdb @loginame=’<login>’, @defdb=’<database that exists>’
{% endhighlight %}

## Miscellaneous notes
*	Default database can be determined by looking at the output of this query (default\_database\_name column):

	Depending on your permissions you might be able to only see information for the groups or roles you belong to.
	
	{% highlight sql %}
	select * from sys.server_principals
	{% endhighlight %}
	
* 	SQL Server won’t let you use sp_defaultdb to set a default database which doesn’t exist. It will throw an error:

> Msg 15010, Level 16, State 1, Line 1
> The database 'nonexistent\_db\_name' does not exist. Supply a valid database name. To see available databases, use sys.databases. 