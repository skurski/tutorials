# Equals

The equals method implements an equivalence relation. It is:

1. __Reflexive:__ For any non-null reference value x, x.equals(x) must return true.
2. __Symmetric:__ For any non-null reference values x and y, x.equals(y) must re-
turn true if and only if y.equals(x) returns true.
3. __Transitive:__ For any non-null reference values x, y, z, if x.equals(y) returns true and y.equals(z) returns true,
then x.equals(z) must return true.
4. __Consistent:__ For any non-null reference values x and y, multiple invocations of x.equals(y) consistently return
true or consistently return false, provided no information used in equals comparisons on the objects is modified.
5. For any non-null reference value x, x.equals(null) must return false.

__Good equals:__

1. Check if NULL
2. Use the == operator to check if it is the same object
3. Use the instanceof operator to check if correct type
4. Cast to the correct type
5. Perform comparison of every "significant" field in the class, if all fields matches return true, otherwise return
false

# Hashcode

```java
@Override public int hashCode() {
    int result = 17;
    result = 31 * result + areaCode;
    result = 31 * result + prefix;
    result = 31 * result + lineNumber;

    return result;
}
```
__Contract:__

1. Whenever it is invoked on the same object more than once during an execution of an application, the hashCode method
 must consistently return the same integer, provided no information used in equals comparisons on the object is 
 modified. This integer need not remain consistent from one execution of an application to another execution of the 
 same application.
2. If two objects are equal according to the equals(Object) method, then calling the hashCode method on each of the
two objects must produce the same integer result.
3. It is not required that if two objects are unequal according to the equals(Object) method, then calling the
hashCode method on each of the two objects must produce distinct integer results. However, the programmer should be 
aware that producing distinct integer results for unequal objects may improve the performance of hash tables.

__Practices:__

If a class is immutable and the cost of computing the hash code is significant, you might consider caching the hash 
code in the object rather than recalculating it each time it is requested. If you believe that most objects of this 
type will be used as hash keys, then you should calculate the hash code when the instance is created. Otherwise, you 
might choose to lazily initialize it the first time hashCode is invoked.
```java
// Lazily initialized, cached hashCode
private volatile int hashCode; // (See Item 71)

@Override public int hashCode() {
    int result = hashCode;
    if (result == 0) {
        result = 17;
        result = 31 * result + areaCode;
        result = 31 * result + prefix;
        result = 31 * result + lineNumber;
        hashCode = result;
    }
    return result;
}
```

__Recipe to write good hashcode:__

1. Store some constant nonzero value, say, 17, in an int variable called result.
2. For every field f tested in the equals() method, calculate a hash code c by:
    1. If the field is a boolean, compute (f ? 1 : 0).
    2. If the field is a byte, char, short, or int, compute (int) f.
    3. If the field is a long, compute (int) (f ^ (f >>> 32)).
    4. If the field is a float, compute Float.floatToIntBits(f).
    5. If the field is a double, compute Double.doubleToLongBits(f), and then hash the resulting long as in step 2.3
    6. If the field is an object reference and this class’s equals method compares the field by recursively invoking 
    equals, recursively invoke hashCode on the field. If a more complex comparison is required, compute a “canonical 
    representation” for this field and invoke hashCode on the canonical representation. If the value of the field is 
    null, return 0 (or some other constant, but 0 is traditional).
    7. If the field is an array, treat it as if each element were a separate field. That is, compute a hash code for 
    each significant element by applying these rules recursively, and combine these values per step 3. If every 
    element in an array field is significant, you can use one of the Arrays.hashCode methods added in release 1.5.
3. Combine the hash code c computed in step 2 into result as follows:
```java result = 31 * result + c;```
4. Return result.
5. When you are finished writing the hashCode method, ask yourself whether equal instances have equal hash codes. 
Write unit tests to verify your intuition! If equal instances have unequal hash codes, figure out why and fix the 
problem.

# Comparable / Comparator

Compares this object with the specified object for order. Returns a negative integer, zero, or a positive integer as 
this object is less than, equal to, or greater than the specified object. Throws ClassCastException if the specified 
object’s type prevents it from being compared to this object. In the following description, the notation sgn
(expression) designates the mathematical signum function, which is defined to return -1, 0, or 1, according to 
whether the value of expression is negative, zero, or positive.

1. The implementor must ensure sgn(x.compareTo(y)) == -sgn(y.compareTo(x)) for all x and y. (This implies that x
.compareTo(y) must throw an exception if and only if y.compareTo(x) throws an exception.)
2. The implementor must also ensure that the relation is transitive: (x.compareTo(y) > 0 && y.compareTo(z) > 0)
implies x.compareTo(z) > 0.
3. Finally, the implementor must ensure that x.compareTo(y) == 0 implies that sgn(x.compareTo(z)) == sgn(y.compareTo
(z)), for all z.
4. It is strongly recommended, but not strictly required, that (x.compareTo(y)== 0) == (x.equals(y)). Generally
speaking, any class that implements the Comparable interface and violates this condition should clearly indicate this
 fact. The recommended language is “Note: This class has a natural ordering that is inconsistent with equals.”
 
  
Compare methods can be improved by using subtractions rather than if conditions because contract specify only the
sign of return value. This trick works fine here but should be used with extreme caution. Don’t use it unless you’re 
certain the fields in question are non-negative or, more generally, that the difference between the lowest and 
highest possible field values is less than or equal to Integer.MAX_VALUE (2^31-1).
```java
public int compareTo(PhoneNumber pn) {
    // Compare area codes
    int areaCodeDiff = areaCode - pn.areaCode;
    if (areaCodeDiff != 0)
        return areaCodeDiff;
    // Area codes are equal, compare prefixes
    int prefixDiff = prefix - pn.prefix;
    if (prefixDiff != 0)
        return prefixDiff;
    // Area codes and prefixes are equal, compare line numbers
    return lineNumber - pn.lineNumber;
}
```
