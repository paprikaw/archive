# Collection

## Two basic type of Collections:

* Collection

A sequence of individual elements with one or more rules applied to them:
List, Queue

* Map

A group of key-value object pairs that looks up a value using a key.
One element is associated with another: (key, value) 

如何使用：you’ll typically make an object of a concrete class, upcast it to the corresponding interface, then use the interface throughout the rest of your code.

## List
In the list object, we will use equals() in the Object class to identify if the target is what we want. It’s important to be aware that List behavior changes depending on equals() behavior.


List -> ArrayList, LinkedList

ArrayList (Itself)

LinkedList -> Stack, Set, Queue

Queue was implemented by LinkedList

PriorityQueue is a individiul class


## Collection and iterator
## Iterator
Collector -> Iterator

List -> ListIterator (Only List object can use this Iterator.)


ListIterator -contains-> reverse traversal ability, set(), ListIterator() to set where to start.

### ListIterator

While Iterator can only move forward, ListIterator is bidirectional. It can produce indices of the next and previous elements relative to where the iterator is pointing in the list, and it can replace the last element it visited using the set() method.

## LinkedList

* getFirst() and element() are identical—they return the head (first element) of the list without removing it, and throw NoSuchElementException if the List is empty. peek() is a slight variation of those two that returns null if the list is empty.

* removeFirst() and remove() are also identical—they remove and return the head of the list, and throw NoSuchElementException for an empty list. poll() is a slight variation that returns null if this list is empty.

* addFirst() inserts an element at the beginning of the list.

* offer() is the same as add() and addLast(). They all add an element to thetail (end) of a list.

* removeLast() removes and returns the last element of the list.


LinkedList -contains-> getFirst() | element() | peek (), removeFirst() | remove() | poll(), addFirst(), offer() | add() | addLast(), removeLast()



