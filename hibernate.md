# Hibernate

#### Hibernate Architecture
![Hibernate architecture Text](http://docs.jboss.org/hibernate/orm/5.1/userguide/html_single/images/architecture/data_access_layers.svg)
Image taken from Hibernate User Guide

#### Hibernate Basics
* __Hibernate Core__ - base service for persistence, it has a query language called HQL (Hibernate Query Language), as well as pro-
grammatic query interfaces for Criteria and Example queries, can be used entirely independently. Contains native API, queries,
XML mapping files.
* __Hibernate Annotations__ - we can use this package on the top of Hibernate Core, hibernate annotations are a set of basic annotations that implement the JPA standard, and they’re also a set of extension annotations we need for more advanced mappings and tuning.
* __Hibernate EntityManager__ - another optional module that we can add on top of Hibernate Core, it implements the part of JPA specification that defines programming interfaces, lifecycle rules for persistence objects and query features.

Hibernate implements significant parts of JPA specification and can be use as JPA implementation or as independent framework with
additional features. To simply check out how it is used we can check the imports: if only the javax.persistence.* import is visible, 
we’re working inside the specification; if we also import org.hibernate.*, we’re using native Hibernate functionality.

#### Hibernate development scenarios
* __Top down__ - we first start with creation of Java classes, and then using __hbm2ddl__ (Hibernate tool) we generate the database schema
  ```
    <property name="hibernate.hbm2ddl.auto">create</property>
  ```
    possible values: validate, update, create, create-drop
* __Bottom up__ - we start with creating a database schema, then we extract metadata from the database and we use this metadata to generate XML mapping files (for example with __hbm2hbmxml__), then with __hbm2java__, the Hibernate mapping metadata is used to generate Java classes

#### Hibernate key interfaces
![Hibernate class diagram Text](http://docs.jboss.org/hibernate/orm/5.1/userguide/html_single/images/architecture/JPA_Hibernate.svg)
* __SessionFactory__ - a thread-safe, immutable object that acts as a factory for org.hibernate.Session instances. The __EntityManagerFactory__ is the JPA equivalent of a __SessionFactory__, both have basically the same SessionFactory implementation. Creating a __SessionFactory__ is very expensive so it should be only one instance for any given database. The __SessionFactory__ maintains services that Hibernate uses across all __Session__ such as second level caches, connection pools, transaction system integrations, etc.
* __Session__ - it's single-threaded, nonshared object, it represent some particular unit of work with the database. In JPA nomenclature the __Session__ is represented by an __EntityManager__. Internally Session wraps a __JDBC__ java.sql.Connection and acts as a factory for __Transaction__ instances. It contains a queue of SQL statements that need to be synchronized with the database and map of managed persistance instances that are monitored by the Session. We can close session and the managed objects will work just fine but if we try to call the database we will get LazyInitializationException (because there is no Session object to manage the persistance instances).
* __Transaction__ - it's single-threaded object, it is used to set transaction boundaries programmatically, JPA equivalent for this is __EntityTransaction__, (it's optional, we can use JDBC transaction instead for example)
* __Query__ - database query can be written in Hibernate's own object oriented query language (HQL) or plain SQL

#### Working with Hibernate
* __SessionFactory__ - is thread-safe, should be singleton, is used to create Session instance (which is single-threaded)
* __Annotations__
