# Hibernate

#### Hibernate Basics
* Hibernate Core - base service for persistence, it has a query language called HQL (Hibernate Query Language), as well as pro-
grammatic query interfaces for Criteria and Example queries, can be used entirely independently. Contains native API, queries,
XML mapping files.
* Hibernate Annotations - we can use this package on the top of Hibernate Core, hibernate annotations are a set of basic annotations 
that implement the JPA standard, and they’re also a set of extension annotations we need for more advanced mappings and tuning.
* Hibernate EntityManager - another optional module that we can add on top of Hibernate Core, it implements the part of JPA specification
that defines programming interfaces, lifecycle rules for persistence objects and query features.

Hibernate implements significant parts of JPA specification and can be use as JPA implementation or as independent framework with
additional features. To simply check out how it is used we can check the imports: if only the javax.persistence.* import is visible, 
we’re working inside the specification; if we also import org.hibernate.*, we’re using native Hibernate functionality.

#### Hibernate development scenarios
* Top down - we first start with creation of Java classes, and then using __hbm2ddl__ (Hibernate tool) we generate the database schema
```
<property name="hibernate.hbm2ddl.auto">create</property>
```
possible values: validate, update, create, create-drop
* Bottom up - we start with creating a database schema, then we extract metadata from the database and we use this metadata to generate XML mapping files (for example with __hbm2hbmxml__), then with __hbm2java__, the Hibernate mapping metadata is used to generate Java classes

#### Hibernate key interfaces
* Session - it's single-threaded, nonshared object, it represent some particular unit of work with the database, it has the persistance manager API we call to store and load objects. Internally Session contains a queue of SQL statements that need to be synchronized with the database and map of managed persistance instances that are monitored by the Session). We can close session and the managed objects will work just fine but if we try to call the database we will get LazyInitializationException (because there is no Session object to manage the persistance instances).
* Transaction - this API can be used to set transaction boundaries programmatically, it's optional, we can use JDBC transaction instead for example
* Query - database query can be written in Hibernate's own object oriented query language HQL or plain SQL

#### Working with Hibernate
* SessionFactory - is thread-safe, should be singleton, is used to create Session instance (which is single-threaded)
* Annotations
