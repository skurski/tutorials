# Domain Driven Design

The Domain Model pattern is useful when dealing with complex business logic. A popular design methodology that utilizes the Domain Model pattern is known as DDD. In a nutshell, DDD is a collection of patterns and principles that aid in your efforts to build applications that reflect an understanding of and meet the requirements of your business. Outside of that, it’s a whole new way of thinking about your development methodology. 

__DDD is about modeling the real domain by first fully understanding it and placing all the terminology, rules, and logic into an abstract representation within your code, typically in the form of a domain model.__

DDD is not a framework, but it does have a set of building blocks or concepts that you can incorporate into your solution:
- __Ubiquitous language__: 
    * common for business and developers (using the same language when writting code)
    * we write code using the same verbs (for methods or services) and nouns (for objects) that the domain experts use when talking to us.
- __Bounded contexts__
    - codebase segregation (services that are not to large to work with)
    - database schema segregation
    - team segregation
- __Layers__: 	
    - UI: users can be humans or other applications connecting to our API
    - Application Layer: no business logic, contains objects relevant to Use Cases, services that uses domain services (below layer)
    - Domain Layer: business logic, domain services, entities, events, heart of the system
    - Infrastructure: technical support -> persistence, messaging
- __Continuous integration__
    - with strong bounded context there is tendency for the model to fragment so integration is pretty important
- __Anti Corruption Layer__
    - layer between 2 subsystems, isolates them making them independent of each other
    - subsystem can be replaced and only middleware needs to be updated
    - especially usuful when working with legacy system
    - this layer uses design patterns like: adapter, facade
- __Shared Kernel__: risky, try to avoid or keep it small
    - if we have shared code we need to rebuild and redeploy all services that depends on that code
    - consultation with all other development teams necessary
- other:
    __Entities__, Value objects, Aggregates roots, domain services, application services, repositories, factories
   
    
The bigger the project, the harder it is to maintain it. A way to help on that is breaking the app into smaller areas according to their business rules, known as domains.

Basically, a domain is an area of knowledge, either public, like the rules of chess, or private, like business rules of a nonprofit organization [NPO].

Each subdomain of your domain is known in DDD as bounded contexts, each has its own model of the world built with specialized units of code, known as building blocks. Here I’ll focus on entities and value-objects.

The only way for a bounded context to interact with another (or with the world) is through their main entity, called aggregate root, an object which coordinates the domain's objects. Everything has to be done through the root, otherwise, you risk to have inconsistent values on your bounded context.


# Real world example

In my previous project, we used the domain driven design architecture. The project was divided into
 3 subdomains: billing, accounting and payment. Each subdomain is a bounded context. Bounded context
  communicates with each other through the aggregate root. Aggregate is actually a domain object 
  that contains all the objects present in the domain. I was part of subdomain billing, and our 
  aggregate root was an object called contract. This object was sent to the payment and billing 
  subdomains.


# Usuful links:

https://medium.com/code-thoughts/what-i-understand-about-domain-driven-design-f7fbd00e364f