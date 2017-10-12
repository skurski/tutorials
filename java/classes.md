# Nested classes in Java 7

### Types of nested classes
1. Non-static nested classes called __inner classes__
  * class created inside another class
  * has access to all fields and methods of outer classes
  * we need reference to outer class instance to be able to call nested class from outside
  * __should not implement Serializable__
2. Static nested classes
  * class created inside another class with static keyword
  * has access only to static fields and methods of outer class
  * we don't need reference to outer class instance to be able to call static nested class from outside
  * __can implement Serializable__
3. Local nested classes
  * class created inside method
  * has access to all fields and methods of outer classes
  * has access to local variables and method parameters but only if they are declared as __final__
4. Anonymous classes
  * used to create object in a fly when we don't need reference to object (we use it only once)
  * when the amount of code is small
  
  
