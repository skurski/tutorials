# GC Roots

Every object tree must have one or more root objects. As long as the 
application can reach those roots, the whole tree is reachable. 
But when are those root objects considered reachable? Special objects 
called garbage-collection roots are always reachable and so is any 
object that has a garbage-collection root at its own root.

![](gc-roots.png)

### GC roots:

* Local variables in the main method
* The main thread
* Static variables of the main class

### Useful links

* [https://www.dynatrace.com/resources/ebooks/javabook/how-garbage-collection-works/](https://www.dynatrace.com/resources/ebooks/javabook/how-garbage-collection-works/)
