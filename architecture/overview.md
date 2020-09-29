# Architecture 

Most common types:

- __monolithic__ 
    * related Anti-pattern: Big Ball of Mud / Spaghetti Architecture
    * nowadays, having a monolithic Architectural Style simply means that all of the application code is deployed and run as a single 
        process on a single node. We assume, it is using modules and components, although it is in fact often not the case. 
        This style, although with very bad reputation, can work very well even for large applications. 
        It only stops being good enough when we need:
        - Independent scalability of different domain components;
        - Different components or modules to be written in different programming languages;
        - Independent deployability, maybe because we have a release rate higher than the deployment pipeline can handle for one code base, 		causing the deployment of a release to be slow because it needs to wait for the deployment of other releases, or even causing the 			deployment queue to grow faster than it is consumed.
     * Drawbacks:
        - entire application needs to use the same technology stack
        - every little change require deployment of entire application (so we gather changes and deploy 
            from time to time)
- __layered__ 
    * related Anti-pattern: Lasagna Architecture
    
    * In a layered system, each layer:
        * Depends on the layers beneath it;
        * Is independent of the layers on top of it, having no knowledge of the layers using it.
    * typical example is a three-tier architecture pattern, also known as n-tier. It is a scalable solution and solves the problem of updating the clients as the user 			interface lives and is compiled on the server, although it is rendered and ran on the client browser.
- __mvc__
- __domain driven design__
- __hexagonal architecture (ports and adapters)__
- __onion__
- __clean__
- __event driven__
- __from cqs to cqrs__
	
	
# Dates

    1950s
    Non-structured Programming
    ~1951 – Assembly
    1960s
      Structured Programming
      Layering: 1 tier with the UI, Business Logic and Data Storage
    ~1958 – Algol
    1970s
      Procedural / Functional Programming
    ~1970 – Pascal
    ~1972 – C
      1979 – Model-View-Controller
    1980s
     Object Oriented Programming (first thoughts were in the late 1960s, though)
      Layering: 2 tier, the 1st tier with the UI, the 2nd tier with Business Logic and Data Storage
    ~1980 – C++
      CORBA – Common Object Request Broker Architecture (though the first stable version was only out in 1991, the first usages were during the 1980s)
    ~1986 – Erlang
    ~1987 – Perl
      1987 – PAC aka Hierarchical Model-View-Controller
      1988 – LSP (~SOLID)
    1990s
      Layering: 3 tier, the 1st tier with the UI, the 2nd tier Business Logic (and the UI presentation logic in case of a browser as client), the 3rd tier with the Data Storage
    ~1991 – Message Bus
    ~1991 – Python
      1992 – Entity-Boundary-Interactor Architecture aka EBC aka EIC
    ~1993 – Ruby
    ~1995 – Delphi, Java, Javascript, PHP
      1996 – Model-View-Presenter
      1996 – OCP, ISP, DIP (~SOLID), REP, CRP, CCP, ADP
      1997 – SDP, SAP
    ~1997 – Aspect Oriented Programming
    ~1997 – Web Services
    ~1997 – ESB – Enterprise Service Bus (although the book that coined the term was published in 2004, the concept was already used before)

    2002 – SRP (~SOLID)
    2003 – Domain-Driven Design
    2005 – Model-View-ViewModel
    2005 – Ports & Adapters Architecture aka Hexagonal Architecture
    2006? – CQRS & ES (Command Query Responsibility Segregation & Event Sourcing)
    2008 – Onion Architecture
    2009 – Microservices (at Netflix)
    2010 – Data-Context-Interaction Architecture 
    2012 – Clean Architecture
    2014 – C4 Model


* Useful links:

    https://herbertograca.com/2017/07/03/the-software-architecture-chronicles/