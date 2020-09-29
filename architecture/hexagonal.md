# Hexagonal Architecture (Ports and Adapters)

The idea is to think about our application as the central artifact of a system, 
where all input and output reaches/leaves the application through a port that isolates 
the application from external tools, technologies and delivery mechanisms. 
The application should have no knowledge of who/what is sending input or receiving its output. 
This is intended to provide some protection against the evolution of technology and 
business requirements, which can make products obsolete shortly after they are developed,
because of technology/vendor lock-down.

https://herbertograca.com/2017/09/14/ports-adapters-architecture/

    Implementation and technology isolation
    
    Context
    We have an application that uses SOLR as a search engine and we use an open source library to
     connect to it and perform searches.
    
    Traditional approach
    Using a traditional approach, we will use that library classes directly in our code base, 
    as type hints, instances and/or superclasses of our implementations.
    
    Ports and adapters approach
    Using ports and adapters we will create an interface, let’s call it UserSearchInterface, 
    which we will use in our code when needed as a type hint. We will also create the adapter 
    for SOLR, which will implement that interface, let’s name it UserSearchSolrAdapter. 
    This implementation is a wrapper for the SOLR library, so it gets the library injected 
    and uses it to implement the methods specified in the interface.
    
    Problem
    At some point, we want to switch from SOLR to Elasticsearch. Moreover, for the same search, 
    sometimes we want to use SOLR and other times we want to use Elasticsearch, with that decision 
    being made at runtime.
    
    If we used the traditional approach, we will have to search and replace the usage of the 
    SOLR library for the Elasticsearch library. However, that is not a simple search and replace: 
    the libraries have different ways of being used, different methods with different inputs and 
    outputs, so replacing the libraries will not be a trivial task. And using one library instead 
    of another one, at runtime, won’t even be possible.
    
    However, if we used Ports & Adapters, we just need to create a new adapter, let’s name 
    it UserSearchElasticsearchAdapter, and inject it instead of the SOLR adapter, maybe just 
    by changing a config in the DIC. To inject a different implementation at run time, we can 
    use a Factory to decide which adapter to inject.