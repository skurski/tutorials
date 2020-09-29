# Sharding (MongoDB example)

https://docs.mongodb.com/manual/sharding/
https://docs.mongodb.com/manual/core/transactions/

Sharding is a method for distributing data across multiple machines. MongoDB uses sharding to 
support deployments with very large data sets and high throughput operations.

Database systems with large data sets or high throughput applications can challenge the capacity of 
a single server. For example, high query rates can exhaust the CPU capacity of the server. 
Working set sizes larger than the system’s RAM stress the I/O capacity of disk drives.

__There are two methods for addressing system growth: vertical and horizontal scaling.__

* Vertical Scaling involves increasing the capacity of a single server, such as using a more powerful 
CPU, adding more RAM, or increasing the amount of storage space. Limitations in available technology 
may restrict a single machine from being sufficiently powerful for a given workload. Additionally, 
Cloud-based providers have hard ceilings based on available hardware configurations. As a result, 
there is a practical maximum for vertical scaling.

* Horizontal Scaling involves dividing the system dataset and load over multiple servers, 
adding additional servers to increase capacity as required. While the overall speed or capacity of a 
single machine may not be high, each machine handles a subset of the overall workload, potentially 
providing better efficiency than a single high-speed high-capacity server. Expanding the capacity of 
the deployment only requires adding additional servers as needed, which can be a lower overall cost 
than high-end hardware for a single machine. The trade off is increased complexity in infrastructure 
and maintenance for the deployment.

## Sharded Cluster 

__A MongoDB sharded cluster consists of the following components:__

* shard: Each shard contains a subset of the sharded data. Each shard can be deployed as a replica set.

* mongos: The mongos acts as a query router, providing an interface between client applications and 
the sharded cluster.

* config servers: Config servers store metadata and configuration settings for the cluster.
 As of MongoDB 3.4, config servers must be deployed as a replica set (CSRS).
 
![](sharded-cluster-production-architecture.svg)

MongoDB shards data at the collection level, distributing the collection data across the shards 
in the cluster.


## Sharded and Non-Sharded Collections

A database can have a mixture of sharded and unsharded collections. Sharded collections are 
partitioned and distributed across the shards in the cluster. Unsharded collections are stored on 
a primary shard. Each database has its own primary shard.

![](sharded-cluster-primary-shard.svg)

## Transactions

https://www.codementor.io/@christkv/mongodb-transactions-vs-two-phase-commit-u6blq7465

* 2PC

A two-phase commit is a standardized protocol that ensures that a database commit is implementing in the situation where a commit operation must be broken into two separate parts.

In database management, saving data changes is known as a commit and undoing changes is
 known as a rollback. Both can be achieved easily using transaction logging when a single 
 server is involved, but when the data is spread across geographically-diverse servers in distributed computing (i.e.,
  each server being an independent entity with separate log records), the process can become more tricky.
  
A special object, known as a coordinator, is required in a distributed transaction. As its name implies, 
the coordinator arranges activities and synchronization between distributed servers. The two-phase commit 
is implemented as follows:

Phase 1 - Each server that needs to commit data writes its data records to the log. If a server is 
unsuccessful, it responds with a failure message. If successful, the server replies with an OK message.

Phase 2 - This phase begins after all participants respond OK. Then, the coordinator sends a signal to 
each server with commit instructions. After committing, each writes the commit as part of its log record 
for reference and sends the coordinator a message that its commit has been successfully implemented. 
If a server fails, the coordinator sends instructions to all servers to roll back the transaction. 
After the servers roll back, each sends feedback that this has been completed.