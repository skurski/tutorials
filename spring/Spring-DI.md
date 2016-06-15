# Spring Core

#### Configuration
* Spring Configuration (automatic configuration + java config is preffered)
  * xml based (explicitly)
  * java based (explicitly)
  * automatic wiring
* You can mix xml config with java config by using for example:
  * @Configuration
  * @Import(CDPlayerConfig.class)
  * @ImportResource("classpath:cd-config.xml")

===

#### Scopes: 
  * declared using @Scope annotation
  * __Singleton__ (default) — one instance of the bean is created for the entire application, may not be safe for mutable objects
  * __Prototype__ — one instance of the bean is created every time the bean is injected into or retrieved from the Spring application context.
  * Also when using web app:
    * __Session__ - one instance of the bean is created for each session.
    * __Request__ - one instance of the bean is created for each request.

===

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


