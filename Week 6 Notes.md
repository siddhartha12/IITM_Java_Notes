# L6.1 - The benefits of Indirection
Abstract data types
* Separate public interface from private implementation
* For instance, a (generic) queue
* Concrete implementation could be a circular array
* Or a linked list
* Implemented of class `Queue`can choose either one
* Public interface is unchanged

Is the user indifferent to choice of implementation
* Interface does not capture other aspects
* Efficiency
	* Circular array is better - one time storage allocation
* Flexibility
	* Linked list is better - circular array has bounded size
* Offer user a choice (Multiple Implementations)
* User chooses
```
public class CircularArrayQueue<E> {
	public void add(E element) { ... };
	public E remove() { ... };
	public int size() { ... };
}

public class LinkedListQueue<E> {
	public void add (E element) { ... };
	public E remove() { ... };
	public int size() { ... };
}

circularArrayQueue<Date> dateq;
LinkedListQueue<String> stringq;

dateq = new CircularArrayQueue<Date>();
stringq = new LinkedListQueue<String>();
```

### Adding Indirection
```
public interface Queue<E> {
	abstract void add (E element);
	abstract E remove();
	abstract int size();
}

public class CircularArrayQuque<E> implements Queue<E> { .. };

public class LinkedListQueue<E> implements Queue<E> { .. };

// Use interface to declare variables

Queue<Date> dateq;
Queue<String> stringq;

dateq=new CircularArrayQueue<Date>();
stringq=nwe LinkedListQueue<String>();
```

Benefit of indirection: to use a different implementation for dateq only need to update the instantiation

Interfaces add a level of indirection. we are postponing the decision as late as possible

"Fundamental theorem of software engineering": All problems in computer science can be solved by another level of indirection

# L6.2 - Collection
Built in data types
* Most programming languages provide built in collective data types
	* arrays, lists, dictionaries
* Java originally had many such pre-defined classes
	* Vector, Stack, Hashtable, Bitset
* Normally choose the one you need
* but changing a choice requires multiple updates
* Instead, organize these dat structures by functionality
* Create a hierarchy of abstract interfaces and concrete implementations
	* provide a level of indirection

Collection interface
* Collection interface abstracts properties of grouped data
	* arrays, lists, sets,
	* but not key-value structures like dictionaries
* add() add to the collection
* iterator() - get an object that implements iterator interface
* use iterator to loop through the elements
* Java later added "for each" loop
	* Implicitly creates an iterator and run through it
	  `for (String element : cstr) { ... }`
example
```
for (E element : c){
	if (element.equals(obj)){ # how does this line work?
		return true;
	}
}
```
Removing elements
* Iterator also has a remove() method
	* which element does it remove?
* The element that was last accessed using next()

`if (element.equals(obj)){ # how does this line work?`
* Actually, `collection` defines a much larger set of abstract methods
	* addAll(from) - adds elements from a compatible collection
	* removeAll(c) - remove elements present in c
	* a different remove() from the one in Iterator
	* ContainsAll - sees is one set is a superset or subset
* To implement collection, need to implement all these methods
* "Correct solution" = provide default implementations in the interface
* Added to Java interfaces later!
* Instead AbstractCollection abstrct class implements Collection
* Concrete classes now extend "AbstractCollection"
	* Need to define iterator() based on internal representation
	* Can choose to override contains()

# L6.3 - Concrete Collections
Builtin types
* collection abstracts properties of grouped data
	* not key-value structures
* Collections can further be organizer on additional properties
* In spirit of indirection, 
	* List - ordered collections
	* set - no duplicates
	* queue - ordered collections

## List interface
* can accessed in two ways
	* through an iterator
	* by position - random access
* Additional functions for random access
* ListIterator extends Iterator
	* `void add(E element)` to insert an element before the current index
	* `void previous()` to go to previous element
	* boolean `hasPrevious()` checks that it is legal to go backwards
* Random access is not equally efficient for all ordered collections
	* In an array, can compute location of element at index i
	* in linked list, must start at beginning and transe i links
* Tagging interface random access
	* tells us whether a list supports randoma ccess or not
	* can choose algorithm based on this
	* `is (c instanceof RandomAccess)`
## AbstractList interface
* `AbstractCollection` is the "usable" version of `Collection
* Correspondingly `AbstractList` extends `AbstractCollection`
* `AbstractSequentialList` extends `AbstractList
	* further subclass to distinguish lists without random access
* Concrete generic class `LinkedList<E>` extends `AbstractSequentialList
	* Internally, the usual flexible list
	* Efficient to add and remove element at random positions
* Concrete generic `ArrayList<E>` extends `AbstractList`

Using concrete list class
* concrete generic class `linkedlist<E>` extends `AbstractSequentialList`
	* Not random access
	* But random access methods of AbstractLIst are still available
	* Loop will execute a fresh scan from start to element i
* Two version of add() available
	* add() from collection appends to end of list
	* add() from `ListIterator` inserts a value before current position of iterator
* In collection, add() returns boolean - did the add update the collection?
	* add() may not update a set, always works for lists
	* add() in `ListIterator` returns void

## Set interface
* Set is a collection without duplicates
* Set interface is identical to collection but behaviour is more constrained
	* add() should have no effect
	* equals() should return true if contents match regardless of the order
* Use Set to constrain values to satisfy
* Set implementation typically designed to allow efficient membership tests
* Ordered collections loop through a sequence to find an element
* Instead, map the value to its positoin
	* Hash function
* or arrange values in a two dimensional structure
	* Balance search tree (binary search)
* Concrete set implementations extend `AbstractSet`

## Concrete sets
* HashSet implements a hash table
	* underlying storage is an array
	* map value v to a position h(v)
	* is h(v) is unoccupied, store at that position
	* otherwise different strategies to handle this case
* checking membership is fast - check is v is at position
* Undordered but supports iterator()
* scan elements in an unspecified order
* visit each element exactly once

## TreeSet
* TreeSet uses a tree representation
	* Values are ordered
	* Maintains a sorted collection
* Iterator will visit elements in sorted order
* Insertion is more complex than a hash table

## Queue
* Ordered, remove front insert rear
* Queue interface supports
	* `boolean add(E element); 
	* `E remove();`
		* If queue full, add() flags an error
		* If queue empty remove() flags an error
* Gentler versions
	* `boolean offer(E element);`
	* `E poll()
		* return false or null if not possible
* Inspect the head, no update
	* `E element()` // throws exception
	* `E peek()` // returns null

## Deque
same as above but there is first and last for every function
peekfirst, peeklast

## Priority Queue
* remove() returns highest priority item


# L6.4 - Maps
Key-value structures come under the Map interface
* two type parameters
* K is the type for keys
* V is the type for values
* get(k) fetches value for key k
	* parameters here in code, the k is of type Object
	* return is of type V generic
* put(k,v) updates value v for key k
	* of type K and V using generics
* As expcted keys form a set
	* Only one entry per key-value
	* Assigning a fresh value to existing key overwrite the old value
	* put(k,v) returns the previous value associated with k, or null

## Updating a Map
* Key-value stores are useful to accumulate quantities
	* Frequencies of words in a text
	* Total runs ina  tournament
* Initialization problem
	* update the value if the key exists
	* otherwise, create a new entry
* Map as a following default method
	* V getOrDefault( Object key, V defaultValue)
	* Returns default value if key doesn't exist
* for updating we can do the following
```
scores.put(bat,
	scores.getOrDefault(bat, 0)+newscore)
```
basically if batsman doesn't exist, it returns the 0 and gets put, if he is there, then add to his previous score
* Alternatively, use `putIfAbsent()` to initialize a missing key
* or use `merge()` - dict.merge(bat, newscore, Integer::sum)

Extracting keys and values
* Methods to extract keys and values
* Keys for a set while values form an abritrary collection
* key-value pairs form a set over a special map entry type
* Java calls these views
* Can now iterate through a map
	* get set of keys
	* for key in keys, do something
* use entrySet() to operate on key and associated value without looking up map again

## HashMap
* Similar to Hashset
* Use a hash table to store keys and values
* No fixed order over keys returned by keyset()

## TreeMap
* Similar to tree
* Use a balanced search tree to store keys and values
* Iterator over keySet() will process keys in sorted order

## LinkedHashMap
* Remembers the order in which keys were inserted
* Hash table entires are also connected as souble linked list
* Iterators over both keySet() and value() enumerate in order of insertion
* Can also use access order
	* each get() or put() moves key-value pair to end of list
	* Process entries in least recently used ordering - scheduling caching