# Designing applications

Based on book - Designing Data Intensive Applications

## The main goals for data intensive apps:

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

## Data models

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


#### Document vs relational:

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
        
#### Schema vs schemaless:

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
        
#### Data locality for queries:

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
    
#### Nowadays

- most relational database has support for XML documents and the ability to index and query
inside XML documents, postgre and mysql also have a similar level of support for JSON
documents
- on the document database side, RethinkDB supports relational like joins in its query
language, and some MongoDB drivers automatically resolve document references (effectively
performing a client side join, although this is likely to be slower than a join performed
in the database)

#### Query languages

1. Imperative language - tells the computer what to do and HOW to do it (perform certain
operations in a certain order)
2. Declarative language - tells only what to do do but NOT HOW to achieve that, SQL
is exampSQL is a declarative query languagele of such query language

#### MapReduce Quering

- programming model for processing large amounts of data in bulk across many machines.
A limited form of MapReduce is supported by some NoSQL datastores, including MongoDB
and CouchDB.
It is based on the map (also known as collect) and reduce (also known as fold or inject)
functions

---

## Storage and Retrieval

__Index__ - additional data structure that is derived from the primary data. Many databases
allow to add and remove indexes, and this doesn't affect the contents of the database, it
only affects the performance of queries.
- well-chosen indexes speed up read queries
- any kind of index usually slows down writes, because the index also needs to be updated
every time data is written

#### Log structured hash table index

#### LSM tree index (Log structured merge tree and Sorted String Table)

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

#### B-Tree index

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

#### B-Tree and LSM Tree comparison

- generally LSM Trees are faster for writes, whereas B-trees are faster for reads
- advantage of b-tree is that each key exists in exactly one place in the index - better
for strong transactional 
- LSM-trees typically can sustain higher write throughput
- LSM-trees can be compressed better, and thus produce smaller files on disk
- compaction process of log structured storage can sometimes interfere with the 
performance of ongoing reads and writes
- if compaction is not configured carefully, it can happen that compaction cannot keep up
with the rate of incoming writes

#### Other indexing structures

__Clustered index__ 
- storing all row data within the index
- the primary key of a table is always a clustered index

__Nonclusted index__ 
- storing only references to the data within the index

__Covering index__ 
- index with included columns (stores some of a table's columns within the index)
