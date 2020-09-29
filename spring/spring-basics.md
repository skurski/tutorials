# Spring Basics

### Spring Overview

* bean container (Dependency Injection Container)
* loosely coupled objects - spring is taking care of connecting beans together
* spring is divided into modules - we decide what modules are necessary
for us (spring beans, core, jdbc, data, security, mvc ...) 
* ___ApplicationContext___ - data of entire application

##### Creating beans in Spring
* by adding ___@Component___ annotation above the class, outer annotations:
@Repository, @Service, @Controller, @RestController
* by using XML file
* by using @Configuration class and ___@Bean___ annotation above the method

##### Injection 
* by ___setter___
    * @Autowired above field or setter, we can create setter or not, 
        injection will work the same
    * it is possible to has Null Pointer Exception if object is created 
    (dependency is not injected yet) and injected into another object 
* by ___constructor___
    * @Autowired above constructor or field, dependency put as 
    constructor argument
    * dependency is injected during creation of an object
    * we can see immediately how many dependencies object has
    * easier to create unit tests 

##### Autowiring
* default - by type
* we can use the concrete class name (starting with small letter) - by name 
* we can use ___@Qualifier___ - by specific name
* we can use ___@Primary___ to specify which object to use if more than
one exists

##### Scope
* Creation:
    * ___@Scope___ - to indicate scope of object
    * @Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE) 
    * @Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE, proxyMode = ScopedProxyMode.TARGET_CLASS) -  
    if we use object that is prototype as a dependency in object that
    is singleton we need to use a proxy, without proxy our dependency
    will be singleton type even if set to be prototype

* singleton - default
* prototype - new object every time the object is requested
* request - http request
* session - http session

##### ComponentScan

* @ComponentScan - annotation above @Configuration class which informs
    Spring where to search for the beans, by default it is package where 
    Application class is present
    
##### Lifecycle of a Bean

* @PreConstruct

* @PostConstruct 

* @PreDestroy - called just before bean is moved outside of the context 

##### @SpringBootApplication

* by default:
    * it contains @Configuration annotation
    * it contains @ComponentScan annotation
    * SpringApplication.run(...) returns application context
    * it contains spring core and spring context 
