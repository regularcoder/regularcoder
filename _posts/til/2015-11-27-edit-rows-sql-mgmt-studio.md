---
layout: post
title:  "TIL#10: Edit rows in SQL Server Management Studio"
date:   2015-11-27 15:00:00
categories: TIL SQLServer
---

I was familiar with the feature in SQL Server Management Studio that let me easily modify a table without writing UPDATE queries:
![Right click on table]({{ site.url }}/img/sqleditrows/edit200.png)

What I recently learned is that you don't have to be restricted to the top 200 rows. You can change the SELECT query running in the background to filter whatever rows you need to update by changing the SQL.

<!--more-->

This is how:

After you choose 'Edit Top … Rows' right click on the editor and then Pane -> SQL:
![Pane -> SQL]({{ site.url }}/img/sqleditrows/panesql.png)

You will now see some SQL that you can change like I did below (after you change the SQL you have to go the results tab and then right click -> Execute SQL again):
![Modified output]({{ site.url }}/img/sqleditrows/modifiedSQL.png)

## Interesting notes

* 	SQL Server won't let you modify cells that don't map directly back to a column, for ex a column generated through a function like SUBSTRING() will be uneditable.

* 	You won't be able to edit the output of JOIN queries