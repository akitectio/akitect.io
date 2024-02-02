---
categories:
  - java
date: 2023-01-30T08:00:00+08:00
description: Java là một ngôn ngữ lập trình cấp cao, đa năng, hướng đối tượng và bảo mật được phát triển bởi James Gosling tại Sun Microsystems, Inc. vào năm 1991. Nó được chính thức gọi là Oak. Năm 1995, Sun Microsystem đổi tên thành Java. Năm 2009, Sun Microsystem tiếp quản bởi Oracle Corporation.
draft: false
featuredImage: /series/java-core-basic/java-core-lesson-1-overview-of-java-core.webp
images:
  - /series/java-core-basic/java-core-lesson-1-overview-of-java-core.webp
  - /java-core-lesson-1-gioi-thieu-tong-quan-ve-java-core/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - java-core-basics
tags:
  - Java
  - Java core
title: Java Core Lesson 1 - Giới thiệu tổng quan về Java Core
url: /java-core-lesson-1-gioi-thieu-tong-quan-ve-java-core
weight: 1
---

Java là một ngôn ngữ lập trình cấp cao, đa năng, hướng đối tượng và bảo mật được phát triển bởi James Gosling tại Sun Microsystems, Inc. vào năm 1991. Nó được chính thức gọi là Oak. Năm 1995, Sun Microsystem đổi tên thành Java. Năm 2009, Sun Microsystem tiếp quản bởi Oracle Corporation.

# Các tính năng của Java

1. Simple: Java là một ngôn ngữ đơn giản vì cú pháp của nó đơn giản, rõ ràng và dễ hiểu. Các khái niệm phức tạp và mơ hồ của C++ hoặc bị loại bỏ hoặc được triển khai lại trong Java. Ví dụ, nạp chồng con trỏ và toán tử không được sử dụng trong Java
2. Object-Oriented: Trong Java, mọi thứ đều ở dạng đối tượng. Nó có nghĩa là nó có một số dữ liệu và hành vi. Một chương trình phải có ít nhất một lớp và đối tượng.
3. Robust: Java nỗ lực kiểm tra lỗi trong thời gian chạy và thời gian biên dịch. Nó sử dụng một hệ thống quản lý bộ nhớ mạnh được gọi là bộ thu gom rác. Các tính năng xử lý ngoại lệ và thu gom rác làm cho nó trở nên mạnh mẽ.
4. Secure: Java là một ngôn ngữ lập trình an toàn vì nó không có con trỏ và chương trình rõ ràng chạy trong máy ảo. Java chứa một người quản lý bảo mật xác định quyền truy cập của các lớp Java.
5. Platform-Independent: Java đảm bảo rằng mã viết một lần và chạy ở mọi nơi. Mã byte này độc lập với nền tảng và có thể chạy trên bất kỳ máy nào.
6. Portable: Java bytecode có thể được mang đến bất kỳ nền tảng nào. Không có các tính năng phụ thuộc thực hiện. Tất cả mọi thứ liên quan đến lưu trữ đều được xác định trước, ví dụ, kích thước của các loại dữ liệu nguyên của nó.
7. High Performance: Java là một ngôn ngữ thông dịch. Java cho phép hiệu suất cao với việc sử dụng trình biên dịch Just-In-Time.
8. Distributed: Java cũng có các phương tiện kết nối mạng. Nó được thiết kế cho môi trường phân tán của internet vì nó hỗ trợ giao thức TCP/IP. Nó có thể chạy qua internet. EJB và RMI được sử dụng để tạo ra một hệ thống phân tán.
9. Multi-threaded: Java cũng hỗ trợ đa luồng. Nó có nghĩa là xử lý nhiều hơn một công việc một lúc.

# Vòng đời của java

Một ứng dụng java có 5 giai đoạn:

1. Code/Text Editor : developer sẽ thiết kế những câu lệnh, cấu trúc, thuật toán của chương trình và lưu vào tiệp .java
2. Java Compiler : Biên dịch chương trình thành tiệp bytecode
3. Class Loader: Đọc các tiệp chứa mã bytecode và lưu vào bộ nhớ RAM
4. Bytecode Verifiler : Đảm bảo các bytecode là hợp lệ và không có rủi ro về bảo mật theo chuẩn của java
5. Interpreter: Biên dịch chương trình thành mã máy, để chương trình thực hiện được và thực chi được chương trình

# Các thuật ngữ cơ bản trong Java

1. Class: Lớp là một bản thiết kế (kế hoạch) thể hiện của một lớp (đối tượng). Nó có thể được định nghĩa là một mẫu mô tả dữ liệu và hành vi được liên kết với phiên bản của nó.
2. Object: Đối tượng là một thể hiện của một lớp. Nó là một thực thể có hành vi và trạng thái.
   - Ví dụ: Một chiếc xe là một đối tượng có các là: thương hiệu, màu sắc và biển số.
   - Hành vi: Chạy trên đường
3. Method: Hành vi của một đối tượng là phương thức.
   - Ví dụ: Đồng hồ báo nhiên liệu cho biết lượng nhiên liệu còn lại trong xe.
4. Instance variables: Mỗi đối tượng có tập hợp các biến thể hiện duy nhất của riêng mình. Trạng thái của một đối tượng thường được tạo bởi các giá trị được gán cho các biến thể hiện này.

## Như vậy mình đã giới thiệu các thành phần chính trong Java Core, có thiếu gì anh em gớp ý thêm nhé, Cảm ơn anh em
