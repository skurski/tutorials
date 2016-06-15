# Servlets

#### Container for Servlets (e.g. Apache Tomcat, Jetty)
Servlets are under the control of another Java application called a Container. Tomcat is an example of such Container. 
__Tomcat__ is a web container not a full Java EE application server like GlassFish or JBoss, it implements only the servlets and jsp specification. __Java EE server__ includes web container and EJB container (plus many other things). 
Tomcat is often used with Spring as bean container as an alternative for Java EE servers.

##### How it works?
* Web server application (like Apache HTTP Server) receives a request for servlet from client (browser)
* Web server hands the request to the Container in which the servlet is deployed (not to the servlet itself)
* Container gives the HTTP request to the servlet and also call's the servlet methods (like doPost() or doGet() for example)

##### What are the Container responsibilities?
* _Communications support_, we don't have to create socket connection
* _Lifecycle management_, container initializes the servlets, invokes servlets methods, makes servlets objects eligable for garbage collection
* _Multithreading support_, container creates new thread for every servlet request it receives, when the servlet ends running HTTP method, the thread completes
* _XML deployment descriptor_, declarative way to modify application without changing source code
* _JSP support_

##### How the container handles the request?
* When the container receives the request for a servlet (not for a static content) it creates 2 objects: HttpServletRequest and HttpServletResponse
* Container finds the servlet based on the URL in the request and creates or allocates a thread for that request
* The container calls the servlet's service() method, passing the request and response objects as arguments
* The service figured out which servlet method to call based on HTTP method sent by the client
* Executed HTTP method (like doGet() or doPost()) generates page and puts the page into response object
* The service method completes and the tread either dies or returns to container-managed thread pool, the container convert the response object into a HTTP response, the request and response objects fall out of scope (which make them eligable for garbage collection)

#### Servlet mapping
Servlet can have three names:
* file path name like: classes/register/SignUp.class
* deployment name like: RegisterServlet
* public URL name that client can see like: http://somehost.com/register/signup
To register servlet in Container you use deployment descriptor which is the XML file (web.xml):
```
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
	      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	      xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
	      http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	      version="2.5">
    
    <servlet>
        <servlet-name>RegisterServlet</servlet-name>
        <servlet-class>register.SignUp</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>RegisterServlet</servlet-name>
        <url-pattern>/register/signup</url-pattern>
    </servlet-mapping>
</web-app>
```
Deployment descriptor provides the mechanism for declaratively modify your application without touching the source code.

#### Servlet Lifecycle
* Servlet is singleton, there is only one instance of servlet in the container
* __Each client request runs in separate thread__
* Servlet has only one, main state - _initialized_
* servlet methods:
    * __init()__, calls by container after servlet is created but before it can service client requests, the method is called only once, it can be used to initialize database connection for example
    * __service()__, when the client request comes in, the container starts new thread (or allocates one from thread pool) and calls the servlet's service() method 
    * __HTTP method like doGet(), doPost() or other__, is called by the service() method based on HTTP method that comes from the client request
    * __destroy()__, the container ends the servlet's life
* Being a servlet:
    * Ater execution of the constructor servlet is just an object
    * Servlet becomes a servlet after executon of the init() method
    * __ServletConfig__, one per servlet, can be used to pass parameters to servlet, can be used to access ServletContext, parameters are configured in the DD
    * __ServletContext__, one per application, can be used to access application parameters (from DD), attributes (messages that other servlets puts inside ServletContext), server info
* Interfaces and implementations:
    * interfaces: javax.servlet.Servlet, javax.servlet.GenericServlet
    * impl: javax.servlet.http.HttpServlet
    * request interfaces: javax.servlet.ServletRequest, javax.servlet.http.HttpServletRequest
    * response interfaces: javax.servlet.ServletResponse, javax.servlet.http.HttpServletResponse

