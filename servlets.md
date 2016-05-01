# Servlets

#### Container for Servlets (e.g. Apache Tomcat)
Servlets are under the control of another Java application called a Container. Tomcat is an example of such Container. 

##### How it works?
* Web server application received a request for servlet
* Web server hands the request to the Container in which the servlet is deployed (not to the servlet itself)
* Container gives the HTTP request to the servlet and also call's the servlet methods (like doPost() or doGet() for example)

##### What are the Container responsibilities?
* Communications support, we don't have to create socket connection
* Lifecycle management, container initialize the servlets, invoking servlets methods, making servlets objects eligable for garbage collection
* Multithreading support, container creates new thread for every servlet request it receives, when the servlet ends running HTTP method, the thread dies
* XML deployment descriptor
* JSP support

##### How the container handles the request?
* When the container receives the request for a servlet (not for a static content) it creates 2 objects: HttpServletRequest and HttpServletResponse
* Container finds the servlet based on the URL in the request and creates or allocates a thread for that request (and put the request and response objects into servlet thread)
* The container calls the servlet's service() method and the service calls some http method like doGet() for example
* doGet() generates page and put into response object
* The thread completes, the container convert the response object into a HTTP response, then deletes request and response objects
