# L1.1 - Introduction
Abstractions used in computational thinking
* Assigned values to vairables
* conditional execution
* iteration
* functions/recursion
* aggregate data structures - arrays, lists, dictionaries
Compilers translate from high level to machine level
interpreters one sentence at a time

Trade off expressiveness for efficiency
* less control over how code is mapped to architecture
* but fewer errors due to mismatch between intent and implementation

## styles of programming
* imperative vs declarative
* Imperative
	* how to compute
	* step by step
* Declarative
	* what the computation should produce
	* Often explort inductive strucutres, express in terms of smaller computations
	* typically avoid using intermediate variables
	* combination of small transformations - functional programming
Example: add values in a list
![[Pasted image 20251025193735.png]]

Basically avoid large blocks of instructions, and breka them up into smaller meaningful units of code which are easier to process

## names, types, values
* Internally everything is stored a sequence of bits
* no difference between data and instructions 
* we impose a notin of type to create some discipline
	* interpret bit strings as high level concepts
	* nature and range of values allowed
	* operations that are permitted on these values

## OOP, abstract datatypes
* collections are important
	* arrays, lists, dictionaries
* Abstract data types
	* structures collections with fixed interface
	* stack is a sequence allows push and pop
	* separate implementation from interface
		* priority queue allows insert and delete-max
		* can implement a priority ququq using sorted or unsortes list or using a heap
* OOP
	* focus on datatypes
	* functions are invoked through the object rather than passing data to the funcitons
	* In python, mylist.sort() vs sorted(myList)

## TO learn
* OOP
* Exception handling
* Using java as the illustrative language

# L1.2 - types
## Role of types
* interpreting data stored in binary in a consistent manner
* naming concepts and structuring out computation
* catching bugs early (incorrect expression of assignment)

## dynamic vs static typing
* every variable we use has a type
* dynamic typing - python determines the type based on current value but difficult to catch errors
* static typing - associate and declare a type in advance
comparing:
* dynamic typing has Empty user defined objects
* static types help clarify code
* More elaborate types - abstract datatypes and object-oriented programming
* no overhead of runtime monitoring


# L1.3 - Memory management
Keeping track of variables:
* variables store immediate values during computation
	* these are local to function
	* can also refer to global variables outside function
	* dynamically create data like notes in a list
* Scope of a variable
	* where the variable is available for use
* lifetime of a variable
	* how long the storage remains allocated
	* hole in scope - variable is alive but not in scope

## Memory stack
* each function needs storage for local variables
* create activation record when function is called
* Activation records are stacked
	* popped when function exits
	* control link points to the start of previous record
	* return value link tells where to store result
* scope of a variable
	* variable in activation record at top of stack
	* access global variables by following control links
* lifetime of variable
	* storage allocated in still on the stack
* When a function is called, arguments are substituted for formal parameters
* Parameters are part of the activation record of the function 
	* values are populated on function call
	* like having implicit assignment statement at the start of the function
* two ways to initialize parameters
	* call by value - copy value
	* call by reference - points to the same location in memory

## Heap
* function that inserts a value in a linked list
	* storage for new node allocated inside function
	* node should persist after function exits
	* cannot be allocated within activation record
* separate storage for persistent data
	* dynamically allocated vs statically declared
	* usually called the heap (not the same as the data structure)
	* conceptually allocate heap storage from opposite end with respect to stack
* heap outlines activation record

managing storage
* on the stack, variables are deallocated when a function exits
* how do we return unused storage on the heap
	* after deleting a node in a linked list, deletd node now dead storage unreachable but still occupying space
* manual memory management
	* explicitly request and return heap storage
	* error prone - memory leaks, invalid assignments
* Automatic garbage collection
	* runtime environment checks and cleans up dead storage
		* mark all storage that is reachable from program variables
		* return all unmarked memory cells to free space
		* disadvantage that slowdowns can happen cuz garbage collector needs to run periodically
	* convenience for programmer vs performance penalty


# L1.4 - Abstraction and Modularity

stepwise refinement
* begin with a high level description of the task
* refine the task into subtasks
* further elaborate each subtask
* subtasks can be coded by different people

Modular software development
* use refinement to divide the solution into components
* Build a prototype of each components to validate design
* Components are described in terms of 
	* interface -> what is visible to other components, typically function calls
	* specification -> behaviour of the components, as visible through interface
* Improve each component independently, preserving interface and specification
* Simplest example of a component: function
	* interfaces - function header, arguments and return type
	* specification - intended input-output behaviour
* Main language: suitable language to write specification
	* balance abstraction and detail, should not be another programming language
	* cannot algorithmically check that specification is met (halting problem!)

programming language support for abstraction
* control abstraction
	* functions and procedures
	* encapsulate a block of code, reuse in different contexts
* Data abstraction
	* abstract data types (ADT)
	* Set of values along with operations permitted on them
	* internal representation should not be acessible
* Object oriented programming
	* organize ADTs in a heirarchy
	* Implicit reuse of implementation - subtyping, inheritance

# L1.4 - OOP
* An object is like an abstract datatype
	* Hidden data with set of public operations
	* All interaction through operations - messages, methods, member-functions
* Uniform way ot encapsulating different combinations of data and functionality
	* an object can hold single integer
	* an entire filesystem or database can be a single object too
* distinguishing features of object-oriented programming
	* abstraction
	* subtyping
	* dynamic lookup
	* inheritance

## abstraction
* objects are similar to abstract datatypes
	* public interface
	* private implementation
	* changing the implementation should not affect interactions with the object
* Data centric view of programming
	* focus on what data we need to maintain and manipulate
* recall that stepwise refinement could affect both code and data
	* tying methods to data makes this easier to coordinate
	* refining data representation naturally tied to updating methods that operate on the data

## subtyping
* a well types queue holds values of a fixed type
* in practice, the queue holds different types of objects
* Arrange types in a heirarchy
	* a subtype is a specialization of a type
	* If A is a subtype of B, wherever an object of type B is needed, an object of type A can be used

## dynamic lookup
* whether a method can be invoked on an object is a static property - type checking
* how the method acts is a dynamic property of how the object is implemented
	* in the simulation queue, all events support a simulate method
	* The action triggered by the method depends on the type of the event
	* In a gui, different types of objected to be rendered
	* invoke using same operation, each object knows how to render itself
* different from overloading
* Dynamic lookup
	* a variable v of type B can refer to an object of subtype A
	* static type of v is B but method implementation depends on runtime type A

## Inheritance 
* reuse of implementations
* Usually one hierarchy of types to capture both subtyping and inheritance
	* A can inherit from B if A is a subtype of B
* Philosophically two are different
	* Subtyping is a relationship of interfaces
	* Inheritance is a relationship of implementations

# L1.6 - Classes and Objects
* objects are like abstract data types
* Class
	* template for datatype
	* how data is stored
	* how public functions manipulate data
* object
	* concrete instance of template
	* each object maintains a separate copy of local data
	* invoke methods on objects - sen a message to the object

Basically watch the lecture as it's an example based explanation for subtyping and inheritance

Summary:
* We should separate public intreface from private implemtnation
* hierarchy of classes to implememt subtyping and inheritance
* python has no mechanism to enforce privacy
