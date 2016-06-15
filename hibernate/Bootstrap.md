# Bootstrap

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
