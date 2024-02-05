---
categories:
  - java
date: 2023-01-30T08:00:00+08:00
description: Some software also uses older versions of Java, such as JDK 11, so installing JDK 11 will enable you to run those applications on your Mac M1 computer. This article will show you how to install JDK 11 and set up the Java Home environment variable on Mac M1.
draft: false
featuredImage: /series/java-core-basic/java-core-lesson-3-naming-rules-data-types-and-casts-operators-control-structures-and-branching.webp
images:
  - /series/java-core-basic/java-core-lesson-3-naming-rules-data-types-and-casts-operators-control-structures-and-branching.webp
  - /java-core-lesson-3-naming-rules-data-types-and-casts-operators-control-structures-and-branching/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - java-core-basics
tags:
  - java
  - java-core
title: Lesson 3 - Naming rules, data types and casts, operators, control structures, and branching
url: /java-core-lesson-3-naming-rules-data-types-and-casts-operators-control-structures-and-branching
weight: 3
---

Java is one of the most popular programming languages today with many applications worldwide. In this lesson, we will learn about naming conventions, data types and type casting, operators, control structures, and branching. Understanding and mastering these concepts will help you become a skilled Java programmer. Let’s start learning these concepts right now!

## Naming conventions

In Java, naming variables, constants, methods, and classes is very important and affects the consistency and readability of the code. Below are the basic naming conventions in Java:

1. Variable and method names should start with a lowercase letter, followed by uppercase letters, and no spaces.

```java
// Correct variable naming convention
int myVariable;
String myMethod();
// Incorrect variable naming convention
int My Variable; // contains spaces
String 1stMethod; // starts with a number
```

2. Class names should start with an uppercase letter and use CamelCase naming convention (i.e., the following words will be capitalized).

```java
// Correct class naming convention
public class MyClass {
    // code
}

// Incorrect class naming convention
public class my_class {
    // code
}
```

3. Constant names should be written in all uppercase letters and words separated by underscores (\_).

````java
// Correct constant naming convention
public static final int MAX_VALUE = 100;

// Incorrect constant naming convention
public static final int maxValue = 100; // not all uppercase
public static final int MAXVALUE = 100; // no underscore

4. Package names should be written in lowercase letters, with words separated by dots.

```java
// Correct package naming convention
package com.example.myproject;

public class MyClass {
    // code
}

// Incorrect package naming convention
package com.example.MyProject; // capital letters used
````

5. Variable, method, class, and constant names should be meaningful and describe their content.

```java
// Unclear variable name declaration
int x = 5;

// Clear variable name declaration
int numberOfStudents = 5;

// Unclear method name declaration
public void doSomething(int a, int b) {
    // code
}

// Clear method name declaration
public void calculateSum(int firstNumber, int secondNumber) {
    // code
}

// Unclear class name declaration
public class MyClass {
    // code
}

// Clear class name declaration
public class Student {
    // code
}

// Unclear constant name declaration
public static final int MAX = 100;

// Clear constant name declaration
public static final int MAX_STUDENTS = 100;
```

6. Avoid using Java keywords as variable, method, class, or constant names.

```java
// Variable declaration using Java keyword as variable name
int int = 5;

// Method declaration using Java keyword as method name
public void public() {
    // code
}

// Class declaration using Java keyword as class name
public class String {
    // code
}

// Constant declaration using Java keyword as constant name
public static final int final = 100;
```

7. Avoid using special characters such as @, $, # in variable, method, class, or constant names.

```java
// Variable declaration using special character in variable name
int my@variable = 5;

// Method declaration using special character in method name
public void do$something() {
    // code
}

// Class declaration using special character in class name
public class My#Class {
    // code
}

// Constant declaration using special character in constant name
public static final int MY$CONSTANT = 100;
```

8. Use English when naming variables, methods, classes, or constants.

```java

// Variable declaration using English in variable name
int numberOfStudents = 5;

// Method declaration using English in method name
public void calculateSum(int firstNumber, int secondNumber) {
    // code
}

// Class declaration using English in class name
public class Student {
    // code
}

// Constant declaration using English in constant name
public static final int MAX_STUDENTS = 100;

```

9. Names should have a reasonable length, not too short or too long.

```
// Variable name is too short and does not describe its content
int x = 5;

// Method name is too long, making the code hard to read
public void calculateSumOfTwoNumbersAndPrintResultOnConsole(int firstNumber, int secondNumber) {
    // code
}

// Class name is too short and does not describe its content
public class A {
    // code
}

// Constant name is too long, making the code hard to read
public static final int MAXIMUM_NUMBER_OF_STUDENTS_THAT_CAN_BE_ENROLLED_IN_A_SCHOOL = 1000;
```

> These naming conventions make your code more readable and understandable, and also help your code meet Java programming standards.

## Data Types and Type Casting

Java is a statically typed programming language, which means you must declare the data type for variables before using them.

1. Primitive Data Types

There are 8 primitive data types in Java, listed in the table below:

| **Data Type** | **Size (bits)** | **Range**                                               | **Default Value** |
| ------------- | --------------- | ------------------------------------------------------- | ----------------- |
| byte          | 8               | -128 to 127                                             | 0                 |
| short         | 16              | -32,768 to 32,767                                       | 0                 |
| int           | 32              | -2,147,483,648 to 2,147,483,647                         | 0                 |
| long          | 64              | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 | 0L                |
| float         | 32              | 1.4E-45 to 3.4028235E38                                 | 0.0f              |
| double        | 64              | 4.9E-324 to 1.7976931348623157E308                      | 0.0d              |
| char          | 16              | 0 to 65,535                                             | '\u0000'          |
| boolean       | 1               | true or false                                           | false             |

2. Wrapper Class

Wrapper Class is a set of object classes in Java that are used to represent primitive data types as objects. Wrapper classes provide methods to manipulate the values of primitive data types and convert between different primitive data types. Wrapper classes are also used in Java's Collection Framework, as collection frameworks only work with objects, not directly with primitive data types.

The wrapper classes corresponding to primitive data types in Java are as follows:

| **Primitive Data Type** | **Wrapper Class** |
| ----------------------- | ----------------- |
| byte                    | Byte              |
| short                   | Short             |
| int                     | Integer           |
| long                    | Long              |
| float                   | Float             |
| double                  | Double            |
| char                    | Character         |
| boolean                 | Boolean           |

Here are some examples of wrapper classes in Java:

```java
// Convert a string to an integer using Integer.parseInt()
String numStr = "123";
int num = Integer.parseInt(numStr);
System.out.println(num); // Output: 123

// Convert an integer to a string using Integer.toString()
int value = 42;
String str = Integer.toString(value);
System.out.println(str); // Output: "42"

// Check if a string represents a valid boolean value using Boolean.parseBoolean()
String boolStr = "true";
boolean bool = Boolean.parseBoolean(boolStr);
System.out.println(bool); // Output: true

// Convert a character to a string using Character.toString()
char ch = 'a';
String str = Character.toString(ch);
System.out.println(str); // Output: "a"

// Convert a string to a character array using String.toCharArray()
String str = "hello";
char[] chars = str.toCharArray();
System.out.println(chars); // Output: "hello"
```

3. Casting

Casting is the process of converting between different data types in Java. Casting can be done using the appropriate type operator or by using conversion methods of the wrapper classes. There are two types of casting in Java:

- Implicit Casting: is the process of converting the value of a smaller data type to a larger data type without using the correct type operator. For example:

```java
int i = 10;
double d = i; // Implicit casting
System.out.println(d); // Output: 10.0
```

- Explicit Casting: is the process of converting the value of a larger data type to a smaller data type. In this process, we need to use the correct type operator to perform the casting. For example:

```java
double d = 10.5;
int i = (int) d; // Explicit casting
System.out.println(i); // Output: 10
```

> Note that when performing explicit casting, the value of the variable may be lost if the value exceeds the range of the destination data type.

## Operators

In Java, there are many types of operators to perform arithmetic and comparison operations. These operators can be divided into groups as follows:

1. Arithmetic operators: used to perform arithmetic operations, including addition (+), subtraction (-), multiplication (\*), division (/), modulus (%), increment (++), decrement (--), and other related operations.

For example:

```java
int a = 10, b = 5;
int sum = a + b;           // sum = 15
int difference = a - b;    // difference = 5
int product = a * b;       // product = 50
int quotient = a / b;      // quotient = 2
int remainder = a % b;     // remainder = 0
int increment = ++a;       // increment = 11, a = 11
int decrement = --b;       // decrement = 4, b = 4
```

2. Assignment operators: used to assign a value to a variable or an expression, including the single assignment operator (=) and the combined assignment operators with other arithmetic operators.

For example:

```java
int a = 10;
a += 5;  // a = 15
a -= 3;  // a = 12
a *= 2;  // a = 24
a /= 3;  // a = 8
a %= 3;  // a = 2
```

3. Logical operators: used to perform logical operations, including AND (&&), OR (||), NOT (!), and other related operations.

For example:

```java
# AND (&&) operator: returns true if both expressions are true, otherwise returns false.
int a = 5;
int b = 10;
if (a > 0 && b > 0) {
    // Do something
}

# OR (||) operator: returns true if at least one of the expressions is true, otherwise returns false.
int a = 5;
int b = 10;
if (a > 0 || b < 0) {
    // Do something
}

# NOT (!) operator: returns true if the expression being evaluated is false and vice versa.
boolean flag = false;
if (!flag) {
    // Do something
}
```

4. Comparison operators: used to compare the values of two variables or expressions, including equal to (==), not equal to (!=), greater than (>), less than (<), greater than or equal to (>=), and less than or equal to (<=).

For example:

```java
int a = 5;
int b = 10;
boolean result;

result = (a == b);  // false
result = (a != b);  // true
result = (a > b);   // false
result = (a < b);   // true
result = (a >= b);  // false
result = (a <= b);  // true
```

5. Bitwise operators: used to perform bitwise operations on integers, including AND (&), OR (|), XOR (^), NOT (~), and other related operations.

For example:

```java
# AND (&) operator
int a = 5;  // binary: 0101
int b = 3;  // binary: 0011
int result = a & b;  // binary: 0001 (decimal: 1)

# OR (|) operator
int a = 5;  // binary: 0101
int b = 3;  // binary: 0011
int result = a | b;  // binary: 0111 (decimal: 7)

# XOR (^) operator
int a = 5;  // binary: 0101
int b = 3;  // binary: 0011
int result = a ^ b;  // binary: 0110 (decimal: 6)

# NOT (~) operator
int a = 5;  // binary: 0101
int result = ~a;  // binary: 1010 (decimal: -6)
```

6. Bitwise shift operators: used to shift the bits in an integer, including the left shift operator (<<), the signed right shift operator (>>), and the unsigned right shift operator (>>>).

For example:

```java
int a = 10;  // binary: 1010
int b = a << 2;  // binary: 101000 (decimal: 40)
int c = a >> 2;  // binary: 10 (decimal: 2)
int d = a >>> 2;  // binary: 10 (decimal: 2)
```

## Control Structures and Branching

> In Java, control structures and branching are used to perform checks and decisions in the program. These structures allow the program to execute different statements depending on specified conditions.

There are two types of control structures in Java: branching control structures and looping control structures.

1. Branching control structures include:

- If statement: used to check a condition and execute different statements depending on the result of that condition.
- If-else statement: similar to the If statement, but with an additional block of code to execute when the condition is false.
- Switch statement: used to check a variable or an expression and execute different statements depending on the value of that variable.

Example of branching control structures:

```java
int age = 20;

if (age < 18) {
  System.out.println("You are not old enough to view this content.");
} else if (age >= 18 && age < 21) {
  System.out.println("You can view this content but cannot drink alcohol.");
} else {
  System.out.println("You can view this content and drink alcohol.");
}
```

2. Looping control structures include:

- For loop: used to repeat a block of code a certain number of times.
- While loop: used to repeat a block of code while a specific condition is true.
- Do-while loop: similar to the While loop, but with a block of code that is executed at least once before checking the condition.

Example of looping control structures:

```java
int i;

for (i = 0; i < 10; i++) {
  System.out.println("The value of i is: " + i);
}

i = 0;
while (i < 10) {
  System.out.println("The value of i is: " + i);
  i++;
}

i = 0;
do {
  System.out.println("The value of i is: " + i);
  i++;
} while (i < 10);
```

#### In this article, we have learned about data types, type casting, wrapper classes, and operators in Java. We have also looked at control structures, including conditional statements and loops. Hopefully, this knowledge will help you understand and develop your Java applications better.
