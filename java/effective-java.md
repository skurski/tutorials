# Effective Java by Joshua Bloch

### Static factory methods instead of constructors

```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```

* One advantage of static factory methods is that, unlike constructors, they have names.
* A second advantage of static factory methods is that, unlike constructors, they are not required to create a new 
object each time they’re invoked.
* A third advantage of static factory methods is that, unlike constructors, they can return an object of any subtype 
of their return type.
* A fourth advantage of static factory methods is that they reduce the verbosity of creating parameterized type 
instances.
* The main disadvantage of providing only static factory methods is that classes without public or protected 
constructors cannot be subclassed.
* A second disadvantage of static factory methods is that they are not readily distinguishable from other static 
methods.

===
### Builder pattern instead constructors with many parameters

* the Builder pattern is a good choice when designing classes whose constructors or static factories would have more 
than a handful of parameters, especially if most of those parameters are optional.

===
### Singleton

```java
// Singleton with public final field
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();

    private Elvis() { ... }

    public void leaveTheBuilding() { ... }
}

// Singleton with static factory
public class Elvis {
    private static final Elvis INSTANCE = new Elvis();

    private Elvis() { ... }

    public static Elvis getInstance() { return INSTANCE; }

    public void leaveTheBuilding() { ... }
}

// Enum singleton - the preferred approach
public enum Elvis {
    INSTANCE;

    public void leaveTheBuilding() { ... }
}
```
* A singleton is simply a class that is instantiated exactly once. Singletons typically represent a system component
that is intrinsically unique,such as the window manager or file system.
* Making a class a singleton can make it difficult to test its clients, as it’s impossible to substitute a mock 
implementation for a singleton unless it implements an interface that serves as its type.
* To make a singleton class that is implemented using either of the previous approaches serializable (Chapter 11), it
 is not sufficient merely to add implements Serializable to its declaration. To maintain the singleton guarantee, you
  have to declare all instance fields transient and provide a readResolve method. Otherwise, each time a serialized 
  instance is deserialized, a new instance will be created, leading, in the case of our example, to spurious Elvis 
  sightings. To prevent this, add this readResolve method to the Elvis class:
```java
// readResolve method to preserve singleton property
private Object readResolve() {
    // Return the one true Elvis and let the garbage collector
    // take care of the Elvis impersonator.
    return INSTANCE;
}
```

===
### Avoid creating unnecessary objects

* ```java String s = new String("stringette"); // DON'T DO THIS!```
```java
// BAD APPROACH
public class Person {
    private final Date birthDate;
    // Other fields, methods, and constructor omitted
    
    // DON'T DO THIS!
    public boolean isBabyBoomer() {
        // Unnecessary allocation of expensive object
        Calendar gmtCal = Calendar.getInstance(TimeZone.getTimeZone("GMT"));
        gmtCal.set(1946, Calendar.JANUARY, 1, 0, 0, 0);
        Date boomStart = gmtCal.getTime();
        gmtCal.set(1965, Calendar.JANUARY, 1, 0, 0, 0);
        Date boomEnd = gmtCal.getTime();
        return birthDate.compareTo(boomStart) >= 0 && birthDate.compareTo(boomEnd) < 0;
    }
}

// GOOD APPROACH
class Person {
    private final Date birthDate;
    // Other fields, methods, and constructor omitted
    private static final Date BOOM_START;
    private static final Date BOOM_END;

    static {
         Calendar gmtCal = Calendar.getInstance(TimeZone.getTimeZone("GMT"));
         gmtCal.set(1946, Calendar.JANUARY, 1, 0, 0, 0);
         BOOM_START = gmtCal.getTime();
         gmtCal.set(1965, Calendar.JANUARY, 1, 0, 0, 0);
         BOOM_END = gmtCal.getTime();

    }

    public boolean isBabyBoomer() {
        return birthDate.compareTo(BOOM_START) >= 0 && birthDate.compareTo(BOOM_END) < 0;
    }
}
```
* prefer primitives to boxed primitives, and watch out for unintentional autoboxing.

===
### Classes and Interfaces

* Make each class or member as inaccessible as possible.
* Instance fields should never be public, classes with mutable fields are not thread safe
* It is wrong for a class to have a public static final array field, or an accessor that returns such a field because
 a nonzero-length array is always mutable => you should return immutable collection or copy of array
 ```java
    private static final Thing[] PRIVATE_VALUES = { ... };
    public static final List<Thing> VALUES =
        Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));
    
    private static final Thing[] PRIVATE_VALUES = { ... };
    public static final Thing[] values() {
        return PRIVATE_VALUES.clone();
    }
 ```

===
### Immutable class

1. Ensure that the class can't be extended => final class
2. Make all fields final.
3. Make all fields private.
4. Don't provide any methods that modify the object's state (known as mutators).
5. Ensure exclusive access to any mutable components.

===
### Favor composition over inheritance

1. Unlike method invocation, inheritance violates encapsulation. In other words, a subclass depends on the 
implementation details of its superclass for its proper function. The superclass’s implementation may change from 
release to release, and if it does, the subclass may break, even though its code has not been touched.
2. Examples of composition over inheritance in design patterns e.g.:
    * Decorator
    * Strategy
3. When inheritance is used the class must document its self-use of overridable methods (if child class override 
method that is used internally in parent class it will break the functionality of child class)
4. The only way to test a class designed for inheritance is to write subclasses.

===
### General advices

* Eliminate obsolete object references
* Avoid finalizers
    * Finalizers are unpredictable, often dangerous, and generally unnecessary.
* Always override toString
* Prefer interfaces to abstract classes
* Favor static member classes over nonstatic