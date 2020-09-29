# Design Patterns in practice

* Refactoring
    * first methods - removed to many nested code, extract
    * then classes - replace conditions with polymorphism
    * then packages - apply patterns

---

* Factory method 
    * used frequently, especially in DDD for creating entities, encapsulates creation of objects
    * part of domain layer (factories)
    * not creating entities on your own, put responsibility into factory hand
    * creates proper object based on input type (some enum type)

* Adapter
    * main pattern in hexagonal architecture (ports and adapters)
    * ex: service.archive that I worked with, responsible for document archiving, using different kinds of
        archiving providers (dreamrobot, trustweaver)
    * infrastructure package contains PORTS - interfaces that are used inside application for
        connection to external components (archiving providers)
    * concrete implementation of ports are adapters that wraps archiving providers interface and translates to
        application interface (port)
    * allow to simple replace providers, add new ones and removed existing ones
    * allow to choose provider during the runtime
    
* Facade
    * simplify the interface, hide internal logic, acts a little bit like service that uses many classes inside
    
* Strategy
    * encapsulated logic that be choose during runtime
    * ex: processing contracts after creation
    * different strategies for contract: singlePayment, Recurring, Subscription
    * strategy registry that keeps all strategies
    * iterate over strategies and apply one that is applicable for given contract
    
* Chain of responsibility
    * great pattern to validate or apply multiple operation to some input / request
    * put the request into first validator and then internally this validator will pass the request down to
        the next validator, this way all validators will be applied to request
    * encapsulates the logic for every validator
    * flat structure, every validator with logic in separate class
    
* Template method
    * can be useful when using inheritance
    * declare abstract method inside base abstract class, this way every subclass will execute common part of code 
        (common functionality) but the rest of implementation will be in subclass hand

* Decorator
    * takes an object and add functionality to it (decorates it)
    * can be used instead of inheritance - add functionality to class without modifying it
    * need to create a stack, from top to bottom like IO package in java
    * then we can decorate objects in a way that we want but we need to know the order
    * ex: notification: notifier class and then SmsNotifier, EmailNotifier, FacebookNotifier, SlackNotifier
    * ex: datasource: add decorators for encryption, 
    * can be applied during runtime
    * https://refactoring.guru/design-patterns/decorator
    
* Proxy
    * encapsulates object
    * for security reason or other
    * controlling access to the target object