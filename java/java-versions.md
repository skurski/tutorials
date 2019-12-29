# Differences between Java versions

## Java 8

##### Interfaces unlocking
- concrete methods: static and default
- methods are public, fields are public static final

##### Lambda expressions
- functional interfaces
- method references

##### Streams
- sequence of elements
- no storage
- intermediate operations and terminal operations
- specialized streams: IntStream, DoubleStream
- infinite streams

##### New Date Api
- LocalDate, LocalDateTime

##### Scripting
- Nashorn engine

##### Changes in JVM architecture
- Parallel GC becomes default type of GC implementation
- Metaspace instead of Permanent Generation space

## Java 9

##### Project Jigsaw
- introduced modularity to monolithic Java SE ecosystem
- module is a self-describing collection of code, data and resources. 
It consists of module-info.java file and one or more packages. 
Modules offer stronger encapsulation than jars.

##### JShell
- Java 9 finally provided a Read-Eval-Print Loop (REPL) tool. 
This is an interactive programming tool which is continually reading 
user input, evaluating it and printing the output value or 
a description of the state change the input caused.

##### G1 GC
- G1 becomes default GC

##### Interfaces
- private methods in interfaces

##### Optional class
- Optional.ifPresentOrElse()
- Optional.or()
- Optional.stream()

##### Immutable Collection Factory
- new static methods in List, Set, Map for creating unmodifiable collections

##### CompletableFuture improvements

##### Enhancements in @Deprecated

## Java 10

##### Keyword “var” (Local-Variable Type Inference)

- Probably the most recognisable feature of this release 
(at least from developer’s point of view). The var keyword that has
 been introduced can replace strict variable type declaration if the 
 type may be easily inferred by the compiler. Therefore we can type less, 
 don’t need to duplicate type information and it just makes the code look nicer.
We can use var in the context of local variables (instantly initialized), 
for loops and try-with-resources blocks.
We can’t use var e.g. when declaring class fields or method parameters.

##### Parallel Full GC for G1
- As we mentioned before, G1 is the default GC starting from Java 9. 
It was mainly designed to avoid full collections and preventing 
the “stop the world” event. It increased the performance significantly, 
however, there was still probability that if concurrent collections 
couldn’t reclaim memory quickly enough, then classic full GC would be used as a fallback.

Therefore in Java 10 they took care of this particular situation too 
and improved the algorithm to parallelize the full GC.

##### Additions to JDK
- A few small additions came also to the standard class library. 
For instance, there is a new overloaded version of Optional.orElseThrow() 
which doesn’t take any parameters and throws NoSuchElementException by default.

Additionally, API for creating unmodifiable collections has been 
improved by adding List.copyOf(), Set.copyOf(), Map.copyOf() methods, 
which are factory methods for creating unmodifiable instances from 
existing collections. Also, Collectors class has some new methods like 
toUnmodifiableList, toUnmodifiableSet, toUnmodifiableMap.

## Java 11

##### Local-Variable Syntax for Lambda Parameters
- ```IntFunction<Integer> doubleIt2 = (var x) -> x * 2;```
- ```IntFunction<Integer> doubleIt2 = (@Valid final var x) -> x * 2;```

##### Launch Single-File Source-Code Programs
- ```java SimpleProgram.java```