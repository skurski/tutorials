# Hibernate

#### Hibernate Architecture
![Hibernate architecture Text](http://docs.jboss.org/hibernate/orm/5.1/userguide/html_single/images/architecture/data_access_layers.svg)
Image taken from Hibernate User Guide: [Hibernate documentation](http://docs.jboss.org/hibernate/orm/5.1/userguide/html_single/Hibernate_User_Guide.html)

===

#### Hibernate Basics
* __Hibernate Core__ - base service for persistence, it has a query language called HQL (Hibernate Query Language), as well as pro-
grammatic query interfaces for Criteria and Example queries, can be used entirely independently. Contains native API, queries,
XML mapping files.
* __Hibernate Annotations__ - we can use this package on the top of Hibernate Core, hibernate annotations are a set of basic annotations that implement the JPA standard, and they’re also a set of extension annotations we need for more advanced mappings and tuning.
* __Hibernate EntityManager__ - another optional module that we can add on top of Hibernate Core, it implements the part of JPA specification that defines programming interfaces, lifecycle rules for persistence objects and query features.

Hibernate implements significant parts of JPA specification and can be use as JPA implementation or as independent framework with
additional features. To simply check out how it is used we can check the imports: if only the javax.persistence.* import is visible, 
we’re working inside the specification; if we also import org.hibernate.*, we’re using native Hibernate functionality.

===

#### Hibernate development scenarios
* __Top down__ - we first start with creation of Java classes, and then using __hbm2ddl__ (Hibernate tool) we generate the database schema
  ```
    <property name="hibernate.hbm2ddl.auto">create</property>
  ```
    possible values: validate, update, create, create-drop
* __Bottom up__ - we start with creating a database schema, then we extract metadata from the database and we use this metadata to generate XML mapping files (for example with __hbm2hbmxml__), then with __hbm2java__, the Hibernate mapping metadata is used to generate Java classes

===

#### Hibernate key interfaces
![Hibernate class diagram Text](http://docs.jboss.org/hibernate/orm/5.1/userguide/html_single/images/architecture/JPA_Hibernate.svg)
* __SessionFactory__ - a thread-safe, immutable object that acts as a factory for org.hibernate.Session instances. The __EntityManagerFactory__ is the JPA equivalent of a __SessionFactory__, both have basically the same SessionFactory implementation. Creating a __SessionFactory__ is very expensive so it should be only one instance for any given database. The __SessionFactory__ maintains services that Hibernate uses across all __Session__ such as second level caches, connection pools, transaction system integrations, etc.
* __Session__ - it's single-threaded, nonshared object, it represent some particular unit of work with the database. In JPA nomenclature the __Session__ is represented by an __EntityManager__. Internally Session wraps a __JDBC__ java.sql.Connection and acts as a factory for __Transaction__ instances. It contains a queue of SQL statements that need to be synchronized with the database and map of managed persistance instances that are monitored by the Session. We can close session and the managed objects will work just fine but if we try to call the database we will get LazyInitializationException (because there is no Session object to manage the persistance instances).
* __Transaction__ - it's single-threaded object, it is used to set transaction boundaries programmatically, JPA equivalent for this is __EntityTransaction__, (it's optional, we can use JDBC transaction instead for example)
* __Query__ - database query can be written in Hibernate's own object oriented query language (HQL) or plain SQL

===

#### Hibernate types
* Hibernate understands both Java and JDBC representations of application data, the Hibernate type is neither a Java type nor a SQL type, it provides information about both of these types and as a result is capable of mapping Java type to SQL type and backwards.
* Hibernate categorizes types into two groups:
  * __Value types__ - is a piece of data that doesn't define its own lifecycle, it is own by an entity type, value types are classified into three subcategories:
    * __Basic types__ - we can see all basic types in Hibernate documentation: [Hibernate basic types](http://docs.jboss.org/hibernate/orm/5.1/userguide/html_single/Hibernate_User_Guide.html#basic), basic types are preceded with __@Basic__ annotation but it can be ignored as it is assumed by default.
    * __Embeddable types__ - in the past it was called __Components__ but JPA calls them embeddables, it is a composition of values, common example is an Address class that is a composition of street, city, zipcode, etc. Address class can be used inside Person class (Address class will be only on the Java side, on the database side Person table will have address data), we can look into Hibernate documentation: [Hibernate embeddable types](http://docs.jboss.org/hibernate/orm/5.1/userguide/html_single/Hibernate_User_Guide.html#embeddables)
    * __Collection types__ - collections can contain almost any other Hibernate type, including: basic types, custom types, embeddable types and references to other entities
  * __Entity types__ - exists independently and define their own lifecycle, entities are classes which correlate to rows in a database table by using __unique identifier__, they are preceded with __@Entity__ annotation

===

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

===

#### POJO Models requirements by JPA and Hibernate
* use __@Entity__ annotation
* public or protected non-argument constructor define (Hibernate also allows package visibility)
* must be top-level class (Hibernate does not have such restriction)
* cannot be __final__, also methods and persistent instance variables cannot be final (Hibernate allows final classes and setters/getters but generally it's not a good idea)
* no enum or interfaces
* both abstract and concrete classes can be entities

===

#### Access strategies
Hibernate use access strategy based on where we put @Id annotation:
* __field-based__ - when we mark field with annotation hibernate assume access via field
* __property-based__ - when we mark getter with annotation hibernate assume access via getter
* __@Access__ - the default strategy can be overridden by indicate access type: @Access(AccessType.FIELD)

===

#### Fields
Not all annotations are necessery, for example @Table, @Column, @JoinColumn are not obligatory. Hibernate assumes that all fields inside entity object are persistent fields and name of table, columns correspond to fields names.

===

#### Identifiers and value generation
* Hibernate assume that corresponding database column is:
  * __Unique__
  * __Not null__
  * __Immutable__
* An identifier might be simple (single value) or composite (multiple values):
  * __Simple identifiers__ - map to single basic attribute, annotated with __javax.persistence.Id__
    * to support portability according to JPA only the following types should be used:
      * any Java primitive and primitive wrapper
      * java.lang.String, java.util.Date, java.sql.Date, java.math.BigDecimal, java.math.BigInteger
    * Values for simple identifiers can be generated, to generate value we use __javax.persistence.GeneratedValue__ annotation
  * __Composite identifiers__ - one or more persistent attributes, general rules
    * must be represented by a "primary key class" using __javax.persistence.EmbeddedId__ or __javax.persistence.IdClass__ annotation
      * composite field is annotated with __@EmbeddedId__
      * primary key class is annotated with __@Embeddable__
      * when we use __@IdClass__ we annotate entity class and indicate primary key class: @IdClass(Some.class)
    * primary key class must be public with no-arg public constructor
    * primary key class must be serializable
    * primary key class must define equals and hashCode methods
* __Generated identifier values__
  * Hibernate supports number of different types to generate value but JPA portably defines that only integer types are advised
  * is indicates using __javax.persistence.GeneratedValue__ annotation
  * how value is generated specifies __javax.persistence.GenerationType__:
    * AUTO (default) - Hibernate should chose appropriate generation strategy 
    * IDENTITY - database identity columns will be used for primary key value generation
      * the entity row we MUST be physically inserted before the identifier will be known
      * hibernate is not able to JDBC batching for inserts of the entities
    * SEQUENCE - database sequence should be used for obtaining primary key values
    * TABLE - database table should be used for obtaining primary key values

===

#### Associations
* __@ManyToOne__
  * equivalent in database e.g. foreign key
  * establishes a relationship between a child entity and a parent
  * when we update entity that contains @ManyToOne association hibernate will set the associated database foreign key column 
* __@OneToMany__
  * links a parent entity with one or more child entities
  * if @OneToMany doesn't have mirroring @ManyToOne association on the child side than the @OneToMany is unidirectional, if there is a @ManyToOne than association is bidirectional
  * undirectional association is possible if we have many to many relationship on the database and intermediate table between parent and child
  * bidirectional association have one owning side (the child side), the other referred to as mappedBy side: @OneToMany(mappedBy=...)
  * when bidirectional association is formed both sides must be synchronized, e.g. childs methods synchronize both ends when child element is added or removed
* __@OneToOne__
  *  can be unidirectional or bidirectional, unidirectional follows the database foreign key semantics, bidirectional adds @OneToOne to parent side too
*  __@ManyToMany__ 
  * requires a link table that joins two entities
  * can be unidirectional or bidirectional
  * similar to unidirectional @OneToMany associations, the link table is controlled by the owning side

===

#### Collections of entities
* Entity collections can represent __@OneToMany__ and __@ManyToOne__ association
* Collection of __Value Type__ must use the __@ElementCollection__ annotation
* Collections not marked as @OneToMany, @ManyToOne, @ElementCollection require a custom __@Type__ annotation and the collection elements must be stored in a single database column
* Categorizing by the underlying collection type:
  * __bags__ - unordered lists
    * can be unidirectional or bidirectional, 
    * use the __List__ interface on java side but don't retain element order
  * __indexed lists__ - ordered lists
    * also use __List__ interface but to preserve element order one of 2 annotations needs to be used on the field:
      * __@OrderBy__
      * __@OrderColumn__ 
  * __sets__ - use __Set__ interface
  * __sorted sets__ - use __SortedSet__ interface
    * __@SortNatural__ - indicate that Hibernate relies on the natural sorting order (Comparable implementation on java side)
    * __@SortComparator__ - provide custom sorting logic e.g. @SortComparator(ReverseComparator.class)
  * __maps__ - an entity can either be a map key or a map value, Hibernate allows using the following map keys:
    * __@MapKeyColumn__ - the map key is a column in database table
    * __@MapKey__ - the map key is the primary key or another property of the entity stored as a map entry value
    * __@MapKeyEnumerated__ - the map key is an Enum of the target child entity
    * __@MapKeyTemporal__ - the map key is a Date or a Calendar of the target child entity
    * __@MapKeyJoinColumn__ - the map key is a an entity mapped as an assosation in the child entity that's stored as a map entry key
  * __sorted maps__
  * __arrays__ - hibernate doesn't support native database array types, java arrays are relevent for basic types only

===

#### Natural Ids
* __@NaturalId__ - We can inform Hibernate that entity has some meaningful property that can uniquely identify entity but its not an entity identifier (Primary Key)
* Hibernate provides API for loading entity by its natural id
* By default @NaturalId is immutable, we can allow entity attribute to change by using @NaturalId(mutable=true)

===

#### Inheritance
* __@MappedSuperclass__ 
  * inheritance is implemented in domain model without reflecting it in the database
  * @MappedSuperclass exists only in the domain model, @Entity classes that extends superclass exists in database
  * it is impossible to use polymorphic queries (fetching subclass by their base class) because base class don't exists in DB
* __@Inheritance(strategy=InheritanceType.SINGLE_TABLE)__
  *  single table is default inheritance strategy
  *  maps all subclasses to only one database table
  *  each class declares its own persistent properties
  *  we use @Entity and @Inheritance annotations on root class (which exists in DB)
*  __@Inheritance(strategy=InheritanceType.JOINED)__
  *  table-per-sublass strategy, each subclass is mapped to its own table
  *  we use @Entity and @Inheritance annotations on root class and extend this class
*  __@Inheritance(strategy=InheritanceType.TABLE_PER_CLASS)__
  * we map only the concrete classes of an inheritance hierarchy to tables    

===

#### Bootstrap
* for building __SessionFactory__ (Hibernate interface)
  * Build the __StandardServiceRegistry__
  * Build the __Metadata__
  * Build the __SessionFactory__ by using those two
    ```java
    StandardServiceRegistry standardRegistry = new StandardServiceRegistryBuilder()
        .configure( "org/hibernate/example/hibernate.cfg.xml" )
        .build();

    Metadata metadata = new MetadataSources( standardRegistry )
        .addAnnotatedClass( MyEntity.class )
        .addAnnotatedClassName( "org.hibernate.example.Customer" )
        .addResource( "org/hibernate/example/Order.hbm.xml" )
        .addResource( "org/hibernate/example/Product.orm.xml" )
        .getMetadataBuilder()
        .applyImplicitNamingStrategy( ImplicitNamingStrategyJpaCompliantImpl.INSTANCE )
        .build();
    
    SessionFactoryBuilder sessionFactoryBuilder = metadata.getSessionFactoryBuilder();
    
    // Supply an SessionFactory-level Interceptor
    sessionFactoryBuilder.applyInterceptor( new CustomSessionFactoryInterceptor() );
    
    // Add a custom observer
    sessionFactoryBuilder.addSessionFactoryObservers( new CustomSessionFactoryObserver() );
    
    // Apply a CDI BeanManager ( for JPA event listeners )
    sessionFactoryBuilder.applyBeanManager( getBeanManager() );
    
    SessionFactory sessionFactory = sessionFactoryBuilder.build();
    ```
* for building __EntityManagerFactory__ (JPA interface)
  * define persistent units in META-INF/persistence.xml 
  * EntityManagerFactory will be available via __javax.persistence0PersistenceUnit__ annotation
    ```java
    @PersistenceUnit
    private EntityManagerFactory emf;
    ```
  * we can configure JPA even without persistence.xml by using __EntityManagerFactoryBuilderImpl__
  
===

#### Persistence Contexts
* Both __org.hibernate.Session__ API and __javax.persistence.EntityManager__ API represent a context for dealing with persistent data
* Persistent data states:
  * __transient__ - the entity has just been instantiated and is not associated with a persistence context. It has no persistent representation in the database and typically no identifier value has been assigned (unless the assigned generator was used).
  * __managed, or persistent__ - the entity has an associated identifier and is associated with a persistence context. It may or may not physically exist in the database yet.
  * __detached__ - the entity has an associated identifier, but is no longer associated with a persistence context (usually because the persistence context was closed or the instance was evicted from the context)
  * __removed__ - the entity has an associated identifier and is associated with a persistence context, however it is scheduled for removal from the database.
* Changing state:
  * Make new entity instance persistent:
    * __persist(Object obj)__ (JPA or native)
    * __save(Object obj)__ (native)
  * Deleting entities:
    * __remove(Object obj)__ (JPA)
    * __delete(Object obj)__ (native)
  * Obtaining an entity reference without initializing its data
    * JPA:
    ```java
    Book book = new Book();
    book.setAuthor( entityManager.getReference( Person.class, personId ) );
    ```
    * Native:
    ```java
    Book book = new Book();
    book.setId( 1L );
    book.setIsbn( "123-456-7890" );
    entityManager.persist( book );
    book.setAuthor( session.load( Person.class, personId ) );
    ```
  * Obtaining an entity with its data initialized
    * JPA: ```Person person = entityManager.find( Person.class, personId );```
    * Native:
      * ```Person person = session.get( Person.class, personId );```
      * ```Person person = session.byId( Person.class ).load( personId );```
  * Obtaining an entity by natural-id
    * get entity reference: ```Book book = session.bySimpleNaturalId( Book.class ).getReference( isbn );```
    * load entity: ```Book book = session.byNaturalId( Book.class ).using( "isbn", isbn ).load( );```
  * Modifying managed/persistent state
    * __any changes to entities in managed/persitent state will be automatically detected and persisted when the persistence context is flushed__
  * Refresh entity state
    * useful when it is known that the database state has changed since the data was loaded
    * ```session.refresh( person );```
* Working with __detached__ data
  * detachment is the process of working with data outside of any persitence context, data becomes detached in number of ways:
    * persistence context is closed
    * clearing the persistence context
    * evicting entity from the persistence context
    * serialization will make the deserialized form to be detached (not the original instance)
  * Reattaching detached data
