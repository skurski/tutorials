# JVM Memory Management - Garbage Collector

1. General look:
  - __Young Generation__
    - __Eden__ - all new objects are located hear
    - __Survivor spaces__ - S0/S1 or "From" "To" (objects that survive are promoted to survivor spaces and then to old generation)
  - __Old Generation__
  - __Permament Generation__ - non-heap, for class metadata
    - we need to set the size of PermGen, if it is smaller than the size of our application class metadata then we will get "Out of memory error"
    - PermGen is thied to Old Generation, if either of them is full, both are collected
    - in JDK8 in place of PermGen we have MetaSpace, metaspace uses native memory, no longer problem with not enough memory, we need to set
    the max size that metaspace can use, metaspace is not managed by garbage collector any more, it has it's own MetaSpace VM
    
    
 2. How it works:
  - Different algorithms for young generation and old generation
  - GC types:
    - __Parallel GC__
    - __CMS (Concurrent Mark Sweep) GC__
    - __G1 GC__
    
    
 3. Tunning:
  - Two goals - __Maximum througput__ or __low latency__
  
