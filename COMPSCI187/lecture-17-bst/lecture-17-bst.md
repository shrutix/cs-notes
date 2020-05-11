# Binary Search Trees

Class: CS 187
Created: Apr 06, 2020 7:03 PM
Materials: Binary%20Search%20Trees/Lec17BST.pdf
Reviewed: No
Test?: Unit 2
Type: Lecture

- **Binary Search Tree (BST)** → A binary tree where at *any node*, its value is
    - **greater than or equal to** the value of any node in its left subtree, and
    - **less than** the value of any node in its right subtree
- Can think of any node as a 'pivot': its entire left subtree is smaller or equal to its value and entire right subtree is larger → this property must be true for every node
- The BST conditions have to be true for **every node** in the tree, not just the root!

### In-Order Traversal of BST

- Work out the **in-order traversal** results of the following two valid BSTs:

![Binary%20Search%20Trees/Screen_Shot_2020-04-07_at_10.44.53_PM.png](Binary%20Search%20Trees/Screen_Shot_2020-04-07_at_10.44.53_PM.png)

- For both, **in-order traversal** gives the same result: `1, 5, 10, 15, 20, 30` . this is sorted!
- For a **BST**, **in-order traversal** visits every node in ascending (more precisely, non-decreasing) order
    - Makes sense because with in-order traversal, you visit (ex. print out) the entire left-subtree first, then the current node (root node in above BSTs) , and then the entire right-subtree. due to the properties of BST, this ends up visiting all nodes in **ascending order**
- What about pre-order and post-order traversals of BST?

### Hey! these are all different things! do not confuse

- Binary Search → an algorithm on a sorted array
- Binary Tree → a tree where nodes have no more than two children
- Binary Search Tree (BST) → a binary tree with a special ordering property

### Search in a BST

- However, b**inary search** and **BST** are related, because the way you search in a BST is similar to performing a binary search in an ordered way

![Binary%20Search%20Trees/Screen_Shot_2020-04-07_at_10.57.48_PM.png](Binary%20Search%20Trees/Screen_Shot_2020-04-07_at_10.57.48_PM.png)

- Start from the root node, then choose to go left or right, depending on the comparison result. the search ends when either you've found the target or you've reached a lead
- Maximum number of steps is the **tree height**
- As in binary search, search in BST can achieve `O(log N)` time. however this requires the BST to be balanced (i.e. height should be small)
- If you have a poorly constructed BST (ex. degenerated to a linked list), you won't get the `O(log N)` performance!

### BST Properties

- The **largest** element in a BST is in the **right-most node** (i.e. starting from the root, follow the right child link all the way until you can't go further)
- Similarily, the **smallest** element in a BST is the **left-most node** (i.e. starting from the root, following the left child link all the way until you cannot go further)
- For a **node that has two children**, its **in-order predecessor** is the largest element (i.e. right-most node) in its left subtree. its **successor** is the smallest (i.e. left-mode node) in its right subtree
- More accuratey: predecessor is the elemt that comes just before the given node in an in-order traversal. successor is the element that comes just after the given node in an in-order traversal

### BSTNode Class
```java
    public class BSTNode<T extends Comparable<T>> {
    	protected T info; // info stored in a node
    	protected BSTNode<T> left; // link to the left child
    	protected BSTNode<T>; // link to the right child
    	
    	public BSTNode(T info) {
    		this.info = info;
    		left = null;
    		right = null;
    	} // plus the standard getters and setters for info, left, and right
    } 
```
```console
         left          info          right
    –––––––––––––––––––––––––––––––––––––––––––
    |      *      |      *      |      *      |
    –––––––––––––––––––––––––––––––––––––––––––
           |             |             |
           v             v             v
    another tree     comparable   another tree
        node          object         node
```
### Basic Operations of BSTs

- `add(elem)` → insert a new node to BST. must maintain BST ordering property
- `remove(elem)` → remove node containing elem. must maintain BST ordering property
- `contains(elem)` → return true if tree contains a node whose info equals elem. exploit BST ordering property
- `get(elem)` → find a tree node with info matching elem,return a reference to it, otherwise return null. exploit BST ordering property
- `size()` → return count of nodes in BST. return count of nodes in BST

### BinarySearchTree Class
```java
    public class BinarySearchTree<T extends Comparable<T>> implements BSTInterface<T> {
    	protected BSTNode<T> root; // pointer to the root node
    	
    	public void add(T element) {...}
    	public boolean remove(T element(...}
    	public boolean contains(T element) {...}
    	public T get(T element) {...}
    	public int size() {...}
    }
```
### Creation and Maintenance of BST

- All modifying operations **must maintain the ordering constraint** of the BST
    - `add(elem)` → insert new node to BST
    - `remove(elem)` → remove node containing elem

### Inserting to an Empty BST

- `5, 9, 7, 3, 8, 12`

![Binary%20Search%20Trees/Screen_Shot_2020-04-07_at_11.36.19_PM.png](Binary%20Search%20Trees/Screen_Shot_2020-04-07_at_11.36.19_PM.png)

### Summary of Insertion

- First, find teh node to insert the new element to. this is much the same process as trying to find an element that turns out not to exist
- Once you have found the node, **insert** the new element as either its left child or right child, depending on the comparison result
- Not that **the new element is always inserted into BST as a *leaf* *node***!

Insertion order will determine the shape of BST  → some lead to more blanced trees, some do not (see slides for ex)

### The Remove Operation

- `remove(elem)` → remove node containing elem
- The most complicated in BST operation
- Must ensure the BST property is preserved when removing an element
- (Need to undertsand it conceptually, such that given a BST and a node to remove, you can draw the resulting BST after removal)

### Three Cases for `remove()`

- Easy → **Removing a leaf (no children)**: removing a leaf is simply a matter of setting the appropriate link of its parent to null
- OK → **Removing a node with only one child**: make the reference from the parent skip over the removed node and point instead to the child of the node we intend to remove
- Tricky → **Removing a node with two children**: replaces the node's info with the info from another node in the tree so that the search property is retained – then remove this other node

### Removal Cases

- Case 1 → Easy: Removing a Leaf Node

![Binary%20Search%20Trees/Screen_Shot_2020-04-08_at_8.07.25_AM.png](Binary%20Search%20Trees/Screen_Shot_2020-04-08_at_8.07.25_AM.png)

- Case 2 → OK: Remove a Node with One Child

![Binary%20Search%20Trees/Screen_Shot_2020-04-08_at_8.09.19_AM.png](Binary%20Search%20Trees/Screen_Shot_2020-04-08_at_8.09.19_AM.png)

- Case 3 → Tricky: Remove a Node with Two Children
    - To remove a node `toDel` (Q in this ex)
        1. Find the node's (in-order) **predecessor** pre
        2. **Replace** `toDel.info` with `pre.info`
        3. **Remove** `pre` (this sounds like a recursion)
    - Why Does this work? Because replacing a node with its predecessor preserves the BST's ordering property. Why?
    - What happens when we remove the root (J)? What is its predecessor?
    - Is the predecessor a leaf? If not, how do you remove it?
    - Is it possible that the predecessor has two children again, so removing it becomes another case 3 (difficult case)?

![Binary%20Search%20Trees/Screen_Shot_2020-04-08_at_8.10.58_AM.png](Binary%20Search%20Trees/Screen_Shot_2020-04-08_at_8.10.58_AM.png)

### Removing a Node with Two Children

- How do we know the predecessor will not have two children itself?
    - Because it cannot have a right child (why?), it is either a leaf node or it is a node with only one left child
    - Hence we know removing the predecessor is one of the easy cases
- What about the successor?
    - Instead of the predecessor, is there another node we can use to replace the node to be removed?
        - Not if there are duplicate elements...

### The `size()`  method

- Method is implemented recursively
```java
    public int size() {
    	return recSize(root);
    }
    private int recSize(BSTNode<T> node) {
    	if (node == null) {
    			return 0;
    	} else {
    			return 1 + recSize(node.getLeft()) + recSize(node.getRight());
    	}
    }
```
### The `get()` method
```java
    public T get(T element) {
    	return recGet(element, root);
    }
    private T recGet(T element, BSTNode<T> node) {
    	// returns element e such that e.compareTo(element) == 0;
    	// if not such element exists, return null
    	if (node == null) {
    		return null; // element is not found
    	} else if (element.compareTo(node.getInfo()) < 0) {
    			return recGet(element, node.getLeft()); // get from left subtree
    	} else if (element.compareTo(node.getInfo()) > 0) {
    			return recGet(element, node.getRight()); // get from right subtree
    	} else {
    			return node.getInfo(); // element is found
    	}
    }
```