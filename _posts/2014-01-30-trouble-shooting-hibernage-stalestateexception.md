---
layout: post
title: ! "Trouble shooting: org.hibernate.StaleStateException: Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1"
categories: program java
---

This exception should be the most famous hibernate exception for user:
{% highlight java %}
org.hibernate.StaleStateException: Batch update returned unexpected row count from update [0]; actual row count: 0; expected: 1
at org.hibernate.jdbc.Expectations$BasicExpectation.checkBatched(Expectations.java:85)
at org.hibernate.jdbc.Expectations$BasicExpectation.verifyOutcome(Expectations.java:70)
at org.hibernate.jdbc.BatchingBatcher.checkRowCounts(BatchingBatcher.java:90)
at org.hibernate.jdbc.BatchingBatcher.doExecuteBatch(BatchingBatcher.java:70)
at org.hibernate.jdbc.AbstractBatcher.executeBatch(AbstractBatcher.java:268)
at org.hibernate.engine.ActionQueue.executeActions(ActionQueue.java:266)
at org.hibernate.engine.ActionQueue.executeActions(ActionQueue.java:168)
at org.hibernate.event.def.AbstractFlushingEventListener.performExecutions(AbstractFlushingEventListener.java:321)
at org.hibernate.event.def.DefaultFlushEventListener.onFlush(DefaultFlushEventListener.java:50)
at org.hibernate.impl.SessionImpl.flush(SessionImpl.java:1027)
at org.hibernate.impl.SessionImpl.managedFlush(SessionImpl.java:365)
at org.hibernate.transaction.JDBCTransaction.commit(JDBCTransaction.java:137)
{% endhighlight %}
When i see this first time, i feel like all wonderful feature of hibernate just left me. But after calm down, thanks to This post , i found most of the reason should be the following:

1. Flushing the data before committing the object may lead to clear all object pending for persist.
2. If object has primary key which is auto generated and you are forcing has assigned key may cause the exception.
3. if you are cleaning the object before committing the object to database may lead this exception.
4. Zero or Incorrect ID: Hibernate excepts an primary or id of null (not initialize while saving) has per point 2 to mean the object was not saved. If you set the ID to zero or something else, Hibernate will try to update instead of insert, or it lead may to throw this exception.
5. Object does not Exist: This is the most easy to determine : has the object get deleted somehow? If so, trying to delete once again it will throw this exception. 
6. Object is Stale: Hibernate caches objects from the session. If the object was modified, and Hibernate doesn’t know about it, it will throw this exception — note the StaleStateException part of the exception. so after committing the object to database need to flush or clear or clean it ,not before.

Finally, i solved my problem. Feels good!