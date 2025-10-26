# L2.1 - First taste of Java
Print hello world
```
public class helloworld {
	public static void main(String[] args)
	{
		system.out.println("hello, world");
	}
}
```
unpacking syntax
* All code in java lies within a class
	* No free floating functions, unlike Python and other languages
	* Modifier public specifies visibility
		yeah so basically everything needs to be in a class in java
* How does the program start?
	* Fix a function name that will be called by default
	* From C, the convention is to call this function main()
* Need to specify input and output types for main()
	* the signature of main()
	* Input parameters is an array of strings, command line arguments
	* no output, so return type is void
* Visibility
	* function has to be run outside of class
* Availability
	* Functions defined inside classes are attached to objects
	* how can we create an object before starting?
	* statis modifier - function that exists independant of dynamic creation of objects
* Actual operation
	* system is a public class
	* out is a stream object
		* like a file handle
		* out must also be statis
	* println() is a method associated with streams
		* prints argument with a newline like python print()
		* 

Running Code
* a java program is a collection of classes
* each class is defined in a separate file with the same name with extension java
	* class helloworld in helloworld.java
* java programs are usually interpreted in JVM (java virtual machine)
	* uniform execution environment across os
	* semantics of java is defined in terms of jvm, os independent
	* write once, run anywhere
* creates bytecode with filename with class extension

# L2.2 - basic datatypes in java
## Scalar types
* an object oriented language all data should be encapsulated as objects
* However, cumbersome
	* useful to manipulate numeric values like conventional languages
* Java has 8 primitive scalar types
	* int 4
	* long 8
	* short 2
	* byte 1
	* float 4
	* double 8
	* char 2
	* boolean 1


* we declare variables before we use them
* characters have single quotes
* strings have double
* booleans are true, false
* appending f after the float number will make it a double
* final indicates a constant (eg: final float pi = 3.14159265f)
* arithmetic operators are usual
* when dividing ints, it becomes an implicit float eg 22/7 will return 3.0
* Exponentiation done using Math.pow()
* increment same as c a++
* operations available +=, *=, /=
* String is a built in class, not arrays of characters, instead use substring
* Arrays are also objects, declared as ``` int [] a or int a[]
* a.length gives length of the array
* for string its string.length()
* size of array can vary


# L2.3 - Control flow in java
* Program layout
	* statements end with ;
	* blocks with {}
* conditions: if, else, and "else if"
* conditional: while, and do while
* iteration: for (one iteration same as C)
* multiway: switch, use break (same as c)
	* cannot use expressions, have to be constants
	* return type is void
* no def for function
* mostly same as C
* also has a python style for loop:
	* for(type x : a)

# L2.4 - Defining classes an object in java
* class is a template for an encapsulated type
	* default visibility is public so can be eliminated
	* packages are administrative units of code
	* all classes defined in same directory form part of same package
	* Instance variables
		* each concrete object will have local copies of its variables in private (not public as that would break encapsulation)
* an object is an instance of a classs
* new creates a new object
* constructors are special functions called when object is created, same name as class with paren taking input values
	* allows for function overloading
	* a later constructor can call the previous one using *this
	* if no constructor defined, default constructor with no arguments
		* new class()
		* variables set to sensible defaults
		* int -> 0
* shallow copy vs deep copy
	* l1 = l2 will refer to the same place in memory
* can add methods to update values
	* this is a reference to current object
	* can omit this if reference is unambiguous
* Accessor and mutator methods

# L2.5 - Basic input and output in java
to print
```
System.out.println("hello, world")
# the above takes to a new line after printing

System.out.print(arg)
# this prints without a newline

System.out.printf()
# this prints a formatted output, borrowed from C
```
for input, we use console class,defined with system:
* readLine and readPassword
* readPassword does not echo characters on the screen and returns an array of char
* readLine returns a string
```
Console cons = System.console();

string username = cons.readline("User name: ");

char [] pwd = cons.readPassword("Password: ")
```
There's a more general Scanner class
* allows a more granual reading of input
* read a full line, or read an integer
```
Scanner in = new Scanner(System.in)
String name = in.nextLine()
int age = in.nextInt()
```