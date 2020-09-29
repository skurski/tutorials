1) First-level cache (persistence context)

First-level cache always Associates with the Session object. Hibernate uses this cache by default. Here, it processes one transaction after another one, means wont process one transaction many times. Mainly it reduces the number of SQL queries it needs to generate within a given transaction. That is instead of updating after every modification done in the transaction, it updates the transaction only at the end of the transaction.

2) Second-level cache

Second-level cache always associates with the Session Factory object. While running the transactions, in between it loads the objects at the Session Factory level, so that those objects will be available to the entire application, not bound to single user. Since the objects are already loaded in the cache, whenever an object is returned by the query, at that time no need to go for a database transaction. In this way the second level cache works. Here we can use query level cache also.


https://javabeat.net/introduction-to-hibernate-caching/