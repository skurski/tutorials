# Reactor 

* implements Reactive Streams interface
* is used by Spring WebFlux

### Two main types:
 * __Publisher__:
    * send notifications
 * __Subscriber__:
    * receives notifications
    * interface with three methods:
        * onNext(T)
        * onComplete() - terminal
        * onError(Throwable) - terminal, optional but recommended 
    * needs to be attached to publisher:
        * flux.subscribe(subscriber) 
       
### Cold Flux
    * won't start pumping until there's a Subscriber attached
    * will emit the whole data set to each new Subscriber 
    
### Hot Flux
    * generally reads live data, eg. data feeds
    * begins pumping on connection (possibly on creation)
    * each Subscriber only gets the newest data from the point where they attach

