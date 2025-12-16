# L7.1 - Dealing with Errors
## exception handling
* code that generates error raises or throws an exception
* Notify the type of error
	* Information about the nature of the exception
	* Natural to structure an exception as an aobject
* Caller catches the exception and takes corrective action
	* extract information about the error from the exception object
	* graceful interruption rather than program crash
* or passes the exception back up the calling chain
* Declare if a method can throw an exception
	* Compiler can check whether calling code has made a provision to handle the exception

## Classification of errors
* All exceptions descend from class Throwable
	* two branched, Error and Exception
* Error - rare, not programmers fault
	* Internal errors, resource limitations within java runtime
	* no realistic corrective action possible, notif ycaller adn terminate gracefully
* Exception - two sub branched
	* runtime and checked
* Runtime - should have been caught by code
* checked
	* typically user defined
		* in a list of order, quantities should be positive integers

# L7.2 - Exceptions in Java
## Catching and Handling exceptions
* try-catch
	* enclose code that my generate exception in try block
	* exception handler in catch block
* If try encounters an exception, rest of the code is skipped
* exception matches type in catch, handler
* uncaught exception is passed back to the code that called this code
* top level uncaught exception - program crash
* Can catch more than one type of exception
	* Multiple catch blocks
* Catch blocks are tried in sequence
	* match exception type again each one in turn
* order block by type, more specific to less specific

When does a function generate an exception
* Error = JVM rntime issue
* RunTimeException
* code calls another function that generates an exception

Notifying checked exceptions
* define exception
* search java docmentation for suitable pre defined exception
* create an object of exception type and throw it

How does caller know that that exact exception is generated
* Declare exceptions throw in header
* Can throw multiple types of exceptions
* Can throw any subtype of declared exception type

* Method declares the exceptions it throw
* if call such a method, must handle
* or pass it, your method should advertise that it throws the same exception
* Need not advertise
* Should not normally gneerate runtime exceptions

More on catching
* Can extract information
	* `catch e { e.getMessage() }`
* Chaining exceptions
	* process and throw a new exception from catch
* Throwable has additional methods to track chain of exceptions
	* getCause(), initCause()
* Add information when you chain exceptions

Cleaning up resources
* may need to clean up some files deallocate resources
* Add a block labelled `finally

# L7.3 - Packages
Java has an organizational unit called package - so names don't overlap
* Can use import to use packages directly
* can create out own hierarchy of packages
* add a package header to include a class in a package
* by default all classes in a directory belong to the same anonymous package
* default visibility is public within the package
	* applies to both methods and variables
* Can also restrict visibility with respect to inheritance hierarchy
	* protected means visible within subtree, so all subclasses
	* Normally a subclass cannot explain visibility of a function
	* however protected in higher class can become public in subclass

# L7.4 - Assertions
* functions may have constrains on the parameters
* We could check the condition and throw an exception
* What if function is only used internally by our own code
	* flag errors during development - debugging
	* but diagnositc code should not trigger at runrime
	* performance lost and affected
* Instead "assert" the property you assume to hold
* If assertion fails, code throws `AssertionError`
* This should not be caught
	* abort and print diagnostic information (stack trace
* Can provide additional information to be printed with diagnostic message

```
public static double function() {
	assert x > 0: x;
	// returns x if asserterror
}
```
* Assertions are enabled or disabled at runtime
	* does not require recompilation
* Use the following flag to run with assertions enabled
	* `java -enableassertions mycode`
* can use -ea as abbreviation 
* Can selectively turn on assertions for a class or a package
	* `java -ea:MyClass MyCode`
* Can also disable for a particular class using -da

# L7.5 - Logging
* typical to generate message within code for diagnosis
* Naive approach is to use print statements
	* Need to add/subtract as we go along
	* enable and disable explicitly
* Instead log diagnostic messages separately
	* Logs are arranged hierarchically - choose the level of logging needed
	* can be displayed in different formats
	* Logs can be processed by other code - handlers
		* can filter out uninteresting entries
	* Logging controlled by a configuration file

Calling
* call inf() method of global logger:
	* `Logger.getGlobal().info("somestring")`
* Supress logging by executing the following code
	* `Logger.getGlobal().setLevel(Level.OFF)`
* Create a custom logger
	* `private static final Logger myLogger = Logger.getLogger("sjsjsj/")`
		* Logger names are hierarchical, like package names
		* setting property for in.ac.iitm, automatically sets it for in.ac.iitm.onlinedegree

Logging level - 7
* SEVERE, WAGNING, INFO, CONFIG, FINE, FINER, FINEST
* By default, first three levels are logged
* can set a different level `logger.setlevel(level.fine)`
* turn on all levels or off  `setlevel(level.all or level.off)`