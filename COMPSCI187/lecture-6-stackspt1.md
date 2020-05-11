# Stacks (part 1)

Class: CS 187
Created: Feb 06, 2020 11:42 AM
Reviewed: Yes
Test?: Unit 1
Type: Lecture

### What is a Stack?

- A **stack** stores a collection of objects with following restricted access:
    - **push** a new object (to top of stack)
    - **pop** top element of the stack (removes from stack)
    - **peek** to look at top element without removing it
    - can **only operate top element**, there is no way to access other elements in the stack
- Pop always removes the last element pushed into stack –> called **LIFO (last-in-first-out)**

### Why do we need stack?

- great data structure for computer systems to manage method calls and returns
- imagine you started cleaning your house (**task A**), but realized to do so you need to buy cleaning supplies (**task B**); to buy supplies you need to get cash first (**task C**); but your car is broken and you have to fix it first (**task D**); think about the order these tasks come up and the order they are done

### Underflow and Overflow

- popping from an empty stack causes an error condition called **stack underflow exception**; before we pop, we need to check and make sure stack is not empty, otherwise will disrupt normal program execution
- some stacks are **bounded**, having a fixed capacity; pushing onto full (at bound) stack causes **stack overflow exception**, so before every push we need to check that stack is not full (has available spots)

### Exceptions and Error Handling

- underflow and overflow conditions that break normal program execution require special processing ––> **exceptions**
- don't want program to crash/terminate when exceptions happen
- Java provides ways for handling exceptions through **Exception** class, **throw** statement, and **try-catch-finally** statements
- Method can **throw** an **Exception** object (which carries information about exception) when it detects an error condition; program flow will jump to exception handling
```java
    public void pop() {
    	throw new Exception("Stack underflow!");
    	// code below this is skipped if exception is thrown above
    }
```
- can also extend the **Exception** class to define a customized exception class
- ex. Java's **IOException** extends the **Exception** class, most I/O related method require handling **IOException**
- caller method can use a **try-catch statement** to handle error condition and process accordingly
```java
    try {
    	// call some method, e. pop()
    } catch (IOException e) {
    	// handle IOException error
    } catch(Exception e) {
    	System.out.println(e.getMessage());
    } finally {
    	// this block always executes
    	// generally for clean-up work
    }
```
### Checked Exceptions

- The **Exception** class itself, and any class extending are called a **checked** exception –> compiler insists that it is told when a method may throw such exceptions
- Methods throwing exceptions must be explicitly handled –> callers of such method must either wrap method in **try-catch** statement, or use **throws** clause to defer the handling further
- Common checked exception –> **IOException**
```java
    class Nested {
    	public static void checkInteger(int i) throws Exception { // 2. So it is specified here
    		if (i < 0) 
    			throw new Exception("Negative Number!!\n"); // 1. Exception can be raised here
    		System.out.println("Check passed.");
    	}
    	public static void main(String[] args) {
    		try {
    			checkInteger(-100);
    		}
    		catch(Exception e) { // 3. Caught and handled here
    			System.out.print( e.getMessage() );
    		}
    		System.out.println("Execution complete.");
    	}
    }
    // What is the output when executed? Negative number, exacution complete.
```
### Unchecked Exceptions

- **Unchecked exceptions** –> classes extending Java's **RuntimeException** class
- Methods that throw such exception do *not* need to declare them in signature and callers are *not* required to try-catch
- Common uncheck exceptions –> **NullPointerException**, **IndexOutOfBoundException**, **ClassCastException**, **ArithmeticException**
- **RuntimeException** itself is subclass of **Exception**

### The Stack Interface
```java
    public interface StackInterface<T> {
    	void push(T element) throws StackOverflowException;
    	T pop() throws StackUnderflowException;
    	T peek() throws StackUnderflowException;
    	boolean isEmpty();
    	boolean isFull();
    } 
```
- Methods that throw non-checked exceptions can **optionally** declare exceptions using the **throws** clause
- Assume these exceptions are runtime exceptions
- T ****is a generic type

### Generics

- Java (and other languages) have a mechanism called **generics** to create entire families of classes (or interfaces) at once, where the object **type** is provided as a **parameter** to the class (or interface) definition
- Each different type variable gives new class (or interface)
- C++ –> generics are called **templates**

### Generic Classes

- Define a **generic Log** class that can be a ***StringLog***, but also be an ***IntegerLog*** or other types of logs (T represents generic type):
```java
    public class ArrayLog<T> {
    	private T[] log;
    	private int lastIndex = -1;
    } // "template" to create classes
```
- When using the class, provide specific type T inside angle brackets <>
```java
    Log<Integer> IntLog = new Log<Integer>();
    Log<String> StrLog = new Log<String>();
```
- **Cannot** use primitive data types (int, float, etc); have to use their **wrapper classes** (Integer, Float, etc)
- **Conceptually*** when the compiler sees **Log<Integer>**, basically creates a new class (i.e LogInteger) and substitutes every T in the 'template' with **Integer** (how generics works in C++ but diff for Java)
- Similarily, when it sees ***Log<String>***, basically creates a new class ***LogString*** and substitues every ***T*** in the 'template' with ***String***
```java
    public class LogString {
    	private String []Log;
    	private int lastIndex = -1;
    	public void insert(String e);
    } // same format for Integer wrapper class
```
- Greatly reduces programmer's work to reuse sam data structure but just changes data type

### Linked Stack Implementation

- To implement a stack using linked list, first define **generic node clase *LLNode<T>***; its info variable points to an object of generic type T, and its link variable points to another *LLNode<T>* object
```java
    public class LLNode<T> {
    	public LLNode<T> link;
    	public T info;
    	
    	public LLNode(T info) {
    		this.info = info;
    	}
    }
```
- Declared both link and info as public so any class can directly access and modify them w/o setters and getters
- Note that ***LLNode*** and ***<T>*** must go together (i.e. can't write ***LLNode*** w/o ***<T>***), **except the constructor**!

### LinkedStack<T> (Generic Stack)

- Only variable needed is a pointer to **top** of the stack; and use **front insertion** when pushing elements
```java
    public interface StackInterface<T> {
    	void push(T element) throws StackOverflowException;
    	T pop() throws StackUnderflowException;
    	T peek() throws StackUnderflowException;
    	boolean isEmpty();
    	boolean isFull();
    }

    public class LinkedStack<T> implements StackInterface<T> {
    	private LLNode<T> top;
    	public LinkedStack() {
    		top = null;
    	}
    }
```
### LinkedStack Methods
```java
    public boolean isEmpty() {
    	return (top == null);
    }
    
    public T peek() {
    	if (isEmpty())
    		throw new StackUnderflowException("underflow");
    	return top.info;
    }
```
- ^Note that ***top*** is a node –> need to return its content (*top.getInfo()*) rather than the node itself
- ***push()*** method accepts a type T object, creates a new node, and adds that to the linked list
    - New node will be inserted at beginning of the linked list (i.e front insertion)
    - Here push() will never throw an exception, why?

### push() and pop()
```java
    public void push (T element) {
    	LLNode<T> newNode = new LLNode<T>(element);
    	newNode.link = top;
    	top = newNode;
    }
```
```java  
    public T pop() {
    	if (isEmpty())
    		throw new StackUnderflowException("underflow");
    	T element = top.info;
    	top = top.link;
    	return element;
    }
```
### Iterator

- **Iterator** –> object that allows you to traverse the elements in a collection one-by-one, regardless of how the collection is implemented
- Java's ***iterator<T>*** interface has three methods:
    - **hasNext()**: returns true if the collection has more elements to traverse, false otherwise
    - **next()**: returns the next element; to get all elements, call this repeatedly; if there is no more element (*hasNext()* returns *false*), this method throws *NoSuchElementException* (unchecked)
    - **remove()**: removes the last returned element

### Iterator<T> and Iterable<T>

- Ex. (let's say object **list** stores a collection of **type T** objects, doesn't matter how data is stored internally)
```java
    Iterator<T> iter = list iterator();
    while (iter.hasNext()) {
    	T element = iter.next();
    }
```
### Putting it Together

- I.e. *List<T>* is a generic class that stores a collection of type T objects, and it implements the *Iterable<T>* interface:
```java
    class List<T> implements Iterable<T> {
    	public Iterator<T> iterator() {
    		return new ListIterator<T>(...);
    	}
    	// variables, constructors, other methods
    }
```
- ^*iterator()* above returns a *ListIterator<T>* object, which implements the *Iterator<T>* interface:
```java
    class ListIterator<T> implements Iterator<T> {
    	public boolean hasNext() {...}
    	public T next() {...}
    	public void remove() {...}
    	// variables, constructors, other methods
    }
```
- Here *ListIterator<T>* is aware of the specific implementation details of *List<T>* and provides the above methods to traverse the collection of objects
- Reference to *List<T>* object is typically passed in through the constructor
```java
    List<String> list = new List<String>();
    ... ...
    Iterator<String> ite = list.iterator();
    // ite is of class ListIterator<String>
    while (iter.hasNext()) {
    	String e = iter.next();
    	... ...
    }
```
- (below) easier way (special for loop) to traverse the list:
```java
    for (String e : list) {
    	... ...
    }
```
### Summary
```java
    List<String> list = new List<String>();
    for (String e : list) { ... }
```
- A class (ex. List) can implement ***Iterable<T>* interface**; –> must implement the *iterator()* method that returns an *iterator* object
- Iterator class implements the ***Iterator<T>* interface**; must provide *hasNext()*, *next()*, and *remove()* methods
- To traverse elements in an iterable object, use the special loop (abpve)