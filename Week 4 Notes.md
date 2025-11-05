# L4.1 - Abstract classes and interfaces
## Grouping together classes
* Sometimes we collect together classes under a common heading
* Classes Circle, Square and Rectangle are all shapes
* Create a class shape so that Circle, Square and Rectangle extend Shape
* We want to force every shape to define a function `public double perimeter()`
* Could define a function in `shape` that returns an absurd value
* rely on subclass to redefine this function

## Abstract classes
A better solution
* Provide an abstract definition in Shape `public abstract double perimeter()`
* Forces subclasses to provide a concrete implementation
* Cannot create objects from a class that has abstract functions
* shape must itself be declared to be abstract
```
public abstract class Shape{
	...
	public abstract double perimeter();
	...
}
```

Can still declare variables whose type is abstract class, like in te example below
```
shape shapearr[] = new Shape[3]

shapearr[0] = new Circle(...);
shapearr[1] = new Rectangle(...);
shapearr[2] = new square(...);

for(i=0; i < 2; i++) {
	sizearr[i] = shapearr[i].perimeter()
	//each shapearr calls the appropriate method
}
```
## Generic functions
Use abstract classes to specify generic properties
```
public abstract class Comparable {
	public abstract int cmp(Comparable s);
	// return -1 if this < s
	// 0 if this = s
	// +1 if this > s	
}

// now we can sort any array of objects that extend Comparable
public class SortFunctions {
		public static void quickcsort(Comparable[] a) {
			...
			//usual code for quicksort except that 
			// to compare a[i] and a[j] we use a[i].cmp(a[j])
		}
}

//to use this definition of quicksort, we write
public class Myclass extends Comparable {
	private double size;
	
	public int cmp(Comparable s){
		//compare this.size and ((Myclass) s).size
		//Note the cast to access s.size
	}
}
```

## Multiple Inheritance
* can we sort Circle objects using the generic functions in SortFunctions?
	* Circle already extends Shape
	* Java doesn't allow Circle to also extend Comparable
An interface is an abstract class with no concrete components
```
public interface Comparable {
	public abstract int cmp(Comparable s);
}

// A class that extends an interface is said to implement it
public class Circle extends Shape implements Comparable{
	public double perimeter(){
	}
}
```
Therefore, can extend only one class but implement multiple interfaces

# L4.2 - Interfaces
* an interface is a purely abstract class
	* All methods are abstract
* A class implements an interface
	* provides concrete code for each abstract function
* Classes can implement multiple interfaces
	* abstract functions so no contradictory inheritance
* Interfaces describe relevant aspects of a class
	* Abstract functions describe a specific slice of capabilities
	* another class only needs to know about these capabilities

Exposing limited capabilities
* Generic quicksort for any datatype that supports comparisons
* Express this capability by making the argument type `Comparable[]`
	* Only information that quicksort needs about the underlying type
	* All other aspects are irrelvant
	* Describe the relevant functions supported by `Comparable` objects through an interface

Java interfaces extended to allow functions to be added
* Static functions
	* Cannot access instance variables
	* Invoke directly or using interface name `Comparable.cmpdoc()
* Default functions
	* Provide a default implementation for some functions
	* Class override these
	* Invoke like normal method, using object name `a[i].cmp(a[j])`

Dealing with conflicts
* old problems of multiple inheritance returns
	* conflict between static/default methods
* Subclass must provide a fresh implementation
* Conflict between class and interface,
	* methods inherited from class wins

# L4.3 - Private Classes
nested objects
* an instance variable can be a user defined type
	* `employee uses date`
* `Date` is a public class also available to other classes

Eg; 
* Linked list - built using Node
* Make Node a private class for securty and encapsulation
	* Nested within LinkedList
	* Also called an inner class
* Objects of private class can see private components of enclosing class

# L4.4 - controlled interaction with objects
* Encapsulation is a key principle of object oriented programming
	* Internal data is private
	* Access to data is regulated through public methods
	* Accessor and mutator methods
* Can ensure data integrty by regulating access
	* Update date as a whole rather than individual components

Example: querying a database
* Object stores train reservation information
	* can query availability for a given train, date
* To control spamming by bots, require user to log in before querying
* Need to connect the query to the logged in status of the user
* "Interaction with State" - Need to connect the query to the logged in status of the user
* Use objects
	* on log in user receives an object that can make a query
	* Object is created from private class that can lookup railwaydb
* How does user know the capability of private class queryObject
* Use an interface
	* Interface describes the capability of the object returned on login
* Query object allows unlimited number of queries
	* to maintain we could possible limit number of queries per login
* Maintain a counter
	* Add instance variables to object returned on login
	* Query object can remember the state of the interaction
![[Pasted image 20251105170955.png]]

# L4.5 - Callbacks
Implementing a call-back facility
* `Myclass m `creates a `Timer t`
* Start `t` to run in parallel
	* `Myclass m` continues to run
	* Will see later how to invoke parallel execution in Java
* `Timer t` notifies `Myclass m` when the time limit expires
	* Assume `Myclass m` has a function `timerdone()`
![[Pasted image 20251105172652.png]]

Need a generic timer for all objects
* Use Java class heirarchy
* Parameter of `Timer` constructor of type `Object`
* Need to cast owner back to `Myclass
* Solution: use interfaces
![[Pasted image 20251105172858.png]]

Runnable is the interface that we can use to run the tasks in parallel

# L4.6 - Iterators
Linear list
* Generic linear list of objects
* Internal implementation may vary
	* array implementation
	* linked list implementation
Want a loop to run through all values in a linear list
* If list is in an array with public access, we can use a simple for loop
* If linked list
	* ```
	  Node m;
	  for (m = head; m != null; m = m.next) {
		  ...
	  }
	  ```
* We don't have public access or knowledge of implementation

make the following abstractinos
```
Start at the beginning of the list
While (there is a next element) {
get the next element
do something with it
}
```

encapsulate this functionality in an interface called iterator
```
public interface iterator {
	public abstract boolean has_next();
	public abstract Object get_next();
}
```
* How do we implement iterator in LinearList
* Need a "pointer" to remember position of the iterator
* how do we handle nested loops?
* Solution: create an Iterator object and export it
![[Pasted image 20251105173857.png]]

![[Pasted image 20251105174217.png]]

