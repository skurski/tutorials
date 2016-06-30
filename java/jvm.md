# JVM

__A Java virtual machine (JVM)__ is an abstract computing machine that enables a computer to run a Java program. There
are three notions of the JVM: __specification, implementation, and instance__. The specification is a document that 
formally describes what is required of a JVM implementation. Having a single specification ensures all 
implementations are interoperable. __A JVM implementation is a computer program that meets the requirements of the 
JVM specification. An instance of a JVM is an implementation running in a process__ that executes a computer program 
compiled into Java bytecode.

__Java Runtime Environment (JRE)__ is a software package that contains what is required to run a Java program. It 
includes a Java Virtual Machine implementation together with an implementation of the Java Class Library. The Oracle 
Corporation distributes a Java Runtime environment with their Java Virtual Machine called HotSpot.

__Java Development Kit (JDK)__ is a superset of a JRE and contains tools for Java programmers, e.g. a javac compiler,
 jconsole.
 
Besides interpreting Java bytecode, most software implementations of the __JVM include a just-in-time (JIT) compiler 
that generates machine code for frequently used methods__. Machine code is the native language of the CPU and can be 
executed much faster than interpreting bytecode.
 
Image taken from https://anturis.com/blog/java-virtual-machine-the-essential-guide/: 
[Hibernate documentation](https://anturis.com/blog/java-virtual-machine-the-essential-guide/jvm-architecture.png)

__JVM includes the following major subsystems:__
* __Class Loader__ - responsible for loading classes into the data areas:
    * HotSpot uses the following hierarchy of class loaders:
    * __Bootstrap Class Loader__ - parent of all class loaders, loads the core Java libraries and is written in 
    native code (C)
    * __Extension Class Loader__ - loads the extension libraries (JRE\lib\ext)
    * __System Class Loader__ - loads application class files that are found in the classpath
          * classpath can be set globally: e.g. set CLASSPATH=classes (this will impact all java application)
          * locally: java -cp classes some.package.Main
              * we can add also jar files: java -cp classes;lib\helper.jar some.package.Main
    * __User-Defined Class Loaders__ - custom class loaders
    * When a class loader receives a request to load a class, it checks the cache to see if the class has already 
    been loaded, then delegates the request to the parent. If the parent fails to load the class, then the child 
    attempts to load the class itself. A child class loader can check the cache of the parent class loader, but the 
    parent cannot see classes loaded by the child. The design is such because a child class loader should not be 
    allowed to load classes that are already loaded by its parent.
    * We can use class loaders for hot deployment (update java code while application is running - add new classes)
* __JVM Memory (5 segments):__
    * __Heap Memory:__
        * Eden (Young generation) - most of the new objects
        * Survivor spaces (Young generation) - objects promoted from eden (that survives some minor GC)
        * Tenured (Old generation) - objects that exists long time
    * Non-Heap Memory:
        * __Method Area__ (part of Permament Generation) - It has been removed in Java 8 from the JVM, and classes are
         now loade as metadata to native memory of the underlying OS.
        * __Native Method Stacks__ (part of Code cache?)
    * __JVM Language Stacks__
    * __PC Registers__ - (program counter) holds the addresses for JVM instructions
* __Execution engine__ - responsible for executing instructions from the data areas:
    * The execution engine executes commands from the bytecode loaded into the data areas one by one. To make the 
    bytecode commands readable to the machine, the execution engine uses two methods:
        * Interpretation - The execution engine changes each command to machine language as it is encountered.
        * Just-in-time (JIT) compilation - If a method is used frequently, the execution engine compiles it to native 
          code and stores it in the cache. After that, all commands associated with this method are executed directly
           without interpretation. 
           Although JIT compilation takes more time than interpretation, it is done only once for a method that might
            get called thousands of times. Running such method as native code saves a lot of execution time compared 
            to interpreting each command one by one every time it is encountered. JIT compilation is not a 
            requirement of the JVM specification, and it is not the only technique that is used to improve JVM 
            performance. JIT Can work as:
            * client system
            * server system
            * These two systems are different binaries. They are essentially two different compilers (JITs)interfacing to
            the same runtime system. The client system is optimal for applications which need fast startup times or small
            footprints, the server system is optimal for applications where the overall performance is most important. In
            general the client system is better suited for interactive applications such as GUIs. Some of the other
            differences include the compilation policy,heap defaults, and inlining policy.
    * Garbage Collector (Memory Management)
    
__Memory (heap) in Java HotSpot virtual machine is divided into 3 generations:__
* __Young generation__
Young generation collections occur relatively frequently and are efficient and fast because the young generation
space is usually small and likely to contain a lot of objects that are no longer referenced.
The young generation consists of an area called __Eden__ plus two smaller __Survivor spaces__.
Most objects are initially allocated in Eden. (A few large objects may be allocated directly in the
old generation.) The survivor spaces hold objects that have survived at least one young generation collection
and have thus been given additional chances to die before being considered _old enough_ to be promoted to the
old generation.
Executing GC on young generation is called __Minor GC__.
* __Old generation__
Objects that survive some number of young generation collections are eventually promoted, or tenured, to the
old generation. This generation is typically larger than the young generation and its occupancy grows more slowly. As
 a result, old generation collections are infrequent, but take significantly longer to complete.
Executing GC on old generation is called __Full GC__ or __Major GC__.
* __Permament generation__ (JVM metadata)
Holds objects that the JVM finds convenient to have the garbage collector manage, such as objects describing classes 
and methods, as well as the classes and methods themselves.

===

### JVM Memory management (GC - garbage collector)

__A garbage collector is responsible for:__
    * allocating memory
    * ensuring that any referenced objects remain in memory, and
    * recovering memory used by objects that are no longer reachable from references in executing code (to which 
    there is no reference in any living thread) Objects that are referenced are said to be live. Objects that are no 
   longer referenced are considered dead and are termed garbage. The process of finding and freeing (also known as 
   reclaiming) the space used by these objects is known as garbage collection

__Types of possible GC:__
* __Serial versus Parallel__
With serial collection, only one thing happens at a time. For example, even when multiple CPUs are
available, only one is utilized to perform the collection. When parallel collection is used, the task of
garbage collection is split into parts and those subparts are executed simultaneously, on different
CPUs. The simultaneous operation enables the collection to be done more quickly, at the expense of
some additional complexity and potential fragmentation.
* __Concurrent versus Stop-the-world__
When stop-the-world garbage collection is performed, execution of the application is completely
suspended during the collection. Alternatively, one or more garbage collection tasks can be executed
concurrently, that is, simultaneously, with the application. Typically, a concurrent garbage collector
does most of its work concurrently, but may also occasionally have to do a few short stop-the-world
pauses. Stop-the-world garbage collection is simpler than concurrent collection, since the heap is
frozen and objects are not changing during the collection. Its disadvantage is that it may be
undesirable for some applications to be paused. Correspondingly, the pause times are shorter when
garbage collection is done concurrently, but the collector must take extra care, as it is operating over
objects that might be updated at the same time by the application. This adds some overhead to
concurrent collectors that affects performance and requires a larger heap size.
* __Compacting versus Non-compacting versus Copying__
After a garbage collector has determined which objects in memory are live and which are garbage, it
can compact the memory, moving all the live objects together and completely reclaiming the
remaining memory. After compaction, it is easy and fast to allocate a new object at the first free
location. A simple pointer can be utilized to keep track of the next location available for object
allocation. In contrast with a compacting collector, a non-compacting collector releases the space
utilized by garbage objects in-place, i.e., it does not move all live objects to create a large reclaimed
region in the same way a compacting collector does. The benefit is faster completion of garbage
collection, but the drawback is potential fragmentation. In general, it is more expensive to allocate
from a heap with in-place deallocation than from a compacted heap. It may be necessary to search the
heap for a contiguous area of memory sufficiently large to accommodate the new object. A third
alternative is a copying collector, which copies (or evacuates) live objects to a different memory area.
The benefit is that the source area can then be considered empty and available for fast and easy
subsequent allocations, but the drawback is the additional time required for copying and the extra
space that may be required.

* How HotSpot GC works:
    * Mark / Sweep / Compact
        * mark - identifies (mark) objects that are still in use (references to this objects exists)
        * sweep - remove unused objects, memory will be fragmentized
        * compact - compact the memory (not every type of GC use this)
* Parameters for JVM:
    * -Xms (-Xms8m) Sets the initial heap size for when the JVM starts.
    * -Xmx (-Xmx32m) Sets the maximum heap size.
    * -Xmn	Sets the size of the Young Generation.
    * -XX:PermSize	Sets the starting size of the Permanent Generation.
    * -XX:MaxPermSize	Sets the maximum size of the Permanent Generation

===

### Questions

__What to Do about OutOfMemoryError?__
One common issue that many developers have to address is that of applications that terminate with
java.lang.OutOfMemoryError. That error is thrown when there is insufficient space to allocate an
object. That is, garbage collection cannot make any further space available to accommodate a new object, and
the heap cannot be further expanded. An OutOfMemoryError does not necessarily imply a memory leak. The
issue might simply be a configuration issue, for example if the specified heap size (or the default size if not
specified) is insufficient for the application.

The first step in diagnosing an OutOfMemoryError is to examine the full error message. In the exception
message, further information is supplied after “java.lang.OutOfMemoryError”. Here are some common
examples of what that additional information may be, what it may mean, and what to do about it:
* __Java heap space__
This indicates that an object could not be allocated in the heap. The issue may be just a configuration
problem. You could get this error, for example, if the maximum heap size specified by the –Xmx
command line option (or selected by default) is insufficient for the application. It could also be an
indication that objects that are no longer needed cannot be garbage collected because the
application is unintentionally holding references to them. The HAT tool can be used to
view all reachable objects and understand which references are keeping each one alive. One other
potential source of this error could be the excessive use of finalizers by the application such that the
thread to invoke the finalizers cannot keep up with the rate of addition of finalizers to the queue. The
jconsole management tool can be used to monitor the number of objects that are pending
finalization.
* __PermGen space__
This indicates that the permanent generation is full. As described earlier, that is the area of the heap
where the JVM stores its metadata. If an application loads a large number of classes, then the
permanent generation may need to be increased. You can do so by specifying the command line
option –XX:MaxPermSize=n, where n specifies the size.
* __Requested array size exceeds VM limit__
This indicates that the application attempted to allocate an array that is larger than the heap size. For
example, if an application tries to allocate an array of 512MB but the maximum heap size is 256MB,
then this error will be thrown. In most cases the problem is likely to be either that the heap size is too
small or that a bug results in the application attempting to create an array whose size is calculated to
be incorrectly huge.
Some of the tools that can be used to diagnose OutOfMemoryError problems are the Heap Analysis Tool (HAT), the 
jconsole management tool, and the jmap tool with the –histo option.
