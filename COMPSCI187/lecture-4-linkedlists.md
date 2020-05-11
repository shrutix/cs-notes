# Linked Lists

Class: CS 187
Created: Feb 11, 2020 11:36 AM
Reviewed: Yes
Test?: Unit 1
Type: Lecture

1. The Linked List Data Structure
2. Arrays vs Linked Lists
3. The LLStringNode Class
4. Operations on a Linked List
5. A linked list implementation of StringLog

### Linked List

- A series of **nodes** chained together
- Each node contains a **data element** and a **link** (i.e. pointer) to the next node in the chain
- If we know the head of the chain, we can follow successive links to reach any node on the chain
- The last node has a **null** pointer, because there is no node following it

### Arrays vs Linked Lists

- Array: store all data elements **consecutively in memory** –> easy to access any element **using an index** ([0], [1], [2]...)
- Linked list: each element is separate in its **own block of memory** (node), so no easy way to directly access an element using index
- Array: **fixed size**; if not filled, it wastes a lot of memeory
- Linked list: has a truly **dynamic size**
- Linked list: faster for certain operatons (ex. inserting or deleting elements at front)

### Benefits: Size/Space

- Size of an array is fixed (bounded)
- Ex. char[] arr = new char[6]
- Size of a linked list is not fixed (unbounded)
- Ex. allocate node "A" (size 1), allocate node "B" (size 2), allocate node "C" (size 3)...
- Linked list needed for many applciations where you can't predict, ahread of time, how many elements you will end up storing

### Benefits: Front Insertion

- To insert new element "X" into front of an **array** –> shift all elements to make room at [0] index; the more exisiting elements there are, the mroe steps it takes
- To insert new element "X" into front of a **linked list** –> create new node and adjust the reference

### The *LLStringNode* Class
```java
    public class LLStringNode {
    	private String info; // ––> Data
    	private LLStringNode link; // ––> Reference to next node
    }
```
- ^**Self-referential** –> a class containing a reference to an object of the same class
    - How much space does the compiler allocate to the link variable?
- In Java, a class-type variable is a **reference** (i.e. pointer) to an object; does **not** contain actual content of the object (*LLStringNode* in *link* variable above)
- So *link* variable merely stores a **memory address**; size of the memory address is typically:
    - 4 bytes on 32-bit JVM
    - 8 bytes on 64-bit JVM
    - (Regardless of what it is pointing to, i.e. *String*, *Object*, *LLStringNode*, etc)

```java
    public class LLStringNode {
        private String info;
        private LLStringNode link;
    
        public LLStringNode(String info) {
            this.info = info;
            link = null;
        }
        public String getInfo() {
            return info;
        }
        public LLStringNode getLink() {
            return link;
        }
        public void setInfo(String i) {
            info = i;
        }
        public void setLink(LLStringNode link) {
            this.link = link;
        }
```    

- Alternatively, can make data members (private variables) public –> can access *info* and *link* directly without any methods (no need to setters and getters)

### Using LLStringNode

- Create the **first** node:
```java
    LLStringNode sNode1 = new LLStringNode("basketball");
```
- Create the **second** node:
```java
    LLStringNode sNode2 = new LLStringNode("baseball");
```
- Add it to the chain:
```java
    sNode.setLink(sNode2);
```
### Traverse a Linked List

- Start from the first node (head), follow chain, and make sure to not run off the end of the list
```java
    LLStringNode node = head;
    while (node != null) {
    	System.out.println(node.getInfo());
    	node = node.getLink(); }
```
- Would this still work if the list is empty? (i.e. *head* is *null*) <– edge case to consider

### Arrays vs Linked List

- Traversing linked list vs traversing array (pretty similar):
```java
    LLStringNode node = head;
    while (node != null) {
    	System.out.println(node.getInfo());
    	node = node.getLink();
    }

    int i = 0;
    while (i != arr.length) { // or < arr.length
    	System.out.println(arr[i]);
    	i++;
    }
```
### Linked List Front Insertion

- *newNode* to insert at front of linked list –> hook to chain, make it first node (head)
```java
    newNode.setLink(head)
    head = newNode;
```
- What happens if insertion is called when linked list is empty (i.e. head == null)? Any problems? A: This is ok
- What happens if we swap the two lines of code in front insertion^? A: Does not work

### Linked List Implementation of *StringLog*

```java
    public class LinkedStringLog implements StringLogInterface {
    	private LLStringNode log; // head
    	public LinkedStringLog(); // constructor
    
    	// rest are interface methods (below)
    	public void insert(String e);
    	public boolean isFull();
    	public int size();
    	public boolean contains(String e);
    	public void clear();
    	public String getName();
    	public String toString();
    }
```

### Implement *insert()* method

- *StringLog* description did not specify whether the strings needs to be stored in a particular order –> can use front insertion of linked list
```java
    public void insert(String element) {
    	LLStringNode newNode = new LLStringNode(element);
    	newNode.setLink(log);
    	log = newNode;
    }
```
### Observers

- *getName()* is the same as ArrayStringLog
- *isFull()* is even simpler –> always returns false (since linked list capacity is unbounded)
```java
    public String getName() {
    	return name;
    }
    public boolean isFull() {
    	return false;
    }
```
- For *size()*, one way is tor traverse the linked list to count the number of nodes in the list
```java
    public int size() {
    	int count = 0;
    	LLStringNode node = log;
    	while (node != null) {
    		count++;
    		node = node.getLink();
    	}
    	return count;
    }
```
- Simpler way is to define an interger variable to track the number of elements exisiting in the list –> *insert()* will increment it, *clear()* will reset it, and *size()* returns the value of it

### *contains()* method

- Need to traverse the list
- Can terminate the loop as soon as the string that we are searching for is found
```java
    public boolean contains(String element) {
    	LLStringNode node = log;
    	while (node != null) {
    		if (element.equalsIgnoreCase(node.getInfo()) {
    			return true;
    		} else {
    			node = node.getLink(); 
    		}
    	}
    	return false;
    }
```
### *toString()* method

```java
    public String toString() {
    	String logString = "Log: " + name + "\n\n";
    	LLStringNode node = log;
    	int count = 0;
    	while (node != null) {
    		count++;
    		logString = logString + count + ". " + node.getInfo() + "\n";
    		node = node.getLink();
    	}
    	return logString;
    }
```

### End Insertion

- An issue w/ **front insertion** is when traversing the linked list, elements will be in **reverse order** as they are inserted –> *toString*() will print elements in reverse order
- Solve by modifying the *insert()* method to always **insert at end** of list
- How to implement **end insertion** with most efficient solution?
- Keep a ***tail*** pointer that points to the last node in the list (akin to *lastIndex* in *ArrayLog* case)
- To insert a new node:
    1. Chain the new node to end of the list
    2. Update the tail pointer

```java
        tail.setLink(newNode); // 1
        tail = newNode; // 2
```
```java
    public class LinkedStringLog implements StringLogInterface {
    	LLStringNode log, tail; // stores both head and tail
    	public void insert(String element) { // end insertion
    		LLStringNode newNode = new LLStringNode(element);
    		if (tail == null) { // if list is empty
    			log = newNode;
    		} else {
    /* 1 */tail.setLink(newNode);
    		}
    /* 2 */tail = newNode; // tail always points to teh new node
    	}
    }
```
- What are ***head*** and ***tail*** pointing to when the list has exactly one element?