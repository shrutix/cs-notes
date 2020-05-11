# Binary Trees

Class: CS 187
Created: Mar 31, 2020 1:17 PM
Materials: Binary%20Trees/Lec16BinaryTree.pdf
Reviewed: No
Test?: Unit 2
Type: Lecture

### Search / Find an Element

- **Searching / finding a specific element** in a list of N elements requires **O(N)** time, whether the list is stored as an array or a linked structure
- In the case of a **sorted Array**, a lot better using a **binary search algorithm**

### Guess-a-Number Game

- The host picks a number between 1 to n (say n = 1000), and asks you as the player to guess that number
- When you make a guess, the host will tell you one of threee things –– your guess is 1. too larges, 2. too small, or 3. correct
- How would you take your guesse sin order to find the correct number in the fewest possible guesses?
- Start with the number in the middle, in our case, `(1 + 1000) / 2 = 500`; if the host say 500 is:
    - **Too large** –– correct number must be between 1 to 499, the next guess would be `(1 + 499) / 2 = 250`
    - **Too small** ––correct number must be between 501 to 1000, the next guess would be `(501 + 1000) / 2 = 750`
    - **Correct** –– great!
- How many guesses to make in the worst case?
- Each guess successively **halves** the range of possible values; eventually (in the worst case) the range narrows down to only one number and that must be the answer
- Even in the worst case, this will take no more than `ceiling(log2(1000) = 10` steps
- This is logarithmic time O(log N) → enormously better than a linear time algorithm O(N) for a sufficiently large N

### Binary Search

- **Problem Statement**: given a sorted array of elements and a target element → find if target exists in the array and return its index (return -1 if it does not exist)
- Ex.       `[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]`
    - Idx: 0    1    2   3     4     5     6      7     8      9    10    11    12    13   14
    - Task 1: find target = 31
    - Using **linear search** it requires 11 steps
    - With binary search, how many steps? Only 3 steps!
    - Task 2: find target = 31 (non-existant)
    - Using **linear search** it requires **12 steps**
    - Using **binary search** it requires only **4 steps**
```java
    protected int find (T target) {
    	int lower = 0, upper = numElements - 1;
    	while (lower <= upper) {
    		int curr = (upper + lower) / 2; // rounds down
    		int result = target.compareTo(list[curr]);
    		if (result == 0) {
    			return curr;
    		} else if (result < 0) {
    			upper = curr - 1;
    		} else {
    			lower = curr + 1; }
    		}
    	return -1;
    }
```
### Clicker Question #1

```java
    protected int find (T target) {
    	int lower = 0, upper = numElements - 1;
    	while (lower <= upper) {
    		int curr = (upper + lower) / 2;
    		int result = target.compareTo(list[curr]);
    		if (result == 0) {
    			return curr;
    		} else if (result < 0) {
    			**upper = curr;**
    		} else {
    			lower = curr + 1; }
    		}
    	return -1;
    }
```

- What happens if the bolded line is changed to `upper = curr` instead of `upper = curr -1`?
    - a. When element is found, the returned index may be wrong
    - b. It may throw a `NullPointerException`
    - c. It may fail to find an existing element
    - d. The loop may run forever
    - e. It may throw an `IndexOutOfBoundException`

### Clicker Question #2
```java
    protected int find (T target) {
    	int lower = 0, upper = numElements - 1;
    	while **(lower < upper)** {
    		int curr = (upper + lower) / 2;
    		int result = target.compareTo(list[curr]);
    		if (result == 0) {
    			return curr;
    		} else if (result < 0) {
    			upper = curr;
    		} else {
    			lower = curr + 1; }
    		}
    	return -1;
    }
```
- What happens if the ≤ in the while loop condition is changed to <?
    - a. When element is found, the returned index may be wrong
    - b. The loop may run forever
    - c. It may fail to find an existing element
    - d. It may throw a `NullPointerException`
    - e. It may throw an `IndexOutOfBoundException`

### Binary Search –– Recursive Verision
```java
    protected int recFind(T target, int lower, int upper) {
    	if (lower > upper) { 
    		return -1;
    	}
    	int cur = (lower + upper) / 2;
    	int result = target.compareTo(list[curr]);
    	if (result == 0) {
    		return cur;
    	} else if (result < 0) {
    		return recFind(target, lower, cur - 1);
    	} else {
    		return recFind(target, cur + 1, upper);
    	}
    }
    protected int find(T target) {
    	return recFind(target, 0, numElements - 1);
    }
```
### Binary Search cont

- For a sorted array w/ N elements → binary search guaranteed to finish within `O(log N)` time; big win for **large array**s!
- For ex. how big is the difference for N = 1,000 or even 1,000,000?
- Is there any downside? What's the tradeoff?
    - The array **must be sorted**, so insertion is more expensive: `O(N)` (compared to `O(1)` for unsorted arrays)
    - Does not work on a linked structure as there is no simple way to index a linked element in `O(1)` time

### The Tree Data Structure

- A **linked list** is a linear structure in which each element has one "successor"

![Binary%20Trees/Screen_Shot_2020-04-05_at_1.30.29_PM.png](Binary%20Trees/Screen_Shot_2020-04-05_at_1.30.29_PM.png)

- A **tree** is a more generalized structure in which each element may have many "successors" (i.e. children)

![Binary%20Trees/Screen_Shot_2020-04-05_at_1.41.52_PM.png](Binary%20Trees/Screen_Shot_2020-04-05_at_1.41.52_PM.png)

- A tree has a top node (**root node**), followed by its children, and the children of children...
- Actually looks like reversed from real trees (inverted)
- Mathematically speaking, trees are **connected**, **acyclic graphs** (i.e. no loops)
    - There **is one unique root**
    - From root to any node, there is **one and only one path**
- Very useful for representing **hierarchical strcutures**, such as file systems, Java's classes, and inheritance relationships between classes
- Will focus on **binary trees**, where each node has **at most two children**
- **Tree summary**: Unique root, Unique path from root to any node

### Is a Linked List a Tree?

- [x]  Unique root
- [x]  Unique path from root to any node?
- Yes, it is a tree!

### Tree Terminology

![Binary%20Trees/Screen_Shot_2020-04-05_at_1.40.52_PM.png](Binary%20Trees/Screen_Shot_2020-04-05_at_1.40.52_PM.png)

- **Root** - the starting node at the top → there is only one root
- **Parent** - node that points to the current node; any node, except the root, has one and only one parent
- **Child** - nodes pointed to by the current node (for a binary tree, we say "left child" and "right child")
- **Leaf** - node with no childfren (there may be many leave in a tree, note that the root may be a leaf! how?)
- **Interior node** - non-leaf node → has at least one child
- **Path** - sequence of nodes visited by traveling from the root to a particular node; each path is unique (due to tree being a-cyclic)
- **Ancestor** - any node on the path from the root to the current node
- **Descendent** - any node whose path from the root contains the current node
- **Subtree** - any node may be considered the root of a subtree, which consists of all the descendants of this node
- **Level** - path length from the root to the current node
    - *See above diagram
    - Recall that each path is unique, hence level is unqiue
    - Root is at level 0
    - **Height** - maximum level in a tree
        - For a reasonably balanced tree with N nodes → height is `O(log N)` (will become obvious later)
        - What is the maximum possible height of a tree of N Nodes? N - 1

### Traversing a Binary Tree

- Traversing means visitng all nodes in the tree in a specific order. While the traversal order is obvious for a linked list, for trees there are three common methods, distinguishing by the **order in which the current node is visited** during the recursive traversal
    - **Pre-order traversal** - visit the *current* node, visit the left subtree, then visit the right subtree
    - **In-order traversal** - visit the left subtree, visit the *current* node, then visit the right subtree
    - **Post-order traversal** - visit the left subtree, visit the right subtree, then visit the *current* node
- Comparing the tree traversal methods (numbers refer to the order of traversal)

![Binary%20Trees/Screen_Shot_2020-04-05_at_4.41.07_PM.png](Binary%20Trees/Screen_Shot_2020-04-05_at_4.41.07_PM.png)

- The subtrees are traversed **recursively**!

### Tree Traversal Examples

![Binary%20Trees/Screen_Shot_2020-04-05_at_1.41.52_PM%201.png](Binary%20Trees/Screen_Shot_2020-04-05_at_1.41.52_PM%201.png)

- Pre-Order : P F B H G S R Y T W Z
- In-Order : B F G H P R S T W Y Z
- Post-Order : ?

### Clicker Question #3

- What is the post-order traversal result of this tree? (above image)
    - a. B G H F P R S W T Z Y
    - b. B H G F W T Z Y R S P
    - c. F S P B H G R Y T W Z
    - d. F B G H R W T Z S P
    - e. B G H F R W T Z Y S P

### Recursive Traversal of Trees
```java
    public void preOrder(TreeNode x) {
    	if (x != null) {
    		// visit by printing the value
    		System.out.println(x.getInfo());
    		preOrder(x.getLeft());
    		preOrder(x.getRight());
    	}
    }
```
- How are in-order and post-order traversals different?

### More Terminology

- **Perfect Tree** - Binary Tree in which all of the leaves are on the same level and every non-leaf node has two children /_\
- If a perfect tree is of height `h`, how many leaf nodes does it have? How many nodes (including leaf and interior) does it have?

### Math of Perfect Trees

/whi

![Binary%20Trees/Screen_Shot_2020-04-05_at_6.46.22_PM.png](Binary%20Trees/Screen_Shot_2020-04-05_at_6.46.22_PM.png)

[Number of nodes at level L](Binary%20Trees/Number%20of%20nodes%20at%20level%20L.csv)

- Total # of nodes in a perfect tree of height h
    - N = 2^0 + 2^1 + 2^2 + ... + 2^h
    - N = 2(2^h) - 1
    - N = 2^(h+1) - 1
- level 0 → 1, level 1 → 2, level 2 → 4, level 3 → 8, total → 15
- Conversely, the height of a perfect tree with N nodes is `h = log2(n+1) - 1 = O(log N)`