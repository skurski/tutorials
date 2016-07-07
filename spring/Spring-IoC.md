# Spring Core

## Spring Context

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

__ApplicationContext implementations:__
   * ClassPathXmlApplicationContext
   * FileSystemXmlApplicationContext
   * AnnotationConfigApplicationContext

__All Spring beans are managed inside a container, called bean factory or application context. Each application has an entry point to that context. The entry point for Spring MVC is the DispatcherServlet.__
___

## Configuration metadata
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

___

## Bean definition with XML:
   1. class
   2. id / name
   3. scope
   4. constructor arguments
   5. properties
   6. autowiring mode
   7. lazy-initialization mode
   8. initialization method
   9. desctruction method
   
___

## Bean instantiation:
   1. __with a constructor:__
      * With XML-based configuration metadata we can specify our bean class as follows:
      ```xml
      <bean id="exampleBean" class="examples.ExampleBean"/>
      
      <bean name="anotherExample" class="examples.ExampleBeanTwo"/>
      ```
   2. __with a static factory method:__
      * When defining a bean that we create with a static factory method, we use the __class__ attribute to specify the class containing the __static factory method__ and an attribute named __factory-method__ to specify the name of the factory method itself. Such object is treated as if it had been created through a constructor.   
      ```xml 
      <bean id="clientService" class="examples.ClientService" factory-method="createInstance"/>
      ```
      ```java
      public class ClientService {
         private static ClientService clientService = new ClientService();
      
         private ClientService() {}
      
         public static ClientService createInstance() {
            return clientService;
         }
      }
      ```
   3. __with an instance factory method:__
      * Instantiation with an instance factory method invokes a non-static method of an existing bean from the container to create a new bean. To use this mechanism, __leave the class attribute empty__, and in the __factory-bean attribute, specify the name of a bean in the current (or parent/ancestor) container that contains the instance method__ that is to be invoked to create the object. __Set the name of the factory method itself with the factory-method attribute.__
      ```xml
      <!-- the factory bean, which contains a method called createInstance() -->
      <bean id="serviceLocator" class="examples.DefaultServiceLocator">
         <!-- inject any dependencies required by this locator bean -->
      </bean>
      
      <!-- the bean to be created via the factory bean -->
      <bean id="clientService" factory-bean="serviceLocator" factory-method="createClientServiceInstance"/>
      ```
      ```java
      public class DefaultServiceLocator {
         private static ClientService clientService = new ClientServiceImpl();
         
         private DefaultServiceLocator() {}
         
         public ClientService createClientServiceInstance() {
            return clientService;
         }
      }
      ```
      
___

## Dependency Injection
1. __Constructor-based:__
   ```xml
   <bean id="exampleBean" class="examples.ExampleBean">
      <!-- constructor injection using the nested ref element -->
      <constructor-arg>
         <ref bean="anotherExampleBean"/>
      </constructor-arg>
      
      <!-- constructor injection using the neater ref attribute -->
      <constructor-arg ref="yetAnotherBean"/>
      
      <constructor-arg type="int" value="1"/>
   </bean>
   
   <bean id="anotherExampleBean" class="examples.AnotherBean"/>
   <bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
   ```
   ```java
   public class ExampleBean {
   
      private AnotherBean beanOne;
      private YetAnotherBean beanTwo;
      private int i;
      
      public ExampleBean(
         AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {
         this.beanOne = anotherBean;
         this.beanTwo = yetAnotherBean;
         this.i = i;
      }
   }
   ```
2. __Setter-based:__
   ```xml
   <bean id="exampleBean" class="examples.ExampleBean">
      <!-- setter injection using the nested ref element -->
      <property name="beanOne">
         <ref bean="anotherExampleBean"/>
      </property>
      
      <!-- setter injection using the neater ref attribute -->
      <property name="beanTwo" ref="yetAnotherBean"/>
      <property name="integerProperty" value="1"/>
   </bean>
   
   <bean id="anotherExampleBean" class="examples.AnotherBean"/>
   <bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
   ```
   ```java
   public class ExampleBean {
   
      private AnotherBean beanOne;
      private YetAnotherBean beanTwo;
      private int i;
      
      public void setBeanOne(AnotherBean beanOne) {
         this.beanOne = beanOne;
      }
      
      public void setBeanTwo(YetAnotherBean beanTwo) {
         this.beanTwo = beanTwo;
      }
      
      public void setIntegerProperty(int i) {
         this.i = i;
      }
   }
   ```
   
* __Value attribute:__ - The value attribute of the <property/> element specifies a property or constructor argument as a human-readable string representation. Spring’s conversion service is used to convert these values from a String to the actual type of the property or argument.
* __Inner beans:__ - A <bean/> element inside the <property/> or <constructor-arg/> elements defines a so-called inner bean. An inner bean definition does not require a defined id or name; if specified, the container does not use such a value as an identifier. The container also ignores the scope flag on creation: Inner beans are always anonymous and they are always created with the outer bean. It is not possible to inject inner beans into collaborating beans other than into the enclosing bean or to access them independently.

   ```xml
   <bean id="outer" class="...">
      <!-- instead of using a reference to a target bean, simply define the target bean inline -->
      <property name="target">
         <bean class="com.example.Person"> <!-- this is the inner bean -->
            <property name="name" value="Fiona Apple"/>
            <property name="age" value="25"/>
         </bean>
      </property>
   </bean>
   ```
* __Collections:__ - In the <list/>, <set/>, <map/>, and <props/> elements, you set the properties and arguments of the Java Collection types List, Set, Map, and Properties, respectively. 

   ```xml
   <bean id="moreComplexObject" class="example.ComplexObject">
      <!-- results in a setAdminEmails(java.util.Properties) call -->
      <property name="adminEmails">
         <props>
            <prop key="administrator">administrator@example.org</prop>
            <prop key="support">support@example.org</prop>
            <prop key="development">development@example.org</prop>
         </props>
      </property>
      <!-- results in a setSomeList(java.util.List) call -->
      <property name="someList">
         <list>
            <value>a list element followed by a reference</value>
            <ref bean="myDataSource" />
         </list>
      </property>
      <!-- results in a setSomeMap(java.util.Map) call -->
      <property name="someMap">
         <map>
            <entry key="an entry" value="just some string"/>
            <entry key ="a ref" value-ref="myDataSource"/>
         </map>
      </property>
      <!-- results in a setSomeSet(java.util.Set) call -->
      <property name="someSet">
         <set>
            <value>just some string</value>
            <ref bean="myDataSource" />
         </set>
      </property>
   </bean>
   ```
* __Autowiring:__ - The Spring container can autowire relationships between collaborating beans. You can allow Spring to resolve collaborators (other beans) automatically for your bean by inspecting the contents of the ApplicationContext. When using XML-based configuration metadata, you specify autowire mode for a bean definition with the autowire attribute of the <bean/> element. The autowiring functionality has four modes. You specify autowiring per bean and thus can choose which ones to autowire.
   * __no__ - (default) no autowiring 
   * __byName__ - autowiring by property name
   * __byType__ - autowiring by bean type, if more than one exists, a fatal exception is thrown, if there are no matching beans, nothing happens - the property is not set.
   * __constructor__ - similar to byType, but applies to constructor arguments, if there is not exactly one bean a fatal error is raised

___

## Annotation-based configuration
* __Annotation injection is performed before XML injection__, thus the latter configuration will override the former for properties wired through both approaches.
* we need to add the following tag in an XML configuration:

   ```xml
   <context:annotation-config/>
   ```
   
   The use of <context:component-scan> implicitly enables the functionality of <context:annotation-config>. 
* __@Autowired__ or __@Inject__ - autowiring by type, if we add __@Qualifier__ Spring will match by name
* __@Resource__ - autowiring by name
* __@Required__ - bean property must be populated at configuration time, if not the exception is thrown
* __@Primary__ -  indicates that a particular bean should be given preference when multiple beans are candidates to be autowired to a single-valued dependency
* __@Qualifier__ - match bean by name

___

## Java-based configuration


___

## Bean Scopes
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
  * optionally we can create __custom scope__
___   
        
## Lifecycle Mechanisms
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

___

## Spring Annotations:
   * __@Configuration__
      * for creating configuration
   * __@Bean__
      * return an object that is register as bean 
      * bean by default is named like class with first letter in lower case
         * @Bean (name = “someName”)
   * __@Component__ (also @Named equivalent) and subtypes:
      * indicate that it is a spring component, a bean will be automatically create inside spring container, it can be than autowire to other bean and retrieved by component scan
      * __@Component(“name”)__ - indicate name of component
      * __@Controller__
      * __@Repository__
      * __@Service__
   * __@Autowired__ (also @Inject)
      * bean is automatically wire to other bean
      * autowire by type is used (only with xml we can define byType or byName)
      * to use autowire by name - use __@Resource__
   * __@Lazy__
      * lazy injection of bean
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
   * __@ComponentScan(basePackages="org.example")__
      * autodetect classes and register the corresponding beans
      * The use of <context:component-scan> implicitly enables the functionality of <context:annotation-config>. 

___

## Spring Expression Language
  * are framed with #{ ... },
  * #{systemProperties['disc.title']}
