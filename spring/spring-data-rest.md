# SPRING DATA REST


### Setting the RepositoryDetectionStrategy

Spring Data REST uses a RepositoryDetectionStrategy to determine whether a repository is exported as a REST resource. The RepositoryDiscoveryStrategies enumeration includes the following values:

DEFAULT

Exposes all public repository interfaces but considers the exported flag of @(Repository)RestResource.

ALL

Exposes all repositories independently of type visibility and annotations.

ANNOTATION

Only repositories annotated with @(Repository)RestResource are exposed, unless their exported flag is set to false.

VISIBILITY

Only public repositories annotated are exposed.

### Resource Discoverability

A core principle of HATEOAS is that resources should be discoverable through the publication of links that point to the available resources. There are a few competing de-facto standards of how to represent links in JSON. By default, Spring Data REST uses HAL to render responses. HAL defines the links to be contained in a property of the returned document.

### Custom Handler Mapping - RepositoryRestHandlerMapping

In order to keep paths that are meant to be handled by your application separate from those handled by Spring Data REST

### Overriding Spring Data Rest Response Handler -> @RepositoryRestController

Controllers annotated with @RepositoryRestController are served from the API base path defined in RepositoryRestConfiguration.setBasePath, which is used by all other RESTful endpoints (for example, /api).

### Customizing JSON

Spring Data REST provides integration with Spring HATEOAS and provides an extension hook that lets you alter the representation of resources that go out to the client.

### The ResourceProcessor Interface

Spring HATEOAS defines a ResourceProcessor<> interface for processing entities. All beans of type ResourceProcessor<Resource<T>> are automatically picked up by the Spring Data REST exporter and triggered when serializing an entity of type T.

### Customizing the Representation - ConversionService

If your project needs to have output in a different format than default, however, you can completely replace the default outgoing JSON representation with your own. If you register your own ConversionService in the ApplicationContext and register your own Converter<Entity, Resource>, you can return a Resource implementation of your choosing.

### Adding Custom Serializers and Deserializers to Jackson’s ObjectMapper

To accommodate the largest percentage of the use cases, Spring Data REST tries to render your object graph correctly. It tries to serialize unmanaged beans as normal POJOs, and tries to create links to managed beans where necessary. However, if your domain model does not easily lend itself to reading or writing plain JSON, you may want to configure Jackson’s ObjectMapper with your own custom type mappings and (de)serializers.

### Abstract Class Registration - for use abstract classes (or interfaces) for domain model

### @CrossOrigin 

Spring Data REST, as of 2.6, supports Cross-Origin Resource Sharing (CORS) through Spring’s CORS support.