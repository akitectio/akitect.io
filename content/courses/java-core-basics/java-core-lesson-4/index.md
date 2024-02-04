---
categories:
  - java
date: 2023-01-30T08:00:00+08:00
description: Xử lý String, Mảng 1 chiều và Mảng 2 chiều là những khái niệm quan trọng trong lập trình Java. Trong phần này, chúng ta sẽ tìm hiểu về cách xử lý chuỗi trong Java và cách sử dụng mảng 1 chiều và mảng 2 chiều để lưu trữ và xử lý dữ liệu. Chúng ta sẽ đi vào từng khái niệm chi tiết, cung cấp các ví dụ cụ thể và giải thích cách sử dụng chúng trong các ứng dụng thực tế.
draft: false
featuredImage: /series/java-core-basic/java-core-lesson-4-string-processing-1-dimensional-array-2-dimensional-array.webp
images:
  - /series/java-core-basic/java-core-lesson-4-string-processing-1-dimensional-array-2-dimensional-array.webp
  - /java-core-lesson-4-string-processing-1-dimensional-array-2-dimensional-array/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - java-core-basics
tags:
  - Java
  - Java core
  - 1-dimensional
  - 2-dimensional
title: Lesson 4 - Xử lý String, Mảng 1 chiều, Mảng 2 chiều
url: /java-core-lesson-4-xu-ly-string-mang-1-chieu-mang-2-chieu
weight: 4
---

Xử lý String, Mảng 1 chiều và Mảng 2 chiều là những khái niệm quan trọng trong lập trình Java. Trong phần này, chúng ta sẽ tìm hiểu về cách xử lý chuỗi trong Java và cách sử dụng mảng 1 chiều và mảng 2 chiều để lưu trữ và xử lý dữ liệu. Chúng ta sẽ đi vào từng khái niệm chi tiết, cung cấp các ví dụ cụ thể và giải thích cách sử dụng chúng trong các ứng dụng thực tế.

## Xử lý String

Trong Java, String là một kiểu dữ liệu đặc biệt để lưu trữ các chuỗi ký tự. Các chuỗi này có thể được xử lý thông qua nhiều phương thức hỗ trợ trong lớp String.

Để khai báo một biến kiểu String, chúng ta có thể sử dụng cú pháp sau:

```java

String str;
#hoặc

String str = "Hello world!";

```

Các phương thức hỗ trợ xử lý chuỗi trong Java bao gồm:

| **Phương thức**                              | **Mô tả**                                                                               |
| -------------------------------------------- | --------------------------------------------------------------------------------------- |
| length()                                     | Trả về độ dài của chuỗi.                                                                |
| charAt(int index)                            | Trả về độ dài của chuỗi.                                                                |
| concat(String str)                           | Nối chuỗi đang gọi với chuỗi được chỉ định.                                             |
| contains(CharSequence s)                     | Kiểm tra xem chuỗi có chứa chuỗi con được chỉ định không.                               |
| equals(Object obj)                           | So sánh hai chuỗi với nhau.                                                             |
| equalsIgnoreCase(String anotherString)       | So sánh hai chuỗi với nhau mà không phân biệt chữ hoa và chữ thường.                    |
| indexOf(int ch)                              | Trả về chỉ số đầu tiên của ký tự được chỉ định trong chuỗi.                             |
| lastIndexOf(int ch)                          | Trả về chỉ số cuối cùng của ký tự được chỉ định trong chuỗi.                            |
| substring(int beginIndex, int endIndex)      | Trích xuất một phần của chuỗi bắt đầu từ vị trí chỉ định.                               |
| toLowerCase()                                | Chuyển đổi toàn bộ chuỗi thành chữ thường.                                              |
| toUpperCase()                                | Chuyển đổi toàn bộ chuỗi thành chữ hoa.                                                 |
| trim()                                       | Loại bỏ khoảng trắng ở đầu và cuối chuỗi.                                               |
| replace(char oldChar, char newChar)          | Thay thế tất cả các trường hợp của ký tự cũ trong chuỗi với ký tự mới.                  |
| replaceAll(String regex, String replacement) | Thay thế tất cả các chuỗi con khớp với biểu thức chính quy bằng chuỗi mới.              |
| split(String regex)                          | Phân tách chuỗi thành một mảng các chuỗi con bằng cách sử dụng một biểu thức chính quy. |

Ví dụ:

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

## Mảng 1 chiều

Mảng 1 chiều (1D array) là một cấu trúc dữ liệu trong lập trình, được sử dụng để lưu trữ một tập hợp các giá trị cùng kiểu dữ liệu. Mảng 1 chiều được đánh chỉ số bắt đầu từ 0 và kích thước của nó được xác định tại thời điểm khai báo.

Để khai báo một mảng 1 chiều trong Java, bạn có thể sử dụng cú pháp sau:

```css
<kiểu dữ liệu>[] <tên mảng> = new <kiểu dữ liệu>[<kích thước>];
```

Ví dụ: để khai báo một mảng 1 chiều chứa các số nguyên có 5 phần tử, bạn có thể sử dụng cú pháp sau:

```java
int[] numbers = new int[5];
```

Sau khi khai báo, bạn có thể gán giá trị cho các phần tử của mảng bằng cách truy cập vào chỉ số của phần tử đó trong mảng. Ví dụ:

```java
numbers[0] = 1;
numbers[1] = 2;
numbers[2] = 3;
numbers[3] = 4;
numbers[4] = 5;
```

Bạn cũng có thể truy cập giá trị của các phần tử trong mảng bằng cách sử dụng chỉ số tương ứng. Ví dụ:

```java
int x = numbers[2]; // lấy giá trị của phần tử thứ 3 trong mảng, có giá trị là 3
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

## Mảng 2 chiều

Mảng 2 chiều trong Java là một mảng có kích thước nhiều hơn so với mảng 1 chiều. Mảng 2 chiều có thể được coi như một bảng (hay một ma trận) với các phần tử được sắp xếp trong các hàng và cột. Mỗi phần tử trong mảng 2 chiều được truy cập thông qua chỉ số của hàng và cột.

Khai báo một mảng 2 chiều trong Java như sau:

```css
dataType[][] arrayName = new dataType[rowSize][colSize];
```

Trong đó:

- **dataType** là kiểu dữ liệu của các phần tử trong mảng.
- **arrayName** là tên của mảng.
- **rowSize** là kích thước của hàng (số lượng phần tử trong hàng) trong mảng 2 chiều.
- **colSize** là kích thước của cột (số lượng phần tử trong cột) trong mảng 2 chiều.

Ví dụ, để khai báo một mảng 2 chiều có kích thước 3 hàng x 4 cột với kiểu dữ liệu là int, ta có thể sử dụng đoạn code sau:

```java
int[][] arr = new int[3][4];
```

Để truy cập và gán giá trị cho các phần tử trong mảng 2 chiều, ta sử dụng cú pháp:

```java
arr[rowIndex][colIndex] = value;
```

Trong đó:

- **rowIndex** là chỉ số của hàng (bắt đầu từ 0).
- **colIndex** là chỉ số của cột (bắt đầu từ 0).
- **value** là giá trị muốn gán cho phần tử có chỉ số tương ứng.

Ví Dụ:

```java
arr[0][0] = 1;  // gán giá trị 1 cho phần tử ở hàng 0, cột 0
int x = arr[1][2];  // truy cập giá trị của phần tử ở hàng 1, cột 2 và lưu vào biến x
```

Một số phương thức hữu ích khi làm việc với mảng 2 chiều trong Java bao gồm:

- **length:** trả về kích thước của mảng.
- **clone():** tạo một bản sao của mảng.
- **equals():** so sánh hai mảng với nhau.

Ví dụ:

```java
int[][] arr1 = {{1, 2}, {3, 4}};
int[][] arr2 = {{1, 2}, {3, 4}};
if (Arrays.deepEquals(arr1, arr2)) {
    System.out.println("Hai mảng bằng nhau.");
}
```

- Trong bài học này, chúng ta đã tìm hiểu về các phương thức hỗ trợ xử lý chuỗi trong Java, bao gồm các phương thức như length(), charAt(), concat(), contains(), equals(), và nhiều hơn nữa. Ngoài ra, chúng ta cũng đã học cách làm việc với mảng 1 chiều và mảng 2 chiều trong Java, bao gồm cách khởi tạo, truy cập và duyệt các phần tử trong mảng.Điều quan trọng là phải hiểu rõ cách hoạt động của các phương thức và tính năng này để có thể sử dụng chúng hiệu quả trong các ứng dụng của mình. Các phương thức và tính năng này rất hữu ích trong việc xử lý dữ liệu trong các ứng dụng Java thực tế.
