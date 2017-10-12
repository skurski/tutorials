
# Interfaces in Java 7, Java 8 and Java 9

### Interface in Java 7

```java
interface example {
	int sum = 0; // public static final int sum = 0; constants
	void fun(); // public abstract void fun(); abstract method
}
```

#### Interface can only contains:
* constants, every field is public, final, static
* abstracts methods, every method is public and abstract

#### Usage
* to be able to implement interface method we need to create class that implement this interface and override method
* in order to create some common implementation we need to use abstract class (that implement this interface) and we can inherit only from one class so this is a big disadvantage


### Interface in Java 8

```java
interface example {
	int sum = 0; //constants
	void fun1(); //abstract method
  default void fun2(){
    //implementation
  }
  static boolean common() {
		System.out.println("Common functionality.");
}
```

#### Interface can contains (all public):
* constant variable
* abstract method
* default method with implementation
* static method - common functionality for interface


### Interface in Java 9 - now we have even more

* Constant variable
Abstract method
Default methodStatic methodPrivate methodPrivate Static method



