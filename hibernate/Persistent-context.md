# Persistence Contexts

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
  * detachment is the process of working with data outside of any persitence context, data becomes detached when:
    * persistence context is closed
    * clearing the persistence context
    * evicting entity from the persistence context
    * serialization will make the deserialized form to be detached (not the original instance)
  * Reattaching detached data
