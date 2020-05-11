# Stacks (part 2)

Class: CS 187
Created: Feb 11, 2020 11:27 AM
Reviewed: No
Test?: Unit 2
Type: Lecture

### The Stack Interface
```java
    public interface StackInterface<T> {
    	void push (T element) throws StackOverflowException;
    	T pop() throws StackUnderflowException;
    	T peek() throws StackUnderflowException;
    	boolean isEmpty();
    	boolean isFull();
    }
```
### Array-based Stack
```java
    public class ArrayStack<T> implements StackInterface<T> {
    	// array-stack is bounded, default capacity –> 100
    	protected final int DEFCAP = 100;
    	protected T[] stack; // stores stack elements
    	protected int topIndex = -1; // index of top element
```
- Other than this is a **generic** class, the data members are similar to *ArrayStringLog* that we've learned before
- Use topIndex to point to top element of the stack –> initialiozed to *-1* indicating the stack is empty to start
- **Pushing and popping happen at the last element in the array**
- When *(topIndex == stack.length-1)*, it has reched the capacity of the array –> stack is full

### Methods for ArrayStack
```java
    public boolean isEmpty() {
    	return (topIndex == -1);
    }
    public boolean isFull() {
    	return (topIndex == (stack.length -1));
    }
    public int size() {
    	return topIndex + 1;
    }
```
### Constructors for ArrayStack

- **Conceptually**, we want to do this:
```java
    public class ArrayStack<T> implements StackInterface<T> {
    	public ArrayStack() {
    		stack = new T[DEFCAP]; // NOOOOOOOO
    	}
    }
```
- **Unfortunately**, Java **does not** allow to create a generic array (i.e. with a to-be-decided-later type) [[read me]](http://www.angelikalanger.com/Articles/Papers/JavaGenerics/ArraysInJavaGenerics.htm)

### Casting to a Generic Type

- Work-around is to create an array of type ***Object*** (i.e. super-class of all Java objects), and **explicitly cast** the resulting array into type ***T[]***
    - Compiler will **warn** about **Type Safety** (that it cannot be sure elements stored in the array are type T), but can ignore this warning
- This is okay because an array of objects is an array of references (i.e. each element is a pointer) –> whether it is an array of Strings, Apples, Objects, the array takes the same amount of memory space

### Code for Constructors
```java
    public ArrayStack(int maxSize) {
    	stack = (T[]) new Object [maxSize];
    }
    public ArrayStack() {
    	this(DEFCAP);
    }
```
- Like before, there are 2 constructors, one w/ no parameter (uses default capacity), and one w/ custom capactiy as parameter
- Note that the **constructor's name does not contain <T> part**

### Code for *peek()*
```java
    public T peek() {
    	if (isEmpty()) 
    		throw new StackUnderflowException("underflow");
    	return stack[topIndex]; // look at top element w/o removing it
    }
```
### Code for *pop()*
```java
    public T pop() {
    	if (isEmpty())
    		throw new StackUnderflowException("underflow");
    	return stack[topIndex--]; // removes top element from stack
    }
```
- Equivalent to:
```java
    public void pop() {
    	if (isEmpty())
    		throw new StackUnderflowException("underflow");
    	T element = stack[topIndex];
    	topIndex--;
    	return element; // removes top element from stack**
    }
```
### Code for *push()*
```java
    public void push (T element) {
    	if (isFull())
    		throw new StackOverflowException("overflow");
    	stack[++topIndex] = element; // add new object to top of stack
    }
```
- Equivalent to:
```java
    public void push (T element) {
    	if (isFull())
    		throw new StackOverflowException("overflow");
    	topIndex++; // or ++topIndex is also fine
    	stack[topIndex] = element; // add new object to top of stack
    }
```
### Stack Application 1: Reversing a Word

- Write a program that accepts a word and outputs the word in reverse order
    - Ex. *STRESSED –> DESSERTS*
- Can be done easily using a stack:
    - **Push** each character one by one to stack
    - **Pop** the stack one by one
```java
    public void reverse(String word) {
    	Stack s = new Stack();
    	for (int i = 0; i < word.length; i++) {
    		s.push(word.charAt(i));
    	}
    	while (!s.isEmpty()) {
    		System.out.print(s.pop());
    	}
    }
```
### Stack Application 2: Delimeter Matching

- Write a program to make sure the parentheses in a math expression are balanced:
    - (w * (x + y) / z - (p / (r - q) ) )
- It may have several different types of delimeters: **braces{}**, **brackets[]**, **parentheses()**
    - Each opening (left) delimeter must be matched by a corresponding closing (right) delimeter
    - A delimeter that opens the last must be closed by a matching delimter first (ex. [ a * ( b + c ] + d ) is wrong!)
- How to implement this algorithm:
    - Read characters one-by-one from the expression
    - Whenever you see a **left** (opening) delimeter, **push** it to stack
    - Whenever you see a **right** (closing) delimeter, **pop** from stack and **check match** (i.e. same type?)
    - If they don't match, report mismatch error
    - Edge cases:
        - What happens if stack is **empty** when trying to match a closing delimeter?
        - What happens if stack is **non-empty** after reaching end of the expression?

### Stack Application 3: Evaluate Postfix Expressions

- Goal –> evaluate arithmetic expressions
- Terminology:
    - **Operands** –> numbers
    - **Operators** –> + - / *
- Focus on **binary** operators (each operator takes 2 numbers as input)
- **Infix notation** –> operators are placed between two operands
    - Ex. (2 + 14) * 23
- **Postfix notation** –> operators are placed after operands (aka RPN, Reverse Polish Notation); ex:
    - 5 3 -                 // 5 - 3
    - A B /                // A / B
    - 2 14 + 23 *    // (2 + 14) * 23
- **An operator acts on the two valies to its left**, where a value may be either a number in original expression or result of previous operator
- *Note order of the operands: **further left –> operator –> closer left** (matters for - and /)
- How do we write an algorithem to evaluate postfix expressions? Note that whenever you encounter an operator, you apply it to last two operands –> use a **stack to store the operands**)
    - Scan the experssion
    - See an **operand** –> **push** it onto the stack
    - See an **operator** –> **pop two values** off the stack, apply the operator to them, **push result** back onto the stack
    - Answer = last (only **one) value** on the stack
- If we ever encounter an empty stack, or if we finish with more than one value on it, the postfix expression was not valid. Thus our algorithm can also test the validity of an input postfix expression
- Invalid examples: 1 + * , 1 2 3 +
- Pseudo code:
```java
    while more token exist {
    	get the next token 
    	if token is an operand:
    		stacck.push(token);
    	else { // if token is an operator
    		operand2 = stack.pop();
    		operand1 = stack.pop();
    		result = (operand1) token (operand2);
    		stack.push(result);
    	}
    }
    return stack.pop()
    // code above omits error detection and handling
```
### Java's *ArrayList* Class

- Arrays can actually dynamically expand, thus overcoming 'fixed capacity' limitation (**dynamic arrays**)
- Java provides several variable-length generic array structures (i.e. *ArrayList<T>*); begins with some fixed capacity –> capacity is reached –> capies itself into another array of twice the size –> appears to be an array of unitmited size
- How does this doubling-the-capacity work?
    - Start with initial ArrayList of capacity 10, keep adding elements to it
    - Adding 11th element causes ArrayList to allocated a new Array of capacity 20 –> copy exisiting 10 elements and 11th element to new array –> release old array
    - By the time we have 81 elements, four capacity 'expansions' happened (20, 40, 80, 160)
    - O(n) complexity