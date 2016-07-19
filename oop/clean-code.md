# Clean Code

## 3. Functions

### General Principles
* __Small__ - functions should be small, no more than 20 lines long
* __Indent__ - no more than one or two, it makes functions easier to read and understand
* __Do One Thing__ - make sure that the statements within our function are all at the same level of abstraction
* __The Stepdown Rule__ - we wane every function to be followed by those at the next level of abstraction so that we can read the program,
descending one level of abstraction at a time as we read down the list of functions.
* __Switch statements__ - it violates the SRP because it always do N things, also Open/Close Principle because we always need to modify
function when we want to extend functionality, one of the solution to switch statement is to use it inside __Abstract Factory__
* __Use Descriptive Names__ - choose a proper name that describe the intent of the function and the order and intent of the arguments, 
don't be afraid to make a name long, be consistent in your names - use the same phrases, nouns and verbs. For example, assertEquals 
might be better written as assertExpectedEqualsActual(expected, actual).
* __Have No Side Effects__ - side effect is when a function promises to do one thing, but it also does other hidden things, for example 
it can be unexpected changes to the variables of its own class or to the parameters passed into the function.
* __Command - Query Separation__ - function should either do something or answer something, but not both so it should either change the 
state of an object, or it should return some infromation about that object.
* __Prefer Exceptions than returning error codes__ - Error Handling is also a One Thing

### Function Arguments number:
* __zero__ - it is the ideal number
* __one__ - it is the most natural situation and easy to read
* __two__ - sometimes two arguments are also natural (e.g. point have 2 parameters (x,y))
* __three__ - should be avoided where possible, more than three should't be used

__Function Arguments good practises:__
* __return value__ - if function take some argument and change it than it should return the changed argument through return value
* __output arguments__ - function should not change arguments that it takes without returning anything, it may be misleading
* __avoid flag arguments__ - render(true) => renderForSuite() and renderForSingleTest()
* __make object from arguments__ - if function seems to need more than two or three arguments, it is likely that some of those arguments
may be wrapped into a class of their own
```java
Circle makeCircle(double x, double y, double radius);
Circle makeCircle(Point center, double radius);
```
* __arguments lists__ - we can pass a variable number of arguments into a function and it also can monads, dyads or triads
```java
void monad(Integer... args);
void dyad(String name, Integer... args);
void triad(String name, int count, Integer... args);
```

### Writing a good function:
* __Write test__ - we should use TDD approach and start with writing tests 
* __Write production code__ - then immediately during writing tests we create production code, we create logic to the moment all our
test are green
* __Refactor__ - we refactor so we splitting our function, changing names, eliminating duplication, create classes if needed, all 
the while keeping the tests passing.

===

## 4. Comments
