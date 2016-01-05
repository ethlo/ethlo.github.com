---
layout: post
title: Waiting for transaction propagation
blog: true
---

In the land of traditional ACID DBs, I often encounter code that is trying to 
collaborate with som external system using a queue or notification system. 
The problem is that is is ending up sending the notification to the external 
system before the data has been committed to the central database.
Then when the external system tries to query for data based on the notification, 
the data is not there yet (a different or outside a transaction). 

The remedy I see most often is using a delay using Thread.sleep(). 

Why is this a problem? First of all you have no idea how long you may 
need to sleep, as it is affected by the transaction and data size 
and the amount of load on the database, so you will have to wait a 
long time just to be _almost_ sure. The other problem you have now 
introduced is that _all_ processes are now delayed for at least the 
amount of time the process waits.

This is probably known to many people already (but obviously not everyone) here goes:
{% highlight java %}

private SomeNotificationService delegate;

public void process(final Object event)
{
  TransactionSynchronizationManager.registerSynchronization(new TransactionSynchronizationAdapter()
        {
            @Override
            public void afterCommit()
            {
                delegate.sendEvent(event);
            }
        }); 
}

{% endhighlight %}

That's it! Spring's transaction handling will wait until the transaction is finished until it sends the notification (or not at all if the commit fails). 

Simple, yet very powerful!
