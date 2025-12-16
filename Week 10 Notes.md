# L10.1 - Concurrency - Threads and Processes
Multiprocessing
* single processor executes several computations in a pseudo parallel - so fat it feels paralle
* time slicing to share access
Logically parallel actions within a single application
* Clicking Stop terminates a download in a browder
* user-interface is running in parallel with network access

* Process
	* Private set of local variables
	* time slicing involves saving the state of one process and loading the suspended state of another
* Threads
	* Operated on same local variables
	* communicates via shared memoty
	* context switches are eaiser
* Process and threads used interchangable

Creating threads in java
* Have a class extend Thread
  `public class Parallel extends Thread{ public void run()}
* define a function run() where execution can begin in parallel
* Invoking `p[i].start()` initiates the `p[i].run()` in a separate thread - runs the process parallely
	* directly calling `p[i].run()` does not execute in a parallel thread
* sleep(t) suspends thread for t milliseconds
	* static function - use if current class does not extend thread
	* throw interruptedexception later
* Cannot always extend thread - single inheritance
* Instead implement Runnable
	* to use a runnable class, explicitly create a Thread and start() it

# L10.2 - Race Conditions
in threads, watch out for race conditions - shared variables must be updated continuously

maintaining data consistency
* `double accounts[]
* two functions that operate on accounts - transfer() and audit()
* what is transaction and audit is run parallel (audit here is under the assumption that total money in the bank does not change)
* the total can show + or - transaction amount
	* depends on how actions of transfer are interleaced with actions of audit
	* can even report is transaction was atomic

race conditions
* race conditions - concurrent update of shared variables, unpredictable outcome
	* executing transfer() and audit() concurrently can cause audit() to report more or less than the actual assets
* avoid this by insisting that transfer() and audit() do not interleave
* Never simultaneously have current control point of one thread within transfer() and another thread within audit()
* Mutually exclusive access to critical regions of code

# L10.3 - Mutual Exclusion
Using a shared turn variable that both programs can switch to activate each other after they run
* mutually exclusive access is guaranteed
* but one thread permanently locked out if other thread shuts down
	* starvation

Using a boolean for their own process request
* Mutually exclusive access is guaranteed
* but if both threads try simultaneously, they block each other
	* deadlock

Using both![[Pasted image 20251217035359.png]]

petersons solution for more than 2 processes is not trivial. For n process mutual exclusion other solutions exist

Lamport's bakery algorithm
* Each new process picks up a token that is larger than all waiting processes
* Lowest token number gets served next
* Still need to break ties - token counter is not atomic

# L10.4 - Test and Set
* fundamental issue preventing consistent concurrent updates of shared variables is test-and-set
* to increment a counter, check its current value, then add 1
* if more than one thread does this in parallel, updates may overlap and get lost
* Need to combine test and set into an atomic, indivisible step

Semaphores
P(S) - check and start
Critical code
V(S) - release

Problems with semaphores
* too Low level
* no clear relationship between a semaphore and the critical region that it protects 
* all threads must cooperate to correctly reset semaphore
* cannot enforce that each P(S) has a matching V(S)
* Can even execute V(S) without a P(S)