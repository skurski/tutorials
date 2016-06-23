# Spring MVC

#### How DispatcherServlet works?
![DispatcherServlet](http://www.mkyong.com/wp-content/uploads/2010/07/spring-mvc-concepts-2.jpg)

  1. __Choose SpringMVC controller__
    * The DispatcherServlet’s job is to send the request on to a Spring MVC controller. 
      A controller is a Spring component that processes the request. But a typical application may have several controllers, 
      and DispatcherServlet needs some help deciding which controller to send the request to. So the DispatcherServlet consults one or
      more handler mappings to figure out where the request’s next stop will be. The handler mapping pays particular attention to the 
      URL carried by the request when making its decision.
  2. __Send request to the choosen controller__
    * Once an appropriate controller has been chosen, DispatcherServlet sends the request on its merry way to the chosen controller. 
      At the controller, the request drops off its payload (the information submitted by the user) and patiently waits while
      the controller processes that information. (Actually, a well-designed controller performs little or no processing itself 
      and instead delegates responsibility for the business logic to one or more service objects.)
      The logic performed by a controller often results in some information that needs to be carried back to the user 
      and displayed in the browser. This information is referred to as the model. But sending raw information back to the user
      isn’t sufficient - it needs to be formatted in a user-friendly format, typically HTML. For that, the information needs 
      to be given to a view, typically a JavaServer Page (JSP).
  3. __Identify view that should render the output__ 
    * One of the last things a controller does is package up the model data and identify the name of a view that should render 
      the output. It then sends the request, along with the model and view name, back to the DispatcherServlet.
      So that the controller doesn’t get coupled to a particular view, the view name passed back to DispatcherServlet doesn’t 
      directly identify a specific JSP. It doesn’t even necessarily suggest that the view is a JSP. Instead, it only carries
      a logical name that will be used to look up the actual view that will produce the result. The DispatcherServlet consults 
      a view resolver to map the logical view name to a specific view implementation, which may or may not be a JSP.
    * Now that DispatcherServlet knows which view will render the result, the request’s job is almost over. Its final stop is 
      at the view implementation, typically a JSP, where it delivers the model data. The request’s job is finally done. The view will
      use the model data to render output that will be carried back to the client by the (notso-hardworking) response object.
      
===

#### DispatcherServlet
  * can be configured in web.xml (old approach) or 
  * in java config class (in spring 3 and servlet 3) that extends AbstractAnnotationConfigDispatcherServletInitializer
  * __AbstractAnnotationConfigDispatcherServletInitializer__
    * newer approach than web.xml
    * you can indicate root application context (ContextLoaderListener) and web application context (DispatcherSerlvet)
    * no need to use web.xml any more
    * web config created with @EnableWebMvc (and @Configuration)
  
===
    
#### SpringMVC annotations
  * __@EnableWebMvc__
    * enable Spring MVC
    * you have to indicate @ComponentScan(...) for searching for web components (controllers)
    * you should configure viewResolver
    * you should configure how to handle static resources
  * __@ComponentScan(...)__
  * __@Controller__
  * __@Repository__
  * __@Service__
  * __@RequestMapping__

===

#### REST SpringMVC annotations:
  * REST - Rest resources (JSON, XML) 
  * __@ResponseBody__
    * before the method indicate that it returns resource
    * used with @RequestMapping(method=RequestMethod.GET, produces="application/json")
    * if Jackson Json library available in the classpath it will return JSON object
  * __@RequestBody__
    * before the parameter in method
    * used with @RequestMapping(method=RequestMethod.POST, consumes="application/json")
    * it will get the resource
  * __@RestController__
    * before class indicate that all methods in the class are RESTful
    * no need to use @ResponseBody 

===

#### Spring with JPA
* In order to have fully functional transaction with JPA you need to :
  * Use __@EnableTransactionManagement__ on your configuration file
  * Declare an EntityManagerFactoryBean
  * Declare a JpaTransactionManager (the bean must be named transactionManager) created with your previously declared EntityManagerFactoryBean
  * Inject (with @PersistenceContext or @Autowired) an EntityManager in your @Repository (which must be a spring bean)
  * Call your repository from a @Service and use a public method anotated with @Transactional

===
