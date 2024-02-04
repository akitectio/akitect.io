---
categories:
  - java
date: 2023-01-30T08:00:00+08:00
description: Java is one of the most popular programming languages ​​today with many applications around the world. In this lesson, we will learn about naming rules, data types and casts, operators, control structures, and branching. Understanding and using these concepts fluently will help you become a good Java programmer. Start learning these concepts now!
draft: false
featuredImage: /series/java-core-basic/java-core-lesson-3-naming-rules-data-types-and-casts-operators-control-structures-and-branching.webp
images:
  - /series/java-core-basic/java-core-lesson-3-naming-rules-data-types-and-casts-operators-control-structures-and-branching.webp
  - /java-core-lesson-3-naming-rules-data-types-and-casts-operators-control-structures-and-branching/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - java-core-basics
tags:
  - Java
  - Java core
title: Lesson 3 - Quy tắc đặt tên, kiểu dữ liệu và ép kiểu, toán tử, cấu trúc điều khiển và rẽ nhánh
url: /java-core-lesson-3-quy-tac-dat-ten-kieu-du-lieu-va-ep-kieu-toan-tu-cau-truc-dieu-khien-va-re-nhanh
weight: 3
---

Java là một trong những ngôn ngữ lập trình phổ biến nhất hiện nay với rất nhiều ứng dụng trên khắp thế giới. Trong bài học này, chúng ta sẽ tìm hiểu về quy tắc đặt tên, kiểu dữ liệu và ép kiểu, toán tử, cấu trúc điều khiển và rẽ nhánh. Việc hiểu và sử dụng thành thạo các khái niệm này sẽ giúp bạn trở thành một lập trình viên Java giỏi. Hãy bắt đầu học các khái niệm này ngay bây giờ!

## Quy tắc đặt tên

Trong Java, việc đặt tên cho biến, hằng, phương thức và lớp rất quan trọng và ảnh hưởng đến tính đồng nhất và dễ đọc của mã. Dưới đây là các quy tắc đặt tên cơ bản trong Java:

1. Tên các biến và phương thức nên được bắt đầu bằng một chữ cái viết thường, và sau đó là các chữ cái tiếp theo viết hoa, không sử dụng khoảng trắng.

```java
// Khai báo biến đúng quy tắc đặt tên
int myVariable;
String myMethod();
// Khai báo biến sai quy tắc đặt tên
int My Variable; // có khoảng trắng
String 1stMethod; // bắt đầu bằng số
```

2. Tên lớp nên được bắt đầu bằng một chữ cái viết hoa và nên sử dụng kiểu đặt tên CamelCase (tức là, các từ tiếp theo sẽ được viết hoa).

```java
// Khai báo lớp đúng quy tắc đặt tên
public class MyClass {
    // code
}

// Khai báo lớp sai quy tắc đặt tên
public class my_class {
    // code
}
```

3. Tên hằng số nên được viết hoa toàn bộ và các từ cách nhau bằng dấu gạch dưới (\_).

```java
// Khai báo hằng số đúng quy tắc đặt tên
public static final int MAX_VALUE = 100;

// Khai báo hằng số sai quy tắc đặt tên
public static final int maxValue = 100; // không viết hoa toàn bộ
public static final int MAXVALUE = 100; // không sử dụng dấu gạch dưới
```

4. Tên gói (package) nên được viết bằng chữ thường, các từ cách nhau bằng dấu chấm.

```java
// Khai báo package đúng quy tắc đặt tên
package com.example.myproject;

public class MyClass {
    // code
}

// Khai báo package sai quy tắc đặt tên
package com.example.MyProject; // viết hoa chữ cái
```

5. Tên các biến, phương thức, lớp và hằng số nên được đặt một cách có ý nghĩa và mô tả được nội dung.

```java
// Khai báo biến có tên không rõ ràng
int x = 5;

// Khai báo biến có tên rõ ràng
int numberOfStudents = 5;

// Khai báo phương thức có tên không rõ ràng
public void doSomething(int a, int b) {
    // code
}

// Khai báo phương thức có tên rõ ràng
public void calculateSum(int firstNumber, int secondNumber) {
    // code
}

// Khai báo lớp có tên không rõ ràng
public class MyClass {
    // code
}

// Khai báo lớp có tên rõ ràng
public class Student {
    // code
}

// Khai báo hằng số có tên không rõ ràng
public static final int MAX = 100;

// Khai báo hằng số có tên rõ ràng
public static final int MAX_STUDENTS = 100;
```

6. Tránh sử dụng các từ khóa (keywords) của Java làm tên biến, phương thức, lớp hoặc hằng số.

```java
// Khai báo biến sử dụng từ khóa của Java làm tên biến
int int = 5;

// Khai báo phương thức sử dụng từ khóa của Java làm tên phương thức
public void public() {
    // code
}

// Khai báo lớp sử dụng từ khóa của Java làm tên lớp
public class String {
    // code
}

// Khai báo hằng số sử dụng từ khóa của Java làm tên hằng số
public static final int final = 100;
```

7. Tránh sử dụng các ký tự đặc biệt như @, $, # trong tên biến, phương thức, lớp hoặc hằng số.

```java
// Khai báo biến sử dụng ký tự đặc biệt trong tên biến
int my@variable = 5;

// Khai báo phương thức sử dụng ký tự đặc biệt trong tên phương thức
public void do$something() {
    // code
}

// Khai báo lớp sử dụng ký tự đặc biệt trong tên lớp
public class My#Class {
    // code
}

// Khai báo hằng số sử dụng ký tự đặc biệt trong tên hằng số
public static final int MY$CONSTANT = 100;
```

8. Sử dụng tiếng Anh khi đặt tên biến, phương thức, lớp hoặc hằng số.

```java

// Khai báo biến sử dụng tiếng Anh trong tên biến
int numberOfStudents = 5;

// Khai báo phương thức sử dụng tiếng Anh trong tên phương thức
public void calculateSum(int firstNumber, int secondNumber) {
    // code
}

// Khai báo lớp sử dụng tiếng Anh trong tên lớp
public class Student {
    // code
}

// Khai báo hằng số sử dụng tiếng Anh trong tên hằng số
public static final int MAX_STUDENTS = 100;

```

9. Các tên phải có độ dài hợp lý, không quá ngắn hoặc quá dài.

```
// Tên biến quá ngắn, không mô tả được nội dung
int x = 5;

// Tên phương thức quá dài, làm cho mã khó đọc
public void calculateSumOfTwoNumbersAndPrintResultOnConsole(int firstNumber, int secondNumber) {
    // code
}

// Tên lớp quá ngắn, không mô tả được nội dung
public class A {
    // code
}

// Tên hằng số quá dài, làm cho mã khó đọc
public static final int MAXIMUM_NUMBER_OF_STUDENTS_THAT_CAN_BE_ENROLLED_IN_A_SCHOOL = 1000;
```

> Những quy tắc đặt tên này giúp cho mã của bạn trở nên dễ đọc và hiểu hơn, đồng thời cũng giúp cho mã của bạn đáp ứng các tiêu chuẩn lập trình Java.

## Các kiểu dữ liệu và ép kiểu

Java là một ngôn ngữ lập trình có kiểu dữ liệu tĩnh, có nghĩa là bạn phải khai báo kiểu dữ liệu cho các biến trước khi sử dụng chúng.

1. Dữ liệu nguyên thuỷ (primitive data types)

Có 8 kiểu dữ liệu nguyên thuỷ, được liệt kê trong bảng sau:

| **Kiểu dữ liệu** | **Size (bits)** | **Phạm vi**                                              | **Giá trị mặc định** |
| ---------------- | --------------- | -------------------------------------------------------- | -------------------- |
| byte             | 8               | -128 đến 127                                             | 0                    |
| short            | 16              | -32,768 đến 32,767                                       | 0                    |
| int              | 32              | -2,147,483,648 đến 2,147,483,647                         | 0                    |
| long             | 64              | -9,223,372,036,854,775,808 đến 9,223,372,036,854,775,807 | 0L                   |
| float            | 32              | 1.4E-45 đến 3.4028235E38                                 | 0.0f                 |
| double           | 64              | 4.9E-324 đến 1.7976931348623157E308                      | 0.0d                 |
| char             | 16              | 0 đến 65,535                                             | '\u0000'             |
| boolean          | 1               | true hoặc false                                          |

2. Wrapper Class

Wrapper Class là các lớp lớp đối tượng trong Java được sử dụng để biểu diễn các kiểu dữ liệu nguyên thủy (primitive data types) dưới dạng đối tượng. Các wrapper class cung cấp các phương thức để thao tác với các giá trị của kiểu dữ liệu nguyên thủy và chuyển đổi giữa các kiểu dữ liệu nguyên thủy khác nhau. Các wrapper class cũng được sử dụng trong các Collection Framework của Java, vì các collection framework chỉ làm việc với các đối tượng, không làm việc trực tiếp với các kiểu dữ liệu nguyên thủy.

Các wrapper class tương ứng với các kiểu dữ liệu nguyên thủy trong Java như sau:

| **Kiểu nguyên thủy** | **Wrapper class** |
| -------------------- | ----------------- |
| byte                 | Byte              |
| short                | Short             |
| int                  | Integer           |
| long                 | Long              |
| float                | Float             |
| double               | Double            |
| char                 | Character         |
| boolean              | Boolean           |

Dưới đây là một số ví dụ về các wrapper class trong Java:

```java
// Chuyển một chuỗi thành số nguyên sử dụng Integer.parseInt()
String numStr = "123";
int num = Integer.parseInt(numStr);
System.out.println(num); // Output: 123

// Chuyển một số nguyên thành chuỗi sử dụng Integer.toString()
int value = 42;
String str = Integer.toString(value);
System.out.println(str); // Output: "42"

// Kiểm tra xem một chuỗi có đại diện cho giá trị boolean hợp lệ không sử dụng Boolean.parseBoolean()
String boolStr = "true";
boolean bool = Boolean.parseBoolean(boolStr);
System.out.println(bool); // Output: true

// Chuyển một ký tự thành chuỗi sử dụng Character.toString()
char ch = 'a';
String str = Character.toString(ch);
System.out.println(str); // Output: "a"

// Chuyển một chuỗi thành mảng ký tự sử dụng String.toCharArray()
String str = "hello";
char[] chars = str.toCharArray();
System.out.println(chars); // Output: "hello"
```

3. Ép kiểu (Casting)

Ép kiểu (Casting) là quá trình chuyển đổi giữa các kiểu dữ liệu khác nhau trong Java. Việc ép kiểu có thể được thực hiện bằng cách sử dụng toán tử đúng kiểu hoặc bằng cách sử dụng các phương thức chuyển đổi của các wrapper class. Có hai loại ép kiểu trong Java:

- Ép kiểu ngầm định (Implicit Casting): là quá trình chuyển đổi giá trị của kiểu dữ liệu có độ lớn nhỏ hơn sang kiểu dữ liệu có độ lớn lớn hơn mà không cần sử dụng toán tử đúng kiểu. Ví dụ:

```java
int i = 10;
double d = i; // Ép kiểu ngầm định
System.out.println(d); // Output: 10.0
```

- Ép kiểu tường minh (Explicit Casting): là quá trình chuyển đổi giá trị của kiểu dữ liệu có độ lớn lớn hơn sang kiểu dữ liệu có độ lớn nhỏ hơn. Trong quá trình này, ta cần sử dụng toán tử đúng kiểu để thực hiện việc ép kiểu. Ví dụ:

```java
double d = 10.5;
int i = (int) d; // Ép kiểu tường minh
System.out.println(i); // Output: 10
```

> Cần lưu ý rằng khi ép kiểu tường minh, giá trị của biến có thể bị mất mát nếu giá trị đó vượt quá phạm vi của kiểu dữ liệu đích.

## Các toán tử

Trong Java, có nhiều loại toán tử để thực hiện các phép tính và các phép so sánh. Các toán tử này có thể được chia thành các nhóm như sau:

1. Toán tử số học: được sử dụng để thực hiện các phép tính số học, bao gồm cộng (+), trừ (-), nhân (\*), chia (/), chia lấy dư (%), tăng (++), giảm (--), và các phép tính liên quan khác.

Ví dụ:

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

2. Toán tử gán: được sử dụng để gán giá trị cho một biến hoặc một biểu thức, bao gồm toán tử gán đơn (=) và các toán tử gán kết hợp với các toán tử số học khác.

Ví dụ:

```java
int a = 10;
a += 5;  // a = 15
a -= 3;  // a = 12
a *= 2;  // a = 24
a /= 3;  // a = 8
a %= 3;  // a = 2
```

3. Toán tử logic: được sử dụng để thực hiện các phép tính logic, bao gồm phép AND (&&), phép OR (||), phép NOT (!), và các phép tính liên quan khác.

Ví dụ:

```java
# Phép AND (&&): kết quả trả về true nếu cả hai biểu thức đều là true, ngược lại sẽ trả về false.
int a = 5;
int b = 10;
if (a > 0 && b > 0) {
    // Do something
}

# Phép OR (||): kết quả trả về true nếu ít nhất một trong hai biểu thức là true, ngược lại sẽ trả về false.
int a = 5;
int b = 10;
if (a > 0 || b < 0) {
    // Do something
}

# Phép NOT (!): kết quả trả về là true nếu biểu thức đang xét là false và ngược lại.
boolean flag = false;
if (!flag) {
    // Do something
}
```

4. Toán tử so sánh: được sử dụng để so sánh giá trị của hai biến hoặc biểu thức, bao gồm các phép so sánh bằng (==), khác (!=), lớn hơn (>), nhỏ hơn (<), lớn hơn hoặc bằng (>=), và nhỏ hơn hoặc bằng (<=).

Ví dụ:

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

5. Toán tử bitwise: được sử dụng để thực hiện các phép tính bitwise trên các số nguyên, bao gồm phép AND (&), phép OR (|), phép XOR (^), phép NOT (~), và các phép tính liên quan khác.

Ví dụ:

```java
#Phép AND (&)
int a = 5;  // binary: 0101
int b = 3;  // binary: 0011
int result = a & b;  // binary: 0001 (decimal: 1)

#Phép OR (|)
int a = 5;  // binary: 0101
int b = 3;  // binary: 0011
int result = a | b;  // binary: 0111 (decimal: 7)

#Phép XOR (^)
int a = 5;  // binary: 0101
int b = 3;  // binary: 0011
int result = a ^ b;  // binary: 0110 (decimal: 6)

#Phép NOT (~)
int a = 5;  // binary: 0101
int result = ~a;  // binary: 1010 (decimal: -6)
```

7. Toán tử di chuyển bit: được sử dụng để di chuyển các bit trong một số nguyên, bao gồm toán tử dịch trái (<<), toán tử dịch phải có dấu (>>), và toán tử dịch phải không dấu (>>>).

Ví dụ:

```java
int a = 10;  // binary: 1010
int b = a << 2;  // binary: 101000 (decimal: 40)
int c = a >> 2;  // binary: 10 (decimal: 2)
int d = a >>> 2;  // binary: 10 (decimal: 2)
```

## Cấu trúc điều khiển và rẽ nhánh

> Trong Java, cấu trúc điều khiển và rẽ nhánh được sử dụng để thực hiện các phép kiểm tra và quyết định trong chương trình. Các cấu trúc này cho phép chương trình thực hiện các lệnh khác nhau tùy thuộc vào các điều kiện được xác định.

Có hai loại cấu trúc điều khiển trong Java: cấu trúc điều khiển rẽ nhánh (branching control structure) và cấu trúc điều khiển vòng lặp (looping control structure).

1. Cấu trúc điều khiển rẽ nhánh bao gồm:

- If statement: sử dụng để kiểm tra một điều kiện và thực hiện các lệnh khác nhau tùy thuộc vào kết quả của điều kiện đó.
- If-else statement: cũng giống như If statement, nhưng nó có thêm một khối lệnh thực hiện khi điều kiện không đúng.
- Switch statement: sử dụng để kiểm tra một biến hoặc một biểu thức và thực hiện các lệnh khác nhau tùy thuộc vào giá trị của biến đó.

Ví dụ về cấu trúc điều khiển rẽ nhánh:

```java
int age = 20;

if (age < 18) {
  System.out.println("Bạn chưa đủ tuổi để xem nội dung này.");
} else if (age >= 18 && age < 21) {
  System.out.println("Bạn có thể xem nội dung này nhưng không được uống rượu bia.");
} else {
  System.out.println("Bạn có thể xem nội dung này và được uống rượu bia.");
}
```

2. Cấu trúc điều khiển vòng lặp bao gồm:

- For loop: sử dụng để lặp lại một khối lệnh một số lần nhất định.
- While loop: sử dụng để lặp lại một khối lệnh trong khi một điều kiện cụ thể đúng.
- Do-while loop: giống như While loop, nhưng với một khối lệnh được thực hiện ít nhất một lần trước khi kiểm tra điều kiện.

Ví dụ về cấu trúc điều khiển vòng lặp:

```
int i;

for (i = 0; i < 10; i++) {
  System.out.println("Giá trị của i là: " + i);
}

i = 0;
while (i < 10) {
  System.out.println("Giá trị của i là: " + i);
  i++;
}

i = 0;
do {
  System.out.println("Giá trị của i là: " + i);
  i++;
} while (i < 10);

```

#### Trong bài viết này, chúng ta đã tìm hiểu về các kiểu dữ liệu, ép kiểu, các wrapper class và các toán tử trong Java. Chúng ta cũng đã xem xét các cấu trúc điều khiển, bao gồm các câu điều kiện và vòng lặp. Hi vọng những kiến thức này sẽ giúp bạn hiểu và phát triển các ứng dụng Java của mình một cách tốt hơn.
