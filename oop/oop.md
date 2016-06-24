### OOP Principles

##### SOLID
* __Single Responsibility Principle__ - a class should have only a single responsibility
* __Open Close Principle__ - open for extension, close for modification
* __Liskov Substitution Principle__ - objects in a program should be replaceable with instances of their subtypes
without altering the correctness of that program
* __Interface Segregation Principle__ - many client specific interfaces are better than one general-purpose
interface, class should only implement methods that are needed
* __Dependency Inversion Principle__ - depend upon abstractions, do not depend upon concretions

##### DRY - Don't Repeat Yourself

##### KISS - Keep It Simple, Stupid

##### YAGNI - You Aren't Gonna Need It

#### Method Binding
* __Static (Early) Binding__ - occur during Compile time, methods that are bounded using static binding are private,
final, static
* __Dynamic (Late) Binding__ - occur during Runtime, virtual methods are lookup by name at runtime, compiler does not
 have enough information to bind method

#### Polymorphism
* __Static__ - at compile time
* __Dynamic__ - at run time

#### Composition over Inheritance
* __Composition__ implies strong ownership, one objects owns (manage the lifecycle) of another object, when parent is
 destroyed, all children are destroyed as well.
* __Aggregation__ - also one objects owns another objects but not such strong relationship as composition, children are
independent, can be used by another objects and can outlive parent.
* Examples of composition in design patterns:
    * __Decorator__
    * __Strategy__ - e.g. Comparator, great example of flexibility in design and harmony with the Open/Close
    principle
* Examples of inheritance in design patterns:
    * __Template method__

#### Code reuse
* __Inheritance__
    * one of disadvantage is that is breaks encapsulation, if sub class depends on super class behaviour for its
    operation when behavior of super class changes, functionality in sub class may get broken
    * java doesn't support multiple inheritance
    * easier to test, composed class can be represented by mock object
    * not so high coupling, more flexibility
* __Composition__

#### Inversion Of Control (IoC)
Programming technique in which object coupling is bound at runtime by an assembler object and is typically not known
at compile time.
__Dependency Injection__ - design pattern that implements inversion of control for resolving dependencies. A dependency
is an object that can be used (a service). An injection is the passing of a dependency to dependency object (a
client) that would use it.
__Service Locator__ - design pattern that also implements inversion of control, the difference is that with service
locator the application class asks for dependency explicitly by a message to the locator. With injection there is no
explicit request, the service appears in the application class.

#### DEPENDENCY INJECTION
* Injection can be made by:
    * Fields
    * Setters
        * no need to use constructors (with many parameters)
        * there is a gap between object creation and injection of dependency and if this object will be injected in
        some other there is a possibility of NullPointerException if some method will be executed and dependency is
        not present yet
    * Constructors
        * simple, object is created with all dependencies
        * need to create constructor with multiple arguments
        * if multiple constructors exists there is a problem which constructor should be executed

#### Design Patterns
* Design pattern is a general repeatable solution to a commonly occurring problem in software design.  A design
pattern isn't a finished design that can be transformed directly into code. It is a description or template for how
to solve a problem that can be used in many different situations.
* Description of design pattern:
    * Name
    * Problem
    * Solution
    * Consequence
* Benefits:
    * standard solution created by experts with experience
    * make code more flexible
    * easier to scale
    * understand by all developers (improve communication)
* Drawbacks:
    * may be complex
    * increase amount of code and as a result may decrease understandability of a design
    * bad implementation of pattern or choosing wrong pattern

#### Design Patterns in Java
    * Strategy - Comparator
    * Iterator - Iterator in collections
    * Decorator - e.g. BufferedInputStream, BufferedReader
    * Template method - java.io
    * Observer - swing