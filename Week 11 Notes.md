# L11.1 - Monitors in Java
## Monitors
* Monitors is like a class in an OO language
	* Data definition - to which access is restricted across thread
	* Collections of functions operating on this data - all are implicitly mutually exclusive
* Monitor guarantees mutual exclusion - if one function is active, any other function will have to wait for it to finish
* Implicit queue associated with each other
	* contains all processes waiting for access

Condition variables
* Thread suspends itself and waits for a state change - `q[source].wait()
* Separate internal queue vs external queue for initially blocked threads
* notify change
* what happens after notifying:
	* Signal and exit - notifying process immediately exits the monitor
	* Signal and wait - notifying process swaps with notified process
	* Signal and continue - notifying process keeps control till it completed and then one of the notified processes steps in

In Java
* Monitors incorporated within existing class definitions - no need to for an additional definition
* Function declared synchronized is to be executed atomically
* Each object has a lock
	* To execute a synchronized method, thread must acquire lock
	* Thread gives up lock when the method exists
	* Only one thread can have the lock at any time
* wait for lock in external queue
* wait() and notify() to suspend and resume
	* wait() - single internal queue
	* notify()
		* notify() signals one (arbitrary) waiting process
		* notifyAll() signals all waiting processes
		* Java uses signal and continue

Object locks
* Use object locks to synchronize arbitrary blocks of code
* f() and g() can start in parallel
* Only one of the threads can grab the lock for o
* Each object has its own internal queue
* can convert methods from "externally synchronized" to "internally synchronized"
* anonymous wait(), notify(), notifyall() abbreviate this.wait(), this.notify(), this.notifyall()
* Actually wait() can be interrupted by an InterruptedException
* Should write
```
try {
	wait();
}
catch (InterruptedException e) {
	...
}
```
* error to wait(), notify() and notifyall() outside synchronized methods
	* IllegalMonitorStateException

Reentrant locks
* separate ReentractLocks
* Similar to a semaphore
	* lock() is like P(S)
	* unlock() is like V(S)
* Always unlock() in finally - avoid abort while holding lock

```
public class Bank {
	private Lock bankLock = new ReentrantLock();
	
	public void transfer() {
		bankLock.lock()
		...
		try {
		
		}
		finally {
			bankLock.unlock()
		}
	}
}
```
Why reentrant?
* Thread holding lock can reacquire it 
* transfer() may call getBalance() that also locks bankLock
* Hold count increases with lock(), decreases with unlock()
* lock is available is hold count is 0

Summary:
* Can synchronize an arbitrary block of code using an object

# L11.2 - Thread in Java
Implementing runnable
* create parallel object p
* create thread t with the parallel object
* start t

Life cycle of a Java thread:
Thread can be in a six states
* New - created but not start()ed
* Runnabe - start()ed and ready to be scheduled
	* Need not be running
	* No guarantee made about how scheduling is done
	* Most java implementations use time-slicing
* Not available to run
	* blocked - waiting for a lock, unblocked when lock is granted
	* waiting - suspended by wait(), unblocked by notify() ot notifyall()
	* timed wait - within sleep() released when sleep timer expires
* Dead: thread terminates

Interrupts
* One thread can interrupt another using interrupt()
	* `p[i].interrupt()` interrupts thread `p[i]`
* Raises InterruptedException with wait(), sleep()
* No exception raised if thread is running
	* interrupt() sets a status flag
	* interrupted() checks interrupt status and clears the flag
* Detecting an interrupt while running or waiting
* Check a threads interrupt status
	* use t.isInterrupt 
	* does not clear flag
* Can give up running status
	* yield() gives up active state to another thread
	* statis method in Thread()
	* normallly, scheduling of threads is handled by OS - preemptive
	* Some mobile platforms use cooperative scheduling 
* Waiting for other thread
	* t.join() waits for t to terminate

summary
* to run in parallel need to extend Thread of implement Runnabe
	* to implement runnable frst create thread from runnable object
* t.start invokes method run

# L10.3 - Concurrent Programming - An Example
example:
* a narrow north south brudge can accommodate traffic only in on edirection at a time
* when a car arrives at a bridge
	* cars on the bridge going in the same direction - can cross
	* No other car on the bridge->can cross
	* cars on the bridge going in the opposite direction-wait for bridge to be empty
* cars waiting to cross from one side may enter bridge in any order after direction, switches in their favour
* when bridge becomes empty and cars are waiting, yet another car can enter in the opposite direction and makes them all wait some more
* Design a class Bridge to implement consistent one way access for cars on the highway 
	* should permit multiple cars to be on the bridge at one time (all in same direction)
* Bridge has public methods cross(id, d, s)
	* id - id of car
	* d - direction
	* s - time to cross

# L10.4 - Thread Safe Collection
Concurrency and collections:
* synchronize access to bank account array to ensure consistent updates
* Noninterfering updates can safely happen in parallel
	* Updates to different accounts, `accounts[i] and accounts[j]
* Insistence on sequential access affects performance
* Can we implement collections to allow such concurrent updates in a safe manner - make them thread safe

Thread safety and correctness
* Thread safety guarantees consistency of individual updates
* if two threads increment `accounts[i]` neither update is lost
* Individual updates are implemented in an atomic manner
* Does not say anything about sequences of updates
* formally, linearizability
* contrast with serializability in databases, where transactions (sequences of updates) appear atomic

Thread safe collections
* To implement thread safe collections, use locks to make local updates atomic
* Granularity of locking depends on data structure
	* In an array, sufficient to protect `a[i]
	* in a linkedlist, restrict access to nodes on either side of insert/delete
* Java provides built-in collection types that are thread safe
	* ConcurrentMap interface, implemented as ConcurrentHashMap
	* BlockingQueue, ConcurrentSkipList
	* Appropriate low level locking is done automatically to ensure consistent local updates
* Remember that these only guarantee atomicity of individual updates

Use Thread safe queue for simpler synchronization of shared objects
* Use a thread safe queue for simpler synchronization of shared object
* Producer-consumer system
	* Producer threads insert items into the queue
	* Consumer threads retreive them
* Bank account example
	* Transfer threads insert transfer instructions into shared queue
	* Update thread processes instruction from the queue, modifies bank accounts
	* Only the update thread modifies the data structure
	* No synchronization necessary

Blocking queues
* blocking queues block when
	* you try to add an element when the queue is full
	* you try to remove an element when the queue is empty
* Update thread tries to remove an item to process, waits if nothing is available
* One of these is a blocking queue
	* use a blocking queue to coordinate producers and consumers
	* ensure safe access to a shared data structure without explicit synchronization