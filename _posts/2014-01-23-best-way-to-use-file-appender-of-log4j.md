---
layout: post
title: Best way to use file appender of log4j in spring without absolute path
categories: program java
---

Well, i think most of developer faced the same problem as i did this afternoon: the spring cannot find file path based on the log4j.properties file. Unless, you gave log4j a absolute path for file appender.

That is totally bad. Because no one will know your web service will be deployed on which environment.

So this is the answer: Use Log4jConfigListener. It allows you to store your log file in /WEB-INF/logs/ without absolute path by using as following:

log4j.appender.logfile.File=${webapp.root}/WEB-INF/logs/myfuse.log
Juse make sure you put the Log4jConfigListener before the spring listener.

Add those to your web.xml:
{% highlight xml %}
<context-param>
    <param-name>log4jConfigLocation</param-name>
    <param-value>WEB-INF/log4j.properties</param-value>
</context-param>

<context-param>
    <param-name>log4jRefreshInterval</param-name>
    <param-value>60000</param-value>
</context-param>

<listener>
    <listener-class>org.springframework.web.util.Log4jConfigListener</listener-class>
</listener>
{% endhighlight %}