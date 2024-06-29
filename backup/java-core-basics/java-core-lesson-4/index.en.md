---
categories:
  - java
date: 2023-01-30T08:00:00+08:00
description: String Processing, 1-Dimensional Array, and 2-Dimensional Array are important concepts in Java programming. In this section, we will learn about how to process strings in Java and how to use 1-dimensional and 2-dimensional arrays to store and process data. We will delve into each concept in detail, provide specific examples, and explain how to use them in real-world applications.
draft: false
featuredImage: /series/java-core-basic/java-core-lesson-4-string-processing-1-dimensional-array-2-dimensional-array.webp
images:
  - /series/java-core-basic/java-core-lesson-4-string-processing-1-dimensional-array-2-dimensional-array.webp
  - /java-core-lesson-4-string-processing-1-dimensional-array-2-dimensional-array/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - java-core-basics
tags:
  - java
  - java-core
  - 1-dimensional
  - 2-dimensional
title: Lesson 4 - String Processing, 1-Dimensional Array, 2-Dimensional Array
url: /java-core-lesson-4-string-processing-1-dimensional-array-2-dimensional-array
weight: 4
---

String Processing, 1-Dimensional Array, and 2-Dimensional Array are important concepts in Java programming. In this section, we will learn about how to process strings in Java and how to use 1-dimensional and 2-dimensional arrays to store and process data. We will delve into each concept in detail, provide specific examples, and explain how to use them in real-world applications.

## String Processing

In Java, String is a special data type used to store character strings. These strings can be processed through many methods supported in the String class.

To declare a String variable, we can use the following syntax:

```java
String str;
#or

String str = "Hello world!";
```

The methods that support string processing in Java include:

| **Method**                                   | **Description**                                                                            |
| -------------------------------------------- | ------------------------------------------------------------------------------------------ |
| length()                                     | Returns the length of the string.                                                          |
| charAt(int index)                            | Returns the character at the specified index.                                              |
| concat(String str)                           | Concatenates the string being called with the specified string.                            |
| contains(CharSequence s)                     | Checks if the string contains the specified substring.                                     |
| equals(Object obj)                           | Compares two strings.                                                                      |
| equalsIgnoreCase(String anotherString)       | Compares two strings without regard to case.                                               |
| indexOf(int ch)                              | Returns the index of the first occurrence of the specified character in the string.        |
| lastIndexOf(int ch)                          | Returns the index of the last occurrence of the specified character in the string.         |
| substring(int beginIndex, int endIndex)      | Extracts a portion of the string starting from the specified index.                        |
| toLowerCase()                                | Converts the entire string to lowercase.                                                   |
| toUpperCase()                                | Converts the entire string to uppercase.                                                   |
| trim()                                       | Removes whitespace from the beginning and end of the string.                               |
| replace(char oldChar, char newChar)          | Replaces all occurrences of the old character in the string with the new character.        |
| replaceAll(String regex, String replacement) | Replaces all substrings that match the regular expression with the specified string.       |
| split(String regex)                          | Splits the string into an array of substrings using a regular expression as the delimiter. |

Example:

```java
String str1 = "Hello";
System.out.println(str1.length()); // output: 5

String str2 = "Java is fun";
System.out.println(str2.charAt(2)); // output: v

String str3 = "Hello";
System.out.println(str3.concat(" world")); // output: Hello world

String str4 = "Java is fun";
System.out.println(str4.contains("is")); // output: true

String str5 = "Hello";
String str6 = "Hello";
System.out.println(str5.equals(str6)); // output: true

String str7 = "Java";
String str8 = "java";
System.out.println(str7.equalsIgnoreCase(str8)); // output: true

String str9 = "Hello";
System.out.println(str9.indexOf('l')); // output: 2

String str10 = "Hello World";
System.out.println(str10.substring(6, 11)); // Output: World

String str11 = "HeLLo WoRLd";
System.out.println(str.toLowerCase()); // Output: hello world

String str12 = "HeLLo WoRLd";
System.out.println(str.toUpperCase()); // Output: HELLO WORLD

String str13 = "   Hello World   ";
System.out.println(str.trim()); // Output: Hello World

String str14 = "Hello World";
System.out.println(str.replace('o', '0')); // Output: Hell0 W0rld

String str15 = "Hello World";
System.out.println(str.replaceAll("o", "0")); // Output: Hell0 W0rld

String str16 = "Hello,World";
String[] strArr = str16.split(",");
System.out.println(strArr[0]); // Output: Hello
System.out.println(strArr[1]); // Output: World

```

1-Dimensional Array

1-Dimensional Array is a data structure in programming used to store a set of values of the same data type. The 1-Dimensional Array is indexed starting from 0 and its size is determined at the time of declaration.

To declare a 1-Dimensional Array in Java, you can use the following syntax:

```css
<datatype>[] <arrayName> = new <datatype>[<size>];
```

For example, to declare a 1-Dimensional Array containing 5 integers, you can use the following syntax:

```java
int[] numbers = new int[5];
```

After declaration, you can assign values to the elements of the array by accessing the index of that element in the array. For example:

```java
numbers[0] = 1;
numbers[1] = 2;
numbers[2] = 3;
numbers[3] = 4;
numbers[4] = 5;
```

You can also access the values of the elements in the array by using the corresponding index. For example:

```java
int x = numbers[2]; // get the value of the 3rd element in the array, which is 3
```

Mảng 1 chiều cũng cung cấp một số phương thức hỗ trợ, bao gồm:

- **length**: trả về số lượng phần tử trong mảng.
- **sort**: sắp xếp các phần tử trong mảng theo thứ tự tăng dần.
- **toString**: trả về một chuỗi đại diện cho mảng.

Ví dụ:

```
int[] numbers = {5, 2, 8, 1, 9};
int length = numbers.length; // trả về 5
Arrays.sort(numbers); // sắp xếp mảng theo thứ tự tăng dần
String str = Arrays.toString(numbers); // trả về chuỗi "[1, 2, 5, 8, 9]"
```

## One-Dimensional Array

One-dimensional array also provides some supporting methods, including:

- **length**: returns the number of elements in the array.
- **sort**: sorts the elements in the array in ascending order.
- **toString**: returns a string representation of the array.

For example:

```
int[] numbers = {5, 2, 8, 1, 9};
int length = numbers.length; // returns 5
Arrays.sort(numbers); // sorts the array in ascending order
String str = Arrays.toString(numbers); // returns the string "[1, 2, 5, 8, 9]"
```

## Two-Dimensional Array

Two-dimensional array in Java is an array with a size larger than that of a one-dimensional array. A two-dimensional array can be considered as a table (or a matrix) with elements arranged in rows and columns. Each element in the two-dimensional array is accessed through the index of the row and column.

To declare a two-dimensional array in Java, use the following syntax:

```css
dataType[][] arrayName = new dataType[rowSize][colSize];
```

Where:

- **dataType** is the data type of the elements in the array.
- **arrayName** is the name of the array.
- **rowSize** is the size of the row (the number of elements in the row) in the two-dimensional array.
- **colSize** is the size of the column (the number of elements in the column) in the two-dimensional array.

For example, to declare a two-dimensional array with a size of 3 rows x 4 columns with the data type of int, you can use the following code:

```java
int[][] arr = new int[3][4];
```

To access and assign values to the elements in the two-dimensional array, use the following syntax:

```java
arr[rowIndex][colIndex] = value;
```

Where:

- **rowIndex** is the index of the row (starting from 0).
- **colIndex** is the index of the column (starting from 0).
- **value** is the value to be assigned to the element with the corresponding index.

For example:

```java
arr[0][0] = 1;  // assigns the value 1 to the element at row 0, column 0
int x = arr[1][2];  // accesses the value of the element at row 1, column 2 and stores it in variable x
```

Some useful methods when working with two-dimensional arrays in Java include:

- **length**: returns the size of the array.
- **clone():** creates a copy of the array.
- **equals():** compares two arrays with each other.

For example:

```java
int[][] arr1 = {{1, 2}, {3, 4}};
int[][] arr2 = {{1, 2}, {3, 4}};
if (Arrays.deepEquals(arr1, arr2)) {
    System.out.println("The two arrays are equal.");
}
```

In this lesson, we have learned about the methods that support string processing in Java, including methods such as length(), charAt(), concat(), contains(), equals(), and more. In addition, we have also learned how to work with one-dimensional and two-dimensional arrays in Java, including how to initialize, access, and traverse the elements in the array. It is important to understand how these methods and features work in order to use them effectively in your Java applications. These methods and features are very useful in processing data in real-world Java applications.
