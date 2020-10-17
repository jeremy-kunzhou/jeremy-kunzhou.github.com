---
layout: post
title: Brilliant Jasper Report
categories: program jasperreport
---

Today i have to make some complex report including multiple group and functions of mysql in query.

I am impressed by the power of jasper report. At beginning, when i found how complex the report is, i was just freak out. The requirement is: 1 search the record based on opening hour not the normal 00:00:00 to 23:59:59 2 show sale record in groups based on opening date and category and statistic the revenue for both group.

Challenge complete!!

1 By after research, i found how wonderful function the mysql provided for developer. So i could just use sql query to process the records i need. using the following in query:
{% highlight java %}
(case when [some condition] then else end) as column_name
{% endhighlight %}
it helps me to get the business date the record belong to. example:
{% highlight java %}
(case when time < $P{startTime} then SUBDATE(date, 1) else date end)
{% endhighlight %}
The following helps me to find the right record based on given period:
{% highlight java %}
(select concat(date,' ',time)) between $P{fromDate} and  $P{toDate}
{% endhighlight %}
2 another challenge is the multiple group statistics of records. Unfortunately, there is some problem in ireport designer. Therefore, i have to use xml to modify the report layout.

The group statistics function is brilliant. The variable could be reset based on report, column, group (if you have multiple group, you could choose one of them) page or even report! That helps me a lot.