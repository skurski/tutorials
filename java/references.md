### References types

Going from strongest to weakest, the different levels of reachability reflect the life cycle of an object. They are 
operationally defined as follows:

1. __Strong Reference:__ Created with __new__ keyword, such object is eligable for garbage collection only if there
 are no reference to that object in any living thread, until that object will exist in memory:
```java StringBuilder builder = new StringBuilder();```
2. __Soft Reference:__ The object will be eligable for garbage collection when the GC will needs more memory (e.g. to
 prevent OutOfMemoryError)
3. __Weak Reference:__ We use weak reference if we don't want to influence the referenced object's lifecycle, in case
 the only reference reachable for the object in memory is the weak reference, then the GC is allowed to garbage 
 collect the object. (In other words: when an object in memory is reachable only by Weak Reference it becomes 
 automatically eligible for GC).
4. __Phantom Reference:__ The get method of this reference always return null. Phantom reference is the __new 
finalize__, allow to determine exactly when an object was removed from memory. (For example if the object is really 
huge we can wait until the object is garbage collected and then create new instance).
