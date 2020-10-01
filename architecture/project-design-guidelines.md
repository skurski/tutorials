    1. Microservice design from scratch:
       
       Typical flow of REST app: 
       
       Request -> validation -> dto -> processing -> entity -> persistance -> database -> database constraints -> response 
       
        1. API desing - CRUD requests
            1. Requests - hide internal implementation details from client
            2. DTOs – for internal processing
            3. Responses – for returning (it might be DTO – open for discussion)
            4. Entities – only for persistence purposes
                1. Entity can have method for transforming to DTO and the same in DTO
            5. API structure uniform (consistent) across service:
                1. sensible HTTP responses (no 500)
                2. pagination / sorting / conditions in GET operation
                3. restriction for UPDATE / DELETE operations
                4. design model types – figure out how they interact with each other
                5. every type has it’s own creation request and validation rule set
                6. HATEOS for REST
        2. Validation
            1. Requests - not allowing to put garbage into the system
                1. not null
                2. not empty
                3. validators for correct values 
                    1. numbers
                    2. enums
                    3. dates
                    4. currency
                    5. custom values
                    6. monetary values
                    7. and many more...
        3. Processing
            1. strategies
            2. chains of strategies that we can apply to given object types
            3. multiple strategies can be applied to multiple types, each type has a list of allowed processing strategies
        4. Persistence
            1. Database choice – depend on the job
                1. Document database – eventual consistency, for configuration, documents
                2. SQL database for ACID transactions
                3. Graph database for related objects stick close to each other
        5. Database
            1. constraints
            2. uuid vs id
        6. Integration with other services - Enterprise Integration Patterns
            1. book to read and familiarize yourself with it
        7. Technologies choice for the job
            1. use specialize libraries for job instead of creating your own
        8. Security
            1. authentication
            2. authorization
        9. Libraries, examples:
            1. mapping - mapper struct
            2. country / language code -> neovisionaries
            3. cucumber
        10. REST
            1. Principles
                1. https://en.wikipedia.org/wiki/Representational_state_transfer
                2. HATEOAS - Hypermedia as the Engine of Application State (HATEOAS) is a component of the REST application architecture that distinguishes it from other network application architectures. 
                   With HATEOAS, a client interacts with a network application whose application servers provide information dynamically through hypermedia. A REST client needs little to no prior knowledge about how to interact with an application or server beyond a generic understanding of hypermedia.
            2. Spring Data Rest
                1. HATEOAS
                2. HAL – hypertext application language
                    1. HAL is a simple format that gives a consistent and easy way
                     to hyperlink between resources in your API.


Api design

design types, processing strategy

Db design (constraints, uuid)


1. Entity only used for persistance, not for processing or anything else -> dto is used there, if something goes wrong and entity is attached to session it can persist to db 

2. Entity can have method for transforming to DTO and the same in DTO


Junit 5 -> package org.junit.jupiter.api:
	BeforeEach, ExtendWith, ...

Assertions -> AssertJ:
```java
    assertThat(object).isEqualTo(...)

        //when
    Throwable thrown = catchThrowable(() -> templateResolver.resolveName(templateConfig));

        //then
    assertThat(thrown).isInstanceOf(IllegalArgumentException.class);
```

see sonar for issues, like exception handling. RuntimeException can be turned into custom response code exceptions

Exception handling -> custom response code @ResponseStatus or ResponseStatusException

```java
public class TransformationException extends ResponseStatusException {

    private TransformationException(String message) {
        super(HttpStatus.UNPROCESSABLE_ENTITY, message);
    }

    public static TransformationException xmlTransformationError() {
        return new TransformationException("XSL transformation error - xml data cannot be correctly applied to xsl template.");
    }

    public static TransformationException contentTypeTransformationError(ContentType contentType) {
        String exceptionMsg = String.format("XSL to %s transformation error.", ContentType.valueOf(contentType));
        return new TransformationException(exceptionMsg);
    }
}
```
