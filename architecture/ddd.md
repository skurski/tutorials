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

# Examples

Your new architecture is also not derive from Domain Driven Design. Of course in context of tactical patterns (as strategy patterns scope go beyond the code).

In Domain Driven Design you have plenty useful building blocks which can help you with managing accidental complexity of your system. But before you start tot apply them you need to answer very important question

Do I really need DDD building blocks in my system/module?
Answer should be yes if your system has rich business logic not just simple CRUD (it is of course simplification, you have a lot of articles about it). Note: I am referring to tactical part of DDD. I really believe in using DDD strategic patterns event for CRUD-like applications (e.g. ubiquitous language).

OK, as the topic is very broad I will give you few simple steps you can weave into your system (it is more less concept of hexagonal architecture).

1) Split your application into modules. They would be more less you Bounded Contexts. How to do it in Java? Use packages - they are really powerful.

So you will have e.g com.mycompany.mysystem.order, com.mycompany.mysystem.invoice

2) Your Bounded Context will be built upon some Aggregate. Aggregate is everything which should be processed in one transaction. Root of your aggregate can be represented as your JPA Entity.

```java
@Entity
class Order {
@Id
String id;
List<Product> products;

void addProductToOrder(...) {
... some business logic
}

Money calculateDiscount(...) {
... some business logic
}

```
Your Entities should be Rich Entities - contains behavior, not just preserve structure. You can also use concepts like immutable Value Objects.

3) Create repository for your aggregate but only for your business part - so that you can save your aggregate and retrieve it. You can use e.g. Spring Data.

4) Create Facade to collect methods needed to perform some action. You can keep validation there but remember that behavior itself should be kept inside aggregate.

5) You can use also other patterns like Factory, Strategy, Observer, etc. All OO patterns fit well into DDD.

6) Keep all these classes in domain package, e.g.

com.mycompany.mysystem.order.domain
- Order.java
- OrderRepository.java
- OrderFacade.java
- OrderFactory.java
Only OrderFacade should be public. Other classes should have package scope.

7) Now, you can create another subpackage - infrastructure. You will keep Spring configuration there, Rest Controller, etc. Everything not related to the domain.

com.mycompany.mysystem.order.domain
- Order.java
- OrderRepository.java
- OrderFacade.java
- OrderFactory.java

com.mycompany.mysystem.order.infrastructure
- OrderCommandController.java
- OrderConfiguration.java

8) There is one very powerful concept used often with DDD. It is called CQRS. You can read about it, there are many sources. I will give you one tip how you can add it to above example.

Do you have situations when you need to retrieve something from database on million ways? Yeah, everybody does.

So according to CQRS you can split your commands/business operations and queries. Following our example, create new subpackage

com.mycompany.mysystem.order.query

Inside you can create Entity called OrderQuery and point it to the same table in database (or use projection in more advanced cases). You can now create new repository (Spring Data) and use it with OrderQuery. In infrastructure subpackage add new controller OrderQueryController.

These steps are obviously just a skeleton and contains many simplifications. It should be enough to start and guide you in good direction.