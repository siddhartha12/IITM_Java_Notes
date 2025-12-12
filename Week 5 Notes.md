# L5.1 - Polymorphism Revisited
refers to effect of dynamic dispatch

* S is a subclass of T
* S overrides a method of f() defined in T
* variable v of type T is assigned to an object of type S
* v.f() uses the definition from f() from S rather than T

Every object "knows" what it needs to do

More Generally, polymorphism refers to behaviour that depends only a specific capabilities - **Structural Polymorphism**
* Reverse an array/list (should work for any type)
* search an element in an array/list (need equality check, needs extraction ability)
* sort an array/list (need to compare values)

Structural Polymorphism
* Use java class heirarchy to simulate this
* for example we can implement Polymorphic Reverse using the object methods defined by java

## Polymorphic Reverse
```
public voic reverse (Object[] objarr) {
	Object tempobj;
	int len = objarr.length;
	for (i=0; i < n/2; i++) {
		tempobj = objarr[i];
		objarr[i] = objarr[(n-1)-i];
		obj
	}
}
```

## Polymorphic find
```
public int find(Object[] objarr, Object o) {
	int i;
	for (i=0; i < objarr.length; i++) {
		if(objarr[i] == o) {
			return i;
		}
	}
	return (-1);
}
```

* The `==` might not work the same way sometimes it may do a pointer compare so instead of that we use `objarr[i].equals(o)`

## Polymorphic Sort
```
public interface Comparable {
	public abstract int cmp(Comparable s);
}

public class SortFunctions {
	public static void quicksort(Comparable[] a) {
		// usual code for quicksort
		// compare using a[i].cmp(a[j])
	}
}
```

## Polymorphic Copy
```
public static void arraycopy (Object [] src, Object[] tgt) {
	int i, limit;
	limit = Math.min(src.length, tgt.length);
	for (i = 0; i < limit; i++) {
		tgt[i] = src[i];
	}
}
```
* Need to ensure that target array is type compatible with source array
	* Type errors should be flagged at compiled time
* More generally source array can be a subtype of the target array
```
# Example - 1
public class Ticket {
	//...
}

public class ETicket extends Ticket {
	//...
}

Ticket[] tktarr = new Ticket[10];
Eticket[] Etktarr = new ETicket[10];

arraycopy(etktarr, tktarr); // legal, basically every Eticket is a ticket

arraycopy(tktarr, etktarr) // Illegal, but every ticket is not an Eticket
```

## Polymorphic LinkedList
```
public class LinkedList {
	private int size;
	private Node first;
	
	public Object head() {
		Object returnval;
		
		return(returnval);
	}
	
	public void insert(Object newdata) {
		//...
	}
	
	private class Node {
		private Object data;
		private Node next;
	}
}
```
Problems
* Type information is lost, need casts
	`t1 = new Ticket(); list.insert(t1); t2 = (Ticket)(list.head();`
		Here the head returns a a simple object of no type, so we need to cast it back into its original type again
* List need not be homogenous

Solution: 
* Use generic programming to address these issues
* Classes and Functions can have type parameters
	* `class LinearList<T>` holds value of type T
	* `public T head() {...}` returns a value fo sme type as T as enclosing the class
* Can describe subclass relationships between type variables
	* `public <S extends T,T> void arraycopy(S[] stc, T[] tgt) {...}`


# L5.2 - Generic Programming in Java

## Generic Polymorphic Functions
```
# Example 1 - reverse
// type quantifier before return type
public <T> void reverse (T[] objarr) {
	T tempobj;
	int len = objarr.length;
	for (i=0, i<n/2; i++) {
	  // Same as above
	}
}

# Example - 2 - Find
// searching a value of incompatible type is compile time error
public <T> int find(T[] objarr, T o) {
	int i;
	for (i=0, i < objarr.length; i++) {
		if (objarr[i] == o) {
			return i;
		}
	}
}

# Eg3 - Copy
public <T> static void arraycopy(T[] src, T[] tgt) {
	// ...
}

# A more generous implementation of it is as follows
public <S extends T, T> static void arraycopy(S[] src, T[] tgt) {
	// ...
}
```

## Generic Polymorphic Data Structures
```
// For homogenous types
public class LinkedList<T> {
	private int size;
	private Node first;
	
	public T head() {
		T returnval;
		
		return(returnval);
	}
	
	public void insert(T newdata) {
		// ...
	}
	
	private class Node {
		private T data;
		private Node next;
	}
}
```

# L5.3 - Java Generics and Subtyping
## Extending subtyping in contexts
* If S is compatible with T, `s[]` is compatible `T[]
```
Eticket[] elecarr = new ETicket[10];
Ticket[] ticketarr = elecarr; # valid
# ETicket[] is a subtype of Ticket[] 

# but
ticketarr[5] = net Ticket();
# cannot assign ticket variable to a subtype eticket variable
# type error at runtime
```
* A type error at run time!
* Java array typing is covariant
	* If S extends T, then `s[] extends T[]`

### Generics and subtypes
* Generic classes are not covariant
	* `LinkedList<String> is not compatible with LinkedList<Object>
* The following will not work to print out an arbitrary `LinkedList`
  ```
  public class LinkedList<T> {...}
  
  public static void printlist(LinkedList<Object> l){
	  Object O;
	  Iterator i = l.get_iterator();
	  while (i.has_next()) {
		  o = i.get_next;
		  System.out.println(o);
	  }
  }
  ```


### Generic Methods
```
public class LinkedLIst<T>{...}

public static <T> void printlist(LinkedList<T> l) {
	Object o;
	Iterator i = l.get_iterator()
	while(i.has_next()) {
		o = i.get_next();
		System.out.println(o);
	}
}
```
* `<T> is a type quantifier, for every type <T>
* Note that T is not used inside the function
	* We use Object o as a generic variable to cycle through the list

## Wildcard Type
Instead, use ? as a wildcard type variable
```
public static void printlist(LinkedList<?> l)
```
* ? stands for an arbitrary unknown type
* Can define variables of a wildcard type
  `public class LinkedList<T>{...}; LinkedList<?> l;`
* But need to be careful about assigning values
  `LinkedList<?> l = new LinkedList<String>(); l.add(new Object()); // compile time error`
* Compiler cannot guarantee the types match

So compiler cannot guarantee that types match, can use wild cards as arguments to functions where the type information of the arbitrary type is not required anywhere in the function, but do not use in order to generate variables and assign values to them

## Bounded Wildcards
* Suppose Circle, square and rect extend shape
* shape has draw()
* all sub overrides
* want a function to draw all elements in a list of shape compatible objects
* `drawAll(LinkedList<? extends shape> l) {}`

Copying linkedlist, using a wildcard
`public static <? extends T, T> void function(Linkedlist<?> src, LinkedList<T> tgt)`
can do in the opposite also
`<T, ? super>`

# L5.4 - Reflection
Reflective programming or reflection is the ability of a process to examine, introspect and modify its own structure and behaviour

Two parts involved in reflection
* Introspection
	* A program can observe, and therefore reason about its state
* Intercession
	* A program can modify its execution state or alter its own interpretation or meaning


## Reflection in Java
* What is we don't know the type that we want to check in advance
* Suppose we want to write a function to check if two different objects are both instances of the same class
```
public static boolean classequal(Object o1, Object o2) {
	// return true if o1 and o2 are same
}
```
* Cant use instaceof
	* will have to check across all defined classes
	* This is not a fixed set
* Can extract the class of an object using `getClass()
* Import package using `java.lang.reflect`
```
import java.lang.reflect.*;

class MyReflectionClass{
	public static boolean classequal(Object o1, Object o2) {
		return (o1.getClass() == o2.getClass());
	}
}
```

What does `getClass()` return?
* An object of type Class that encodes class information

Can also phrase the classequal as
```
c1 = o1.getClass()
c2 = o2.getClass()
return(c1==c2)
```

Using the class object
```
Class c = obj.getClass();
Object o = c.newInstance(); // creates a new object of same type as obj

# Another way
* Can also get hold of the class object using the name of the class
String s = "Manager";
Class c = Class.forName(s);
Object o = c.newInstance();
```

We can also extract details about constructors, methods and fields of the class
* Constructors, methos and fields themselves have structure
	* contructors: argument
	* method: arguments and return type
	* All three: modifiers static, private etc
* Additional Classes `Constructor, Method, Field`
* Use
	* `getConstructors()`
	* `getMethods()`
	* `getFields()`

Get the list of parameters for each constructor
```
Class c = obj.getClass();
Constructor[] constructors = c.getConstructors();
for (int i=0;i<constructors;i++) {
	class params[] = constructors[i].getParameterTypes();
}
```

We can also invoke methods and examine/set values of fields
```
class c = obj.getClass()

Method[] methods = c.getMethods();

Object[] args = {}

methods[3].invoke(obj, args)
```

Reflection and security
* Can we extract information about private methods, fields
* getConstructors only return publicly defined values
* Separate functin to also include private components
	* `getDeclaredConstructors()
	* `getDeclaredMethods()
	* `getDeclaredFields()

Using Reflection
* BlueJ, a programming environment to learn Java
* can define and compile java classes
* for compiled code, create object, invoke methods, examine state
* uses reflective capabilities of Java - BlueJ need not internally maintain "debugging" information about each class

Limitations
* Canot create or modify classes at runtime
* Other langugages like Smalltalk allow redefining methods at run time

# L5.5 - Java Generics at runtime
Erasure of generic information
* Type erasure - Java does not keep record of all versions of `LinkedList<T>` as separate types
* Cannot write
	* `if (s instaceof LinkedList<String>) { ... }`
* At run time, all type variables are promoted to `Object`
	* `Linkedlist<T> becomes LinkedList<Object>`
* Or the upper bound if one is available
	* `LinkedList<? extends Shape> becomes <inkedList<Shape>`
* Since no information about `T` is preserved, cannot use `T` in the expressions like
	* `if (o instanceof T) { ... }`

Erasure and Overloading
* Type erasure means the comparison in the following code fragment returns `True`
```
o1 = new LinkedList<Employee>();
o2 = new LinkedList<Date>();

if (o1.getClass() == o2.getClass()) {
	// True, so this block is executed
}
```
* As a consequence, the following overloading is illegal
```
public class Example {
	public void printlist(LinkedList<string> strList)
	public void printList(LinkedList<date> datelist)
}
```
* both functions have same signature

Arrays and generics
* Recall the covariance problem for arrays
	* If S extneds T then `s[] exntends T[]
* can lead to runtime type errors
* To avoid similar problem, can declare a generic array, but cannot instantiate it
	* `T[] newarray; //OK; newarray = new T[100]; // cant`
* Ugly workaround
	* `T[] newarrray; newarray (T[]) new Object[100]`

Wrapper classes
* Type erasure - at runtime, all type variables are object
* Basic types - int, float, are not compatible with object
* Cannot use basic type in place of a generic type variable T
	* Cannot instantiate `linkedlist<T>` as `LinnkedList<int> or <double>`
* Wrapper class for each basic type
	* byte = Byte
	* short = Short
	* int = Integer
	* long = Long
	* float = Float
	* double = Double
	* boolean = Boolean
	* char = Character
*  All wrapper classes other than Boolean, Character extend the class Number

Converting back and forth
```
int x = 5;
Integer myx = Integer(x);
int y = myx.Value();
```
doing it for all becomes a nuisance so there's something called autoboxing
```
int x = 5;
Integer myx = x;
int y = myx;
```
