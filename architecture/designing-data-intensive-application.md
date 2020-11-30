# Designing applications

Based on book - Designing Data Intensive Applications

[Foundations of Data Systems](#foundations-of-data-systems)

[Data models](#data-models)

[Storage and Retrieval](#storage-and-retrieval)

[Encoding and Evolution](#encoding-and-evolution)

[Distributed Data](#distributed-data)


# Foundations of Data Systems

1. Reliability

    - continuing to work correctly, even when things go wrong (faults)
    - system that anticipate faults and can cope with them are called
    fault-tolerant or resilient
    - failure is when the system as w whole stops providing the required
    service
    - therefore the goal is to design fault tolerant system that prevent
    faults from causing failures
    - faults:
        - hardware crashes
        - software errors
        - human errors
        
2. Scalability

    - system has ability to cope with increased load
    - load parameters examples:
        - requests per second to a web server
        - the ratio of reads and writes in a database
        - the number of simultaneously active users
        - it depends on application characteristics
    - describing performance:
        - throughput - the number of records we can process per second
        - latency and response time
            - latency is a duration that a request is waiting to be
            handled (awaiting for service)
            - response time - the actual time to process the request 
            (service time) plus network delays and queueing delays
            - good metric for response time is median (percentiles 
            not average time), median is the 50th percentile, to see how
            bad it looks you can look at higher percentiles: the 95th,
            99th
    - scaling types:
        - scaling up (vertical scaling) - moving to a more powerful 
        machine
        - scaling out (horizontal scaling) - distributing the load across
        multiple smaller machines

3. Maintainability
    - Operability
        - make it easy for dev ops to keep the system running smoothly
        Can be achieved by:
            - good health monitoring
            - good support for automation and integration with standard tools
            - avoiding dependency on individual machine
            - providing good documentation
            - giving admins manual control over the system state when needed
    - Simplicity
        - make it easy for new engineers to understand system, removing as much 
        Can be achieved by:
            - avoid accidental complexity, ex: big ball of mud
            - fallow / choose good architecture design
            - hide implementation under level of abstraction
        complexity as possible from the system
    - Evolvability (extensibility, modifiability, plasticity)
        - make it easy for engineers to make changes to the system in the future, 
        adapting it for unanticipated use cases as requirements change.
        - closely link to simplicity and abstraction: simple and easy to understand system
        are usually easier to modify
        
---

# Data models

Historically first model were hierarchical model and later its main alternative -
 the network model.

- Relational Model
    - Basics:
        - data is organized into tables (relations)
        - each table is unordered collection of rows (tuples)
        - requires translation layer between database and code, object - relational mismatch
        (impedance mismatch)
        (ORM like hibernate reduce the amount of boilerplate code require for this translation 
        layer)
        - normalization vs denormalization
        - added support for structured datatypes and XML (oracle, ibm db2, ms sql, postgresql). 
        A Json datatype is also supported by several databases (ibm db2, mysql, postgresql) 
        - to retrieve data from multiple table we need to either perform multiple queries
        or perform multi-way join between tables
        - declarative query language - SQL (implementation is hidden and query optimizer
        works under the hood)
        - when performing many to one and many to many relationships, the related item is
        referenced by a foreign key
    - Advantages and disadvantages:
        - one to many and many to one relations fit nicely into relational model
        - joins are easy
        - the query optimizer automatically decides which parts of the query to execute in
        which order, and which indexes to use
        - can handle simple cases of many to many relationships, but as the connections 
        within data become more complex, graph is more natural approach
        - it is possible to put graph data in a relational structure but queries will
        be definitely more complex (recursive common table expressions - the 
        WITH RECURSIVE syntax)
- Document Model
    - MongoDB, RethinkDB, CouchDB
    - Basics:
        - support JSON model
        - Json model reduce the impedance mismatch between the application code and the 
        storage layer
        - we can store all relevant information in one place so only one query is sufficient
        to retrieve our data (as opposed to relational model), we can store nested records
        within the parent record rather than in a separate table
        - when performing many to one and many to many relationships, the related item is
        referenced by a document reference
    - Advantages and disadvantages:
        - One to many relations are the main use case or no relationships between records
        - Many to one relation don't fit nicely into the document model
        - support for joins is often week
        - if db does not support joins, we have to emulate a join in application code by
        making multiple queries to the database
- Graph Model
    - Basics:
        - many to many relationships with highly interconnected data -> anything is 
        potentially related to everything
        - consists of two kinds of objects: vertices (nodes, entites) and edges 
        (relationships, arcs)
        - examples:
            - social graphs
            - the web graph
            - road or rail networks
        - graph types:
            - __property graph__ model
                - Neo4j, Titan, InfiniteGraph
            - __triple store__ model
                - Datomic, AllegroGraph
        - declarative query languages for graphs:
            - Cypher (Neo4j), SPARQL, Datalog
        
    - Advantages and disadvantages:
        - good for evolvability: as we add features to our application, a graph can easily
        be extended to accommodate changes in our application's data structures
        - any vertex can have an edge to any other vertex
        - we can refer directly to any vertex by its unique ID, or we can use an index
        to find vertices with a particular value
        - vertices and edges are not ordered
        - it is possible to write our traversal in imperative code but mot graph databases
        support declarative query languages
        - just like document database, graph storage don't enforce a schema


## Document vs relational:

1. Document
    - schema flexibility
    - better performance due to lacality
    - for some applications it is closer to the data structures used by the application
    (no need for things like ORM)
    - great fit if data in application has a document like structure (tree of 
    one to many relations)
    - limitations: we cannot refer directly to a nested item within a document
    
2. Relational
    - better support for joins and many to one and many to many relations 
        
## Schema vs schemaless:

The difference is particularly noticeable in situations where an application wants
        to change the format of its data
    
1. document database: 
    - don't need to do anything, just start writing new documents, application 
        code needs to handle the case when old documents are read
    - schema on read (schemaless) is advantageous if the items in the collection
        don't all have the same structure for some reason:
        * there are many different types of object, and it is not practicable to 
        put each type of object in its own table
        * the structure of the data is determined by external systems and which
        may change at any time 

2. relational database: 
    - we need to perform migration, most relational database systems
        execute the ALTER TABLE statement in a few miliseconds, MySQL is a notable 
        exception (it copies the entire table, which mean minutes or even hours of downtime
        when altering a large table - work around tools exists)
    - in cases where all records are expected to have the same structure, schemas are
        a useful mechanism for documenting and enforcing that structure

- UPDATE statement on a large table is likely to be slow on any database, since
every row needs to be rewritten
        
## Data locality for queries:

1. document database:
    - document is usually stored as a single continuous string, encoded as JSON,
    XML, or a binary variant thereof (MongoDB's BSON) -> if application needs to access
    the entire document, there is a performance advantage
    - if we do not need the entire document, just small part of it, the database
    typically needs to load the entire document, which can be wasteful on large document
    - on updates to a document, the entire document usually needs to be rewritten - only
    modifications that don't change the encoded size of a document can easily be performed
     in place
2. relational database:
    - if the data is split across multiple tables, multiple lookups are required to
    retrieve it all
    
## Nowadays

- most relational database has support for XML documents and the ability to index and query
inside XML documents, postgre and mysql also have a similar level of support for JSON
documents
- on the document database side, RethinkDB supports relational like joins in its query
language, and some MongoDB drivers automatically resolve document references (effectively
performing a client side join, although this is likely to be slower than a join performed
in the database)

## Query languages

1. Imperative language - tells the computer what to do and HOW to do it (perform certain
operations in a certain order)
2. Declarative language - tells only what to do do but NOT HOW to achieve that, SQL
is exampSQL is a declarative query languagele of such query language

## MapReduce Quering

- programming model for processing large amounts of data in bulk across many machines.
A limited form of MapReduce is supported by some NoSQL datastores, including MongoDB
and CouchDB.
It is based on the map (also known as collect) and reduce (also known as fold or inject)
functions

---

# Storage and Retrieval

__Index__ - additional data structure that is derived from the primary data. Many databases
allow to add and remove indexes, and this doesn't affect the contents of the database, it
only affects the performance of queries.
- well-chosen indexes speed up read queries
- any kind of index usually slows down writes, because the index also needs to be updated
every time data is written

## Log structured hash table index

## LSM tree index (Log structured merge tree and Sorted String Table)

The algorithm describe below is essentially what is used in LevelDB, RocksDB, key-value
storage engine libraries. Similar storage engines are used in __Cassandra and HBase__, both
of which were inspired by Google's Bigtable paper.
__Lucene__, an indexing engine for full-test search used by Elastisearch and Solr, uses
a similar method for storing its term dictionary.

1. When a write comes in, add it to an in-memory balanced tree data structure
(for example, a red black tree). This in-memory tree is sometimes called a __memtable__
2. When a memtable gets bigger than some threshold - typically a few megabytes - write it
out to disk as an __SSTable file (Sorted String Table)__. This can be done efficiently
because the tree already maintains the key-value pairs sorted by key. The new SSTable
file becomes the most recent segment of the database. While the SSTable is being written
out to disk, writes can continue to  a new memtable instance.
3. In order to serve a read request, first try to find the key in the memtable, then in
the moest recent on-disk segment, then in the next-older segment, etc.
4. From time to time, run a merging and compaction process in the background to combine
segment files and to discard overwritten or deleted values.
5. In order to resolve problem with database crashes and lost the data from memtable - 
we can keep a separate __log__ on disk to which every write is immediately appended.
That log is not in sorted order, but that doesn't matter, because its only purpose is to
restore the memtable after a crash. Every time a memtable is written out to an SSTable, 
the corresponding log can be discarded.

__Performance problems__:

- LSM tree algorithm can be slow when looking up keys that do not exists in the database
-> we have to check the memtable, then the segments all the way back to the oldest.
Solution is to use additional __Bloom filter__ - in-memory efficient data structure for
approximating the contents of a set.
- different strategies to determine the order and timing of how SSTables are compacted
and merged
- can support remarkably high write throughput

## B-Tree index

- B-trees keep key-value pairs sorted by key, which allows efficient key-value lookups and
range queries
- break the database down into fixed-size blocks or pages, traditionally 4 KB in size.
and read or write one page at a time. Each page can be identified using an address or
location, which allows one page to refer to another - similar to a pointer, but on disk
instead of in memory.
- one page is designated as the root of the B-tree, whenever we want to look up a key
in the index, we start here
- the number of references to child pages in one page is called __branching factor__, 
typically it is several hundred
- to update the value of existing key, we search for the leaf page containing that key,
change the value and write the page back to disk
- to add a new key, we need to find the page whose range encompasses the new key and add it
to that page, if there isn't enough free space, it is split into two half-full pages, and
the parent page is updated to account for the new subdivision of key ranges
- B-tree with n keys always has a depth of O(log n)
- to prevent corrupted index after database crashes it is common to include an 
additional data structure on disk: __a write ahead log (WAL, known also as redo log)__.
This is append only file to which every modification must be written before it can be
applied to the pages of the tree itself.
- updating pages in place require also more careful concurrency control

## B-Tree and LSM Tree comparison

- generally LSM Trees are faster for writes, whereas B-trees are faster for reads
- advantage of b-tree is that each key exists in exactly one place in the index - better
for strong transactional 
- LSM-trees typically can sustain higher write throughput
- LSM-trees can be compressed better, and thus produce smaller files on disk
- compaction process of log structured storage can sometimes interfere with the 
performance of ongoing reads and writes
- if compaction is not configured carefully, it can happen that compaction cannot keep up
with the rate of incoming writes

## Other indexing structures

__Clustered index__ 
- storing all row data within the index
- the primary key of a table is always a clustered index

__Nonclusted index__ 
- storing only references to the data within the index

__Covering index__ 
- index with included columns (stores some of a table's columns within the index)

> to be continue... (not finished)

---

# Encoding and Evolution

## Encoding basics 

__Encoding__ is the process of converting data from one form to another.
While "encoding" can be used as a verb, it is often used as a noun, 
and refers to a specific type of encoded data. 

Programs usually work with data in (at least) two different representations:
1. __In memory__, data is kept in objects, structs, lists, arrays, hash tables, 
trees, and so on. These data structures are optimized for efficient access 
and manipulation by the CPU (typically using pointers).
2. When you want to write data to a file or send it over the network, 
you have to encode it as some kind of self-contained __sequence of bytes 
(for example, a JSON document)__. Since a pointer wouldn’t make sense to 
any other process, this sequence-of-bytes representation looks quite 
different from the data structures that are normally used in memory.

Thus, we need some kind of translation between the two representations. 
The translation from the in-memory representation to a byte sequence is 
called __encoding__ (also known as serialization or marshalling), and the 
reverse is called __decoding__ (parsing, deserialization, unmarshalling).

### Language-Specific Formats

Many programming languages come with built-in support for encoding in-memory 
objects into byte sequences. For example, Java has java.io.Serializable.

These encoding libraries are very convenient, because they allow in-memory 
objects to be saved and restored with minimal additional code. However, 
they also have a number of deep problems:
* The encoding is often tied to a particular programming language, 
and reading the data in another language is very difficult.
* In order to restore data in the same object types, the decoding process 
needs to be able to instantiate arbitrary classes. This is frequently a 
source of security problems: if an attacker can get your application to 
decode an arbitrary byte sequence, they can instantiate arbitrary classes, 
which in turn often allows them to do terrible things such as remotely 
executing arbitrary code.
* Versioning data is often an afterthought in these libraries: as they 
are intended for quick and easy encoding of data, they often neglect 
the inconvenient problems of forward and backward compatibility.
* Efficiency (CPU time taken to encode or decode, and the size of the 
encoded structure) is also often an afterthought. For example, Java’s 
built-in serialization is notorious for its bad performance and bloated 
encoding.

For these reasons it’s generally a bad idea to use your language’s 
built-in encoding for anything other than very transient purposes.

### JSON, XML, and Binary Variants

JSON, XML, and CSV are textual formats, and thus somewhat human-readable. 
Besides the superficial syntactic issues, they also have some subtle problems:
* There is a lot of ambiguity around the encoding of numbers. In XML and 
CSV, you cannot distinguish between a number and a string that happens 
to consist of digits (except by referring to an external schema). 
JSON distinguishes strings and numbers, but it doesn’t distinguish 
integers and floating-point numbers, and it doesn’t specify a precision.

    This is a problem when dealing with large numbers; for example, integers 
    greater than 253 cannot be exactly represented in an IEEE 754 
    double-precision floating-point number, so such numbers become inaccurate 
    when parsed in a language that uses floating-point numbers 
    (such as JavaScript). An example of numbers larger than 253 occurs on 
    Twitter, which uses a 64-bit number to identify each tweet. The JSON 
    returned by Twitter’s API includes tweet IDs twice, once as a JSON 
    number and once as a decimal string, to work around the fact that the 
    numbers are not correctly parsed by JavaScript applications.
* JSON and XML have good support for Unicode character strings, but 
they don’t support binary strings (sequences of bytes without a character 
encoding). Binary strings are a useful feature, so people get around 
this limitation by encoding the binary data as text using Base64. The 
schema is then used to indicate that the value should be interpreted 
as Base64-encoded. This works, but it’s somewhat hacky and increases 
the data size by 33%.
* There is optional schema support for both XML and JSON. These schema 
languages are quite powerful, and thus quite complicated to learn and 
implement. Use of XML schemas is fairly widespread, but many JSON-based 
tools don’t bother using schemas. Since the correct interpretation of 
data (such as numbers and binary strings) depends on information in 
the schema, applications that don’t use XML/JSON schemas need to 
potentially hardcode the appropriate encoding/decoding logic instead.
* CSV does not have any schema, so it is up to the application to 
define the meaning of each row and column. If an application change 
adds a new row or column, you have to handle that change manually. 
CSV is also a quite vague format (what happens if a value contains a 
comma or a newline character?). Although its escaping rules have been 
formally specified, not all parsers implement them correctly.

### BINARY ENCODING

For data that is used only internally within your organization, there 
is less pressure to use a lowest-common-denominator encoding format. 
For example, you could choose a format that is more compact or faster 
to parse.

JSON is less verbose than XML, but both still use a lot of space compared 
to binary formats. This observation led to the development of a profusion 
of binary encodings for JSON (MessagePack, BSON, BJSON, UBJSON, BISON, 
and Smile, to name a few) and for XML (WBXML and Fast Infoset, for example).


__Apache Thrift__ and __Protocol Buffers__ are binary encoding libraries 
that are based on the same principle. Protocol Buffers was originally 
developed at Google, Thrift was originally developed at Facebook, 
and both were made open source in 2007–08. Both Thrift and Protocol 
Buffers require a schema for any data that is encoded.

__Apache Avro__ is another binary encoding format that is interestingly 
different from Protocol Buffers and Thrift. It was started in 2009 as a 
subproject of Hadoop, as a result of Thrift not being a good fit for 
Hadoop’s use cases. Avro also uses a schema to specify the structure 
of the data being encoded. It has two schema languages: one (Avro IDL) 
intended for human editing, and one (based on JSON) that is more 
easily machine-readable.

Binary encodings based on schemas are also a viable option. They have a 
number of nice properties:
* They can be much more compact than the various “binary JSON” variants, 
since they can omit field names from the encoded data.
* The schema is a valuable form of documentation, and because the schema 
is required for decoding, you can be sure that it is up to date 
(whereas manually maintained documentation may easily diverge from reality).
* Keeping a database of schemas allows you to check forward and backward 
compatibility of schema changes, before anything is deployed.
* For users of statically typed programming languages, the ability to 
generate code from the schema is useful, since it enables type checking 
at compile time.

## Data flows

1. Via databases 
2. Via service calls 
3. Via asynchronous message passing 

### Dataflow through databases

### Dataflow through service calls - REST and RPC

#### REST

When you have processes that need to communicate over a network, most 
common arrangement is to have two roles: __clients and servers__. 
The servers expose an API over the network, and the clients can connect 
to the servers to make requests to that API. The API exposed by the 
server is known as a service.

When HTTP is used as the underlying protocol for talking to the service, 
it is called a webservice. For example:
* A client application running on a user’s device making requests to a 
service over HTTP. These requests typically go over the public internet.
* One service making requests to another service owned by the same 
organization, often located within the same data center, as part of a 
service-oriented/microservices architecture. (Software that supports 
this kind of use case is sometimes called middleware.)
* One service making requests to a service owned by a different 
organization, usually via the internet. This is used for data exchange 
between different organizations’ backend systems. This category includes 
public APIs provided by online services, such as credit card processing 
systems, or OAuth for shared access to user data.

There are two popular approaches to web services: __REST and SOAP__.
1. __REST__ is not a protocol, but rather a design philosophy that builds 
upon the principles of HTTP. It emphasizes simple data formats, using 
URLs for identifying resources and using HTTP features for cache control, 
authentication, and content type negotiation. REST has been gaining 
popularity compared to SOAP, at least in the context of 
cross-organizational service integration, and is often associated with 
microservices. An API designed according to the principles of REST is 
called RESTful.
    * RESTful APIs tend to favor simpler approaches, typically involving 
    less code generation and automated tooling. A definition format such 
    as OpenAPI, also known as Swagger, can be used to describe RESTful 
    APIs and produce documentation.
2. By contrast, __SOAP__ is an XML-based protocol for making network API 
requests. Although it is most commonly used over HTTP, it aims to be 
independent from HTTP and avoids using most HTTP features. Instead, it 
comes with a sprawling and complex multitude of related standards 
(the web service framework, known as WS-*) that add various features.
    * The API of a SOAP web service is described using an XML-based 
    language called the Web ServicesDescription Language, or WSDL. 
    WSDL enables code generation so that a client can access a remote 
    service using local classes and method calls (which are encoded to 
    XML messages and decoded again by the framework). This is useful 
    in statically typed programming languages, but less so in dynamically 
    typed ones.
    
#### RPC

The Remote Procedure Call (RPC) model tries to make a request to a remote 
network service look the same as calling a function or method in your 
programming language, within the same process (this abstraction is 
called location transparency).

REST seems to be the predominant style for public APIs. The main focus 
of RPC frameworks is on requests between services owned by the same 
organization, typically within the same data center.

#### Evolution

The backward and forward compatibility properties of an RPC scheme are 
inherited from whatever encoding it uses: 
* Thrift, gRPC (Protocol Buffers), and Avro RPC can be evolved according 
to the compatibility rules of the respective encoding format.
* In SOAP, requests and responses are specified with XML schemas. These 
can be evolved, but there are some subtle pitfalls.
* RESTful APIs most commonly use JSON (without a formally specified 
schema) for responses, andJSON or URI-encoded/form-encoded request 
parameters for requests. Adding optional request parameters and adding 
new fields to response objects are usually considered changes that 
maintain compatibility.

### Message passing dataflow

At asynchronous message-passing systems request (usually called a message) 
is not sent via a direct network connection, but goes via an intermediary 
called a message broker (also called a message queue or 
message-oriented middleware), which stores the message temporarily.

Using a message broker has several advantages:
* It can act as a buffer if the recipient is unavailable or overloaded, 
and thus improve system reliability.
* It can automatically redeliver messages to a process that has crashed, 
and thus prevent messages from being lost.
* It avoids the sender needing to know the IP address and port number of 
the recipient (which is particularly useful in a cloud deployment where 
virtual machines often come and go).
* It allows one message to be sent to several recipients.
* It logically decouples the sender from the recipient (the sender just 
publishes messages and doesn’t care who consumes them).

Communication via message broker is usually one-way: a sender normally 
doesn’t expect to receive a reply to its messages. It is possible for 
a process to send a response, but this would usually be done on a 
separate channel. This communication pattern is asynchronous: the sender 
doesn’t wait for the message to be delivered, but simply sends it and 
then forgets about it.

Most popular open-source message brokers:
* RabbitMQ, ActiveMQ, HornetQ, NATS, Apache Kafka

How it works:
* one process sends a message to a named queue or topic, and the broker 
ensures that the message is delivered to one or more consumers of or 
subscribers to that queue or topic. There can be many producers and many 
consumers on the same topic.A topic provides only one-way data flow. 
However, a consumer may itself publish messages to another topic, or to 
a reply queue that is consumed by the sender of the original message 
(allowing a request/response data flow).

---

# Distributed Data

There are various reasons why you might want to distribute a database 
across multiple machines:    
1. __Scalability__        
    If your data volume, read load, or write load grows bigger than a 
    single machine can handle, you can potentially spread the load across 
    multiple machines.        
2. __Fault tolerance/high availability__        
    If your application needs to continue working even if one machine 
    (or several machines, or the network, or an entire datacenter) 
    goes down, you can use multiple machines to give you redundancy.  
    When one fails, another one can take over.        
3. __Latency__                
    If you have users around the world, you might want to have servers 
    at various locations worldwide so that each user can be served from 
    a datacenter that is geographically close to them. That avoids the 
    users having to wait for network packets to travel halfway around 
    the world.
    
## Scaling to Higher Load
 
__Vertical scaling (also called scaling up):__

1. Shared-memory architecture

    Many CPUs, many RAM chips, and many disks can be joined together 
    under one operating system, and a fast interconnect allows any CPU 
    to access any part of the memory or disk. All the components can be 
    treated as a single machine
    
2. Shared-disk architecture

    Uses several machines with independent CPUs and RAM, but stores data 
    on an array of disks that is shared between the machines, which are 
    connected via a fast network
    
    
__Horizontal scaling (scaling out - shared nothing architecture):__

1. In this approach, each machine or virtual machine running the database 
software is called a node. Each node uses its CPUs, RAM, and disks 
independently. Any coordination between nodes is done at the software 
level, using a conventional network.


__There are two common ways data is distributed across multiple nodes:__

1. __Replication__

    Keeping a copy of the same data on several different nodes, 
    potentially in different locations. Replication provides redundancy: 
    if some nodes are unavailable, the data can still be served from the 
    remaining nodes. Replication can also help improve performance.
    
2. __Partitioning (Sharding)__

    Splitting a big database into smaller subsets called partitions so 
    that different partitions can be assigned to different nodes (also 
    known as sharding).
    
These are separate mechanisms, but they often go hand in hand, for example:

[https://docs.mongodb.com/manual/sharding/](https://docs.mongodb.com/manual/sharding/)

---

# Replication

All of the difficulty in replication lies in handling changes to replicated 
data, three popular algorithms for replicating changes between nodes: 

1. __single-leader__, 
2. __multi-leader__, 
3. __leaderless replication__.

## Leader based replication

Each node that stores a copy of the database is called a replica.

Every write to the database needs to be processed by every replica; 
otherwise, the replicas would no longer contain the same data. The most 
common solution for this is called leader-based replication (also known 
as active/passive or master–slave replication). It works as follows:

1. One of the replicas is designated the leader (also known as master or 
primary). When clients want to write to the database, they must send 
their requests to the leader, which first writes the new data to its 
local storage.

2. The other replicas are known as followers (read replicas, slaves, 
secondaries, or hot standbys). Whenever the leader writes new data to 
its local storage, it also sends the data change to all of its followers 
as part of a replication log or change stream. Each follower takes the 
log from the leader and updates its local copy of the database 
accordingly, by applying all writes in the same order as they were 
processed on the leader.

3. When a client wants to read from the database, it can query either 
the leader or any of the followers. However, writes are only accepted on 
the leader (the followers are read-only from the client’s point of view).

This mode of replication is a built-in feature of many relational databases, 
such as PostgreSQL(since version 9.0), MySQL, Oracle Data Guard,and SQL 
Server’s AlwaysOn Availability Groups. It is also used in some 
non relational databases, including MongoDB, RethinkDB, and Espresso. 
Finally, leader-based replication is not restricted to only databases: 
distributed message brokers such as Kafka and RabbitMQ highly available 
queues also use it. Some network filesystems and replicated block devices 
such as DRBD are similar.

### Synchronous Versus Asynchronous Replication

An important detail of a replicated system is whether the replication 
happens synchronously or asynchronously. (In relational databases, 
this is often a configurable option; other systems are often hardcoded 
to be either one or the other.)

The advantage of synchronous replication:
* an up-to-date copy of the data that is consistent with the leader

The disadvantage is: 
* if the synchronous follower doesn’t respond (because it has crashed, or 
there is a network fault, or for any other reason), the write cannot 
be processed. The leader must block all writes and wait until the 
synchronous replica is available again.

In practice, if you enable synchronous replication on a database, it 
usually means that one of the followers is synchronous, and the others 
are asynchronous. This configuration is sometimes also called 
__semi-synchronous__.

Often, leader-based replication is configured to be __completely 
asynchronous__. In this case, if the leader fails and is not recoverable, 
any writes that have not yet been replicated to followers are lost. 
This means that a write is not guaranteed to be durable, even if it has 
been confirmed to the client. However, a fully asynchronous 
configuration has the advantage that the leader can continue processing 
writes, even if all of its followers have fallen behind.

Leader-based replication requires all writes to go through a single 
node, but read-only queries can go to any replica. For workloads that 
consist of mostly reads and only a small percentage of writes(a common 
pattern on the web), there is an attractive option: create many 
followers, and distribute the read requests across those followers. 
This removes load from the leader and allows read requests to be served 
by nearby replicas.

This approach only realistically works with asynchronous replication—if 
you tried to synchronously replicate to all followers, a single node 
failure or network outage would make the entire system unavailable for 
writing.

Unfortunately, if an application reads from an asynchronous follower, 
it may see outdated information if the follower has fallen behind. 
This leads to apparent inconsistencies in the database: if you run the 
same query on the leader and a follower at the same time, you may get 
different results, because not all writes have been reflected in the 
follower. This inconsistency is just a temporary state—if you stop 
writing to the database and wait a while, the followers will eventually 
catch up and become consistent with the leader. For that reason, this 
effect is known as __eventual consistency__.


