# L3.1 - Philosophy of OOP
Classical
* algo come first
* data representations come later
	* design structure to suit procedural manipulations
OOP
* identify data we want to manipulate and maintain
* identify algorithms to operate on data
* works better for large systems

Nouns signify object, verbs signify methods

## Designing objets
* behaviour - what methods do we need to operate on objects?
* state - how does the object react when methods are invoked, state is the information about instance variables
	* encapsulation - should not change unless a method operates on it
* Identity - distinguish between different objects of the same class, state may be the same 
* these features interact
	* state will typically affect behaviour
* relationships between classes - dependence,, aggregations, inheritance

# L3.2 - Subclasses and inheritance
if parent class variables are private, and the subclass cannot access them directly, we use the super function, the super function acts like basically the constructor

Inheritance:
* In general, subclass has more features than parent class
	* subclass inherits instance variables, methods from parent class
* Every *Manager* is an *Employee*
	* can use a subclass in place of a superclass
	* ```Employee e = new Manager(m)```
	* but reverse will not work

# L3.4 - dynamic dispatch and polymorphism
* Subclass extends a parent
* inherits instance variables and methods from parent
* subclasses cannot see private components of parent class
* subclass can add more instance variables and methods
* can also override methods
```
# for example
double bonus(float percent) {
	return 1.5 * super.bonus(percent)
}
```
* say we declare an employee variable with a manager constructor and we call a bonus, which one will it call?
	* static: using superclass method
	* dynamic: using subclass method
* Dynamic dispatch 

functions:
* signature is its name and the list of argument types
* can have different functions with same nane and different signatures
* Overloading: different signatures
* overriding: multiple methods, choice is static
* dynamic dispatch: multiple methods, same signature, choice made at runtime

type casting: we use to overcome static type restrictions
* convert e to manager
	* ```((manager) e).setSecretary(s)```
* cast fails if e is not a manager
* can test if e is a manager
```
	  if (e instance of Manager) {
		((manager) e).setSecretary(s)
		}
```

# L3.4 - Class hierarchy
Multiple Inheritance: java allows no multiple inheriance
* if both classes have same function, which one is chosen?

class hierarchy: There is the most supreme class called object. Methods in object
* equals: but this defaults to pointer equality
	* == is also pointer equality, wtf
* toString(): converts object to string form
can exploit the tree function to write generic function
* can override the equals() function for our own objects (cuzit defaults to pointer in nondefined equality case)
	* eg:
		* boolean equals(Date d) # this does not work
		* boolean equals(Object 0) { if (d  instanceof Date){yadayadayada}}

# L3.5 - Subtyping vs inheritance
Class heirarchy provides both subtyping and inheritance
## Subtyping
* Capabilities of the subtype are a superset of the main type
* if B is a subtype of A, wherever we require an object of type A, we can use an object of type B
	* Employee e = new Manager() is legal

## Inheritance
* Subtype can reuse code of the main type
* B inherits from A if some functions for B are written in terms of function of A 
* Manager.bonus() uses Employee.bonus()
Taking example
* Subtyping
	* deque has more functionality than queue or stack
	* deque is a subtype of both these types
* Inheritance
	* Can suppress two functions in a deque and use it as a queue or stack
	* both queue and stack inherit from deque
Class hierarchy represents both subtyping and inheritance
* Subtyping
	* Compatibility of interfaces
	* B is a subtype of A if every function that can be invoked on an object of type A can also be invoked on object of type B
* Inheritance
	* Reuse of implementations
	* B inherits from A if some functions for B are written in terms of functions of A
* Using one idea (heirarchy of classes) to implement both concepts blurs the distinctions between the two

# L1.6 - Java modifiers
* Java uses modifiers in declarations to cover different features of object oriented programming
* public vs private to support encapsulation of data
* static for entities defined inside classes that exist without creating objects of the class
* final, for values that cannot be chaged
* These modifiers can be applied to classes, instance variables and methods

## public vs private
Faithful implementation of encapsulation necessities modifiers public and private
* typically, instance variables are private
* methods to query (acessor) and update (mutator) the state are public
* ![[Pasted image 20251104133506.png]]


## Accessor and Mutator methods
![[Pasted image 20251104133931.png]]

## Static components
use static components for components that exist without creating objects
* library functions main()
* Useful constants like Math.PI, Integer.MAX_VALUE
* These static components are also public
* private static makes sense?
	* Internal constants for bookkeeping (eg: constructor sets unique id for each order)
![[Pasted image 20251104134240.png]]

## final components
* final denots that a value cannot be updated
* usually used for constants (public and static instance variables)
* used for methods
	* cannot redefine functions at run-time, unlike python
* Over-riding
	* subclass redefines a method available with the same signature in parent class

Can also have private classes