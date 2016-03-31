---
layout: post
title:  "Recovering from a failed DB2 load"
date:   2016-03-28 15:00:00
categories: DB2
comments: true
---

If you are loading a DB2 table using Informatica in bulk mode then a failure will result in the table going into check pending state. You will not be able to resume load or even query the table.

Here is how you can recover from the failure.

<!--more-->
WARNING: This will truncate any data that is currently in the table.

1. Create an empty file (for ex: C:\empty.del)
2. Open DB2 command line editor and connect to database using a privileged ID (the one used to run the load for example) and then run: *load from C:\empty.del of del terminate into [SCHEMA].[TABLENAME] NONRECOVERABLE*
![Subfolders]({{ site.url }}/img/db2_recover_load/load_from_empty.png)
3. Open RapidSQL and run this query: *SET INTEGRITY FOR [SCHEMA].[TABLENAME] materialized query, generated column, foreign key, staging, check immediate unchecked;*

The table should now be in queryable state (without any data) and you can retry the load.