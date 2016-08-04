# Apache SOLR

Apache SOLR is the pen source enterprise search platform (server) built on Apache Lucene. 

__Apache Lucene__ is:
* Open code API for information retrieval
* Originally developed in Java
* Main purpuse: Indexing and Search
* High performance, scalable
Apache Lucene is responsible for getting content from data source, create indexes based on the fields content and search through this
indexes.

__Apache SOLR__ is fully configuarable through __XML config file__. It uses Lucene and can be __standalone__ or __embedded__.

Comparison between Apache SOLR engine vs Relational database: [Why use SOLR?](http://wiki.apache.org/solr/WhyUseSolr)

We have to SOLR server implementations:

1. __The embedded Solr server__ connects directly to Solr core. We can use this server for development purposes but we must also remember that using it in production environment is not recommended. However, using the embedded Solr server is still a viable option in the development environment.
2. __The HTTP Solr server (standalone)__ connects to an external Solr server by using HTTP. This is the recommended way of using the Solr search server and that is why we should always use it in the production environment.
