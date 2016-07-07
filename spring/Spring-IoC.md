# Spring Core

### Spring Context

The __org.springframework.beans__ and __org.springframework.context__ packages are the basis for Spring Framework’s IoC 
container. The __BeanFactory__ interface provides an advanced configuration mechanism capable of managing any type of 
object. __ApplicationContext__ is a sub-interface of BeanFactory. It adds easier integration with Spring’s AOP 
features; message resource handling (for use in internationalization), event publication; and application-layer 
specific contexts such as the WebApplicationContext for use in web applications.


The interface __org.springframework.context.ApplicationContext__ represents the Spring IoC container and is responsible 
for instantiating, configuring, and assembling the beans. The container gets its instructions on what objects to 
instantiate, configure, and assemble by reading configuration metadata. __The configuration metadata is represented in 
XML, Java annotations, or Java code__. It allows you to express the objects that compose your application and the rich 
interdependencies between such objects.


#### Configuration metadata
* Forms of metadata that can be used with Spring container:
    * __Xml-based configuration__ - define beans in XML files
    * __Annotation-based configuration__ - Spring 2.5 introduced support for annotation-based configuration metadata.
    * __Java-based configuration__ - Starting with Spring 3.0, many features provided by the Spring JavaConfig project 
became part of the core Spring Framework. Thus you can define beans external to your application classes by using 
Java rather than XML files. To use these new features, see the @Configuration, @Bean, @Import and @DependsOn 
annotations.
* You can mix xml config with java config by using for example:
    * @Configuration
    * @Import(CDPlayerConfig.class)
    * @ImportResource("classpath:cd-config.xml")


### Bean Scopes
Spring allows to declare the scope of the objects within the container. (Simply we can define how many objects of concreate type will be created). Predefine scopes:

 * to indicate scope we use __@Scope__ annotation
 * __singleton__ (default) — one instance of the bean is created for the entire application, may not be safe for 
 mutable objects
 * __prototype__ — one instance of the bean is created every time the bean is injected into or retrieved from the 
 Spring application context.
 * Also when using web app:
     * __session__ - one instance of the bean is created for each HTTP session.
     * __request__ - one instance of the bean is created for each HTTP request.
     * __globalSession__ - one instance of the bean is created for global HTTP session, typically only valid when used in a portlet context
     * __application__ - one instance of the bean is created for a lifecycle of a ServletContext
     * __websocket__ - one instance of the beank is created for a lifecycle of a WebSocket
        
        
### Lifecycle Mechanisms
__There are three options for controlling bean lifecycle behavior: the InitializingBean and DisposableBean callback 
interfaces; custom init() and destroy() methods; and the @PostConstruct and @PreDestroy annotations.__ You can combine 
these mechanisms to control a given bean.


Multiple lifecycle mechanisms configured for the same bean, with different initialization methods, are called as follows:

1. __Construct methods:__
   * Methods annotated with @PostConstruct
   * afterPropertiesSet() as defined by the InitializingBean callback interface
   * A custom configured init() method
 
2. __Destroy methods__ are called in the same order:
   * Methods annotated with @PreDestroy  
   * destroy() as defined by the DisposableBean callback interface
   * A custom configured destroy() method


#### Spring Annotations:
  * __@Configuration__
    * for creating configuration
  * __@Bean__
    * return an object that is register as bean 
    * bean by default is named like class with first letter in lower case
      * @Bean (name = “someName”)
  * __@Component__ (also @Named) and subtypes:
    * indicate that it is a spring component, a bean will be automatically create inside spring container, it can be than autowire to other bean and retrieved by component scan
    * __@Component(“name”)__ - indicate name of component
    * __@Controller__
    * __@Repository__
    * __@Service__
  * __@Autowired__ (also @Inject)
    * bean is automatically wire to other bean
    * autowire by type is used (only with xml we can define byType or byName)
    * to use autowire by name - use __@Resource__
  * __@Profile (“profileName”)__ 
    * class annotation, class will be loaded only if profile is active, from spring 3.2 it may be also used with @Bean at the level of method
    * all beans without profile will be created regardless which profile is active
    * activation properties
      * spring.profiles.active
      * spring.profiles.default
  * __@Conditional(MagicExistsCondition.class)__
    * from spring 4, you can indicate condition on which the bean will be created
    * used with @Bean
    * parameter is any class that implement Condition interface
  * __@Primary__
    * if you have @Autowired an more than one bean matches, you can mark the bean with this annotation, it will be the first choice
  * __@Qualifier__
    * used with @Autowired (also bean can be mark using @Qualifier)
    * if only with @Autowired than the bean ID will be used
    * if you mark bean with @Qualifier(“someName”), you can than call it with @Autowired @Qualifier(“someName”)
    * you can use multiple qualifiers (on both sides)
  * __@Scope__
    * used with @Component or @Bean
    * @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
  * __@PropertySource__
    * @PropertySource("classpath:/com/soundsystem/app.properties")
    * you can inject references to other objects but you can also inject some string values using properties in spring, you avoid hard coding
    * methods to retrieve properties:
      * String getProperty(String key)
      * String getProperty(String key, String defaultValue)
      * T getProperty(String key, Class<T> type)
      * T getProperty(String key, Class<T> type, T defaultValue)
  * __@Value__
    * @Value("${disc.title}") String title
    * property placeholder => ${disc.title}, property is stored in some.properties file
    * create @Bean => PropertySourcesPlaceholderConfigurer

===

#### Spring Expression Language
  * are framed with #{ ... },
  * #{systemProperties['disc.title']}

===


