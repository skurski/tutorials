# Servlets

#### Container for Servlets (e.g. Apache Tomcat)
Servlets are under the control of another Java application called a Container. Tomcat is an example of such Container. 
##### It works like this:
* Web server application received a request for servlet
* Web server hands the request to the Container in which the servlet is deployed (not to the servlet itself)
* Container gives the HTTP request and HTTP response to the servlet and also call's the servlet methods (like doPost(..) or doGet(..))
##### What are the Container responsibilities? The following:
* Communications support, we don't have to create socket connection
* Lifecycle management, container initialize the servlets, invoking servlets methods, making servlets objects eligable for garbage collection
* Multithreading support, container creates new thread for every servlet request it receives, when the servlet ends running HTTP method, the thread dies
* XML deployment descriptor
* JSP support
