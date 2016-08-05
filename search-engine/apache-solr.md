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

### Running Apache SOLR
1. Start SOLR application - ``` java -jar start.jar```
2. Post data to SOLR - ```java -jar post.jar *.xml```

Upload (post) mechanisms for Apache SOLR Server:

* __DataImportHandler__
* Add XML, JSON, CSV documents
* SolrJ or SolrNet
* Multiple tools that post documents to Solr's index

__Query__: [http://localhost:8983/solr/select?g=video](http://localhost:8983/solr/select?g=video), consists:

1. host name and port number (localhost and 8983)
2. application name (solr)
3. request handler for queries (select)
4. query itself (g=video)

Apache SOLR allows multiple __cores__. __Every Core__ has its own schema and configuration.

### Schemas, documents and indexing
* __Document__
  * basic unit of information, made up of fields
* __Field__ 
  * specific piece of information within a document,
  * contains different types of data,
  * each field can have many different attributes,
  * e.g: ``` <field name="description" type="text_general" indexed="true" stored="true">``` (if the field is set to indexed="true" but stored="false" we can search (and sort) by indexed field but we are not able to retrieve information about indexed field)
  * [field properties by use case](https://cwiki.apache.org/confluence/display/solr/Field+Properties+by+Use+Case)
