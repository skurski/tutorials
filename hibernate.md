# Hibernate

#### Hibernate Architecture
![Hibernate architecture Text](http://docs.jboss.org/hibernate/orm/5.1/userguide/html_single/images/architecture/data_access_layers.svg)
Image taken from Hibernate User Guide: [Hibernate documentation](http://docs.jboss.org/hibernate/orm/5.1/userguide/html_single/Hibernate_User_Guide.html)

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

#### Hibernate types
* Hibernate understands both Java and JDBC representations of application data, the Hibernate type is neither a Java type nor a SQL type, it provides information about both of these types and as a result is capable of mapping Java type to SQL type and backwards.
* Hibernate categorizes types into two groups:
  * __Value types__ - is a piece of data that doesn't define its own lifecycle, it is own by an entity type, value types are classified into three subcategories:
    * __Basic types__ - we can see all basic types in Hibernate documentation: [Hibernate basic types](http://docs.jboss.org/hibernate/orm/5.1/userguide/html_single/Hibernate_User_Guide.html#basic), basic types are preceded with __@Basic__ annotation but it can be ignored as it is assumed by default.
    * __Embeddable types__ - in the past it was called __Components__ but JPA calls them embeddables, it is a composition of values, common example is an Address class that is a composition of street, city, zipcode, etc. Address class can be used inside Person class (Address class will be only on the Java side, on the database side Person table will have address data), we can look into Hibernate documentation: [Hibernate embeddable types](http://docs.jboss.org/hibernate/orm/5.1/userguide/html_single/Hibernate_User_Guide.html#embeddables)
    * __Collection types__ - collections can contain almost any other Hibernate type, including: basic types, custom types, embeddable types and references to other entities
  * __Entity types__ - exists independently and define their own lifecycle, entities are classes which correlate to rows in a database table by using __unique identifier__, they are preceded with __@Entity__ annotation

#### Hibernate annotations
* __@Basic__ - all basic types values, assumed by default, we can change the BasicType used by Hibernate by using __@Type(type="newType")__ before property, we can also create our own custome type by implementing one of two interfaces: 
  * org.hibernate.type.BasicType
  * org.hibernate.usertype.UserType
* __@Column__ - indicate that property is a column in the table
* __@Table__ - we can indicate the properties of table, name for example: @Table(name="`table name`"), we can force hibernate to quote an identifier if needed
* __@Enumerated__ - map enum type, two strategies:
  * ORDINAL, by default
  * STRING, @Enumerated(STRING)
* __@Convert(converter=SomeConverter.class)__ - another way to map enums is by using @Converter class that implement AttributeConverter interface and @Convert annotation before property that indicate the converter class
* __@Type__ - custom type mapping, it also can be used to map enums
* __@Lob__ - mapping lobs (database large objects):
  * java.sql.Blob
  * java.sql.Clob
  * java.sql.NClob
* __@Nationalized__ - map specific attribute to a nationalized variant data type: NCHAR, NVARCHAR, LONGNVARCHAR, NCLOB
* __@Temporal__ - map date/time value with indication of type: DATE, TIME or TIMESTAMP, ex: @Temporal(TemporalType=DATE), Java have two types: java.util.Date and java.util.Calendar, SQL standard defines three date/time types:
  * java.sql.Date
  * java.sql.Time
  * java.sql.Timestamp
* __@Generated__ - generated properties are properties that have their values generated by the database (id column for example), if we use generated annotation hibernate execute SELECT statement immediately after SQL INSERT or UPDATE, only __@Version__ and __@Basic__ types can be marked as generated
* __@ColumnTransformer__ - we can customize the SQL that hibernate uses to read and write the values of columns mapped to __@Basic__ types
* __@Formula__ - we can tell database to do some computation for us, this kind of property is read only (its value is calculated by content of formula on the database side and store in property)
* __@Any__ - describes the column holding the metadata information
* __@AnyMetaDef__ / __@AnyMetaDefs__ - link the value of the metadata information to actual entity type
* __@Embedabble__ - used before class, indicate that class is a embeddable value type (component - its lifecycle is bound to parent entity type)
* __@Embedded__ - used before property inside entity type, indicate that property is a component, if we use multiple embeddables and need to change name of property (we want different name in Java and in SQL database) we can use:
  * __@AttributeOverrides__ - we indicate the name and column name
* __@Id__ - identify entity, does not need to be mapped to the column that define the primary key in the database but definitely should be map to column that uniquely identify each row
* __@Transient__ - exclude a field from being part of the entity persistent state
* __@Access__ - override default access strategy
* __@ElementCollection__
* __@CollectioTable__

#### POJO Models requirements by JPA and Hibernate
* use __@Entity__ annotation
* public or protected non-argument constructor define (Hibernate also allows package visibility)
* must be top-level class (Hibernate does not have such restriction)
* cannot be __final__, also methods and persistent instance variables cannot be final (Hibernate allows final classes and setters/getters but generally it's not a good idea)
* no enum or interfaces
* both abstract and concrete classes can be entities

#### Access strategies
* __field-based__ - when we mark field with annotation (ex: @Id) hibernate assume access via field
* __property-based__ - when we mark getter with annotation hibernate assume access via getter
* __@Access__ - the default strategy can be overridden by indicate access type: @Access(AccessType.FIELD)

#### Identifiers and value generation
* Hibernate assume that corresponding database column is:
  * __Unique__
  * __Not null__
  * __Immutable__
* An identifier might be simple or composite

