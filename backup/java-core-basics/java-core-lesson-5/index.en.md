---
categories:
  - java
date: 2023-01-30T08:00:00+08:00
description: The Java Collections framework is a set of classes and interfaces in Java that provides data structures for storing, accessing, and managing objects in Java. It provides some common types of data structures such as List, Set, and Map, as well as more complex data structures such as Queue, Stack, and Tree. The Java Collections framework can be used to increase efficiency, reduce time, and cost of data processing operations in Java.
draft: false
featuredImage: /series/java-core-basic/java-core-lesson-5-java-collections-framework-en.webp
images:
  - /series/java-core-basic/java-core-lesson-5-java-collections-framework-en.webp
  - /java-core-lesson-5-java-collections-framework-en/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - java-core-basics
tags:
  - java
  - java-core
title: Lesson 5 - Java Collections Framework
url: /java-core-lesson-5-java-collections-framework-en
weight: 5
---

The Java Collections framework is a set of classes and interfaces in Java that provides data structures for storing, accessing, and managing objects in Java. It provides some common types of data structures such as List, Set, and Map, as well as more complex data structures such as Queue, Stack, and Tree. The Java Collections framework can be used to increase efficiency, reduce time, and cost of data processing operations in Java.

Here we will go into detail about the interfaces of the Java Collections framework.

## 1. List

> List in Java is an interface of the Java Collection Framework, providing methods to manipulate a list of elements. List allows storing duplicate elements and in the order they were inserted.

Some basic methods of List:

- add(E element): add an element to the end of the list.
- add(int index, E element): add an element to the specified position in the list.
- remove(Object obj): remove the specified element from the list.
- get(int index): return the element at the specified position in the list.
- size(): return the number of elements in the list.
- clear(): remove all elements from the list.

List is an interface, so we cannot directly instantiate a List object. Instead, we can use List implementation classes such as ArrayList, LinkedList, Vector, ... to create a list that suits our needs.

Example:

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        // Initialize a List of type String
        List<String> myList = new ArrayList<>();

        // Add elements to the List
        myList.add("Java");
        myList.add("Python");
        myList.add("C++");

        // Access elements in the List
        System.out.println("Elements in the List are:");
        for (String element : myList) {
            System.out.println(element);
        }

        // Remove an element from the List
        myList.remove("Python");
        System.out.println("After removing the element Python, the remaining elements in the List are:");
        for (String element : myList) {
            System.out.println(element);
        }

        // Check if an element exists in the List
        boolean containsJava = myList.contains("Java");
        System.out.println("Does the List contain the element Java? " + containsJava);

        // Get the size of the List
        int size = myList.size();
        System.out.println("The size of the List is: " + size);
    }
}

```

The output is:

```
Elements in the List are:
Java
Python
C++
After removing the element Python, the remaining elements in the List are:
Java
C++
Does the List contain the element Java? true
The size of the List is: 2
```

2. Set

> In Java, Set is an interface in the Collection framework used to store a collection of unique elements. It inherits the Collection interface and adds additional features to manage sets of unique elements.

Characteristics of Set:

- Does not contain duplicate elements.
- Can contain some null elements (if allowed).
- Does not guarantee the order of elements.

Some common methods of Set:

- add(E e): Add an element to the Set.
- remove(Object o): Remove an element from the Set.
- contains(Object o): Check if the Set contains the specified element.
- size(): Return the number of elements in the Set.
- iterator(): Return an Iterator to iterate through the elements in the Set.

Example of Set:

```java
Set<String> set = new HashSet<>();

set.add("apple");
set.add("banana");
set.add("orange");

System.out.println(set); // Output: [orange, banana, apple]

set.remove("banana");

System.out.println(set); // Output: [orange, apple]

System.out.println(set.contains("apple")); // Output: true

System.out.println(set.size()); // Output: 2

Iterator<String> iterator = set.iterator();

while (iterator.hasNext()) {
   System.out.println(iterator.next());
}

```

The output is:

```
[orange, banana, apple]
[orange, apple]
true
2
orange
apple
```

3. Map

> In the Java Collections Framework, Map is an interface used to store a collection of key-value pairs, where each key is associated with a value. Map is a subinterface of Collection and is used to create data maps in Java.

Some characteristics of Map:

- Keys in Map are unique and cannot be duplicated.
- Value can be duplicated.
- Map does not guarantee the order of key-value pairs.

Map is implemented by classes such as HashMap, TreeMap, LinkedHashMap, etc.

Example:

```java
import java.util.*;

public class MapExample {
    public static void main(String[] args) {
        // Initialize a Map object using the HashMap class
        Map<String, Integer> myMap = new HashMap<>();

        // Add key-value pairs to the Map
        myMap.put("apple", 1);
        myMap.put("banana", 2);
        myMap.put("orange", 3);

        // Access the value based on the key
        int value = myMap.get("apple");
        System.out.println("Value of key 'apple' is: " + value);

        // Loop through all key-value pairs in the Map
        for (Map.Entry<String, Integer> entry : myMap.entrySet()) {
            String key = entry.getKey();
            int val = entry.getValue();
            System.out.println("Key: " + key + ", Value: " + val);
        }
    }
}
```

The output shows:

```
Value of key 'apple' is: 1
Key: apple, Value: 1
Key: banana, Value: 2
Key: orange, Value: 3
```

4. Queue

> Queue is a data structure in the Java Collections Framework that stores elements in a First-In-First-Out (FIFO) manner, meaning that the first element added will be the first element removed.

Some important methods of Queue:

- add(element): Add an element to the queue, throws an exception if the queue is full.
- offer(element): Add an element to the queue, returns true if successful, false if the queue is full.
- remove(): Remove and return the first element of the queue, throws an exception if the queue is empty.
- poll(): Remove and return the first element of the queue, returns null if the queue is empty.
- peek(): Return the first element of the queue without removing it, returns null if the queue is empty.

Example:

```java

import java.util.LinkedList;
import java.util.Queue;

public class QueueExample {
    public static void main(String[] args) {
        Queue<String> queue = new LinkedList<>();
        queue.offer("apple");
        queue.offer("banana");
        queue.offer("orange");
        System.out.println("Queue: " + queue); // Queue: [apple, banana, orange]
        System.out.println("Size of queue: " + queue.size()); // Size of queue: 3
        System.out.println("First element of queue: " + queue.peek()); // First element of queue: apple
        System.out.println("Remove first element of queue: " + queue.poll()); // Remove first element of queue: apple
        System.out.println("Queue after removing first element: " + queue); // Queue after removing first element: [banana, orange]
    }
}
```

The output is:

```
Queue: [apple, banana, orange]
Size of queue: 3
First element of queue: apple
Remove first element of queue: apple
Queue after removing first element: [banana, orange]
```

## 5. Stack

> Stack is a special type of list data structure in which insertion and deletion both occur at one end of the list. The special thing about Stack is that it follows the Last-In-First-Out (LIFO) principle, meaning that the element inserted last will be the first one to be removed. Stack is often used in algorithms related to parsing, temporary storage, depth-first search (DFS), sorting, searching, reversing a string, and undo-redo operations in text and graphics applications.

Stack has two basic methods: push() to add an element to the top of the stack, and pop() to remove the element at the top of the stack. In addition, Stack also provides some other methods such as empty() to check if the stack is empty, peek() to get the value of the top element without removing it from the stack, and search() to find the position of an element in the stack.

Example of using Stack to reverse a string:

```java
import java.util.*;

public class ReverseString {
    public static void main(String[] args) {
        String str = "Hello world!";
        Stack<Character> stack = new Stack<>();

        // Push characters of string to stack
        for (int i = 0; i < str.length(); i++) {
            stack.push(str.charAt(i));
        }

        // Pop characters from stack to reverse the string
        StringBuilder reversedStr = new StringBuilder();
        while (!stack.empty()) {
            reversedStr.append(stack.pop());
        }

        System.out.println("Original string: " + str);
        System.out.println("Reversed string: " + reversedStr.toString());
    }
}
```

The output is:

```
Original string: Hello world!
Reversed string: !dlrow olleH
```

## 6. Tree

> Tree in the Java Collections Framework is often used to refer to data structures such as Binary Search Tree (BST), AVL Tree, Red-Black Tree, and many other types of trees.

A Tree in Java is a data structure organized into nodes and linked together by edges to form a tree. The root node is the top node of the tree, while the leaf node is a node without any child nodes. Other nodes are internal nodes.

In Java, we can use the "Tree" interface to work with trees such as BST or TreeMap, and the main methods include: add, remove, contains, size, and clear.

Example:

```java
import java.util.TreeSet;

public class TreeExample {
    public static void main(String[] args) {
        // Create a TreeSet to store integers
        TreeSet<Integer> treeSet = new TreeSet<>();

        // Add numbers to the TreeSet
        treeSet.add(5);
        treeSet.add(2);
        treeSet.add(9);
        treeSet.add(1);
        treeSet.add(3);

        // Print the elements in the TreeSet in ascending order
        System.out.println("TreeSet: " + treeSet);
    }
}
```

The output is:

```
TreeSet: [1, 2, 3, 5, 9]
```

In this article, we have learned about the Java Collections Framework, a collection of classes and interfaces used to store and manage objects. We have learned about collection types such as List, Set, Map, and other data structures such as Queue, Stack, and Tree. In summary, the Java Collections Framework is an important part of Java and a useful tool for programmers to manage and manipulate data easily and efficiently.
