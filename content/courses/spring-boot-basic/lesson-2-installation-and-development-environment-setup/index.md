---
draft: false
featuredImage: /courses/spring-boot-basic/lesson-2-installation-and-development-environment-setup.webp
images: [/courses/spring-boot-basic/lesson-2-installation-and-development-environment-setup.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [spring-boot-basic]
tags: [spring-boot, spring-framework, spring-boot-features, spring-boot-history]
title: Lesson 2 - Cài đặt và thiết lập môi trường phát triển Spring Boot
description: Trong bài học này, chúng ta sẽ cài đặt và thiết lập môi trường phát triển Spring Boot trên máy tính cá nhân của bạn, để bạn có thể bắt đầu phát triển ứng dụng Spring Boot, mà không cần phải cài đặt nhiều thứ.
date: 2024-06-29T19:23:30.000Z
weight: 2
---

##### 1. Hướng dẫn cài đặt Java Development Kit (JDK)

-   **Giới thiệu**: JDK là một bộ công cụ phát triển phần mềm được sử dụng để phát triển các ứng dụng Java. Nó bao gồm một trình biên dịch (javac), một máy ảo Java (JVM), và nhiều công cụ phát triển khác.

-   **Cài đặt JDK**:
    -   **Windows**:
        1.  Tải JDK từ [trang web chính thức của Oracle](https://www.oracle.com/java/technologies/javase-downloads.html) hoặc [OpenJDK](https://jdk.java.net/).
        2.  Chạy file cài đặt và làm theo các bước hướng dẫn.
        3.  Cấu hình biến môi trường JAVA_HOME:
            -   Tìm kiếm "Environment Variables" trên Windows và mở.
            -   Thêm biến hệ thống mới với tên `JAVA_HOME` và giá trị là đường dẫn đến thư mục cài đặt JDK.
            -   Thêm đường dẫn `%JAVA_HOME%\bin` vào biến môi trường `PATH`.
    -   **macOS**:
        1.  Tải JDK từ [trang web chính thức của Oracle](https://www.oracle.com/java/technologies/javase-downloads.html) hoặc sử dụng Homebrew để cài đặt OpenJDK.
        2.  Chạy lệnh: `brew install openjdk`.
        3.  Cấu hình biến môi trường JAVA_HOME:
            -   Mở file `.bash_profile`, `.zshrc` hoặc `.bashrc` và thêm dòng: `export JAVA_HOME=$(/usr/libexec/java_home)`.
        4.  Lưu và tải lại file cấu hình bằng lệnh: `source ~/.zshrc` hoặc `source ~/.bash_profile`.
    -   **Linux**:
        1.  Sử dụng package manager của hệ điều hành để cài đặt OpenJDK:
            -   Ubuntu: `sudo apt update && sudo apt install openjdk-21-jdk`
            -   CentOS: `sudo yum install java-21-openjdk-devel`
        2.  Xác nhận cài đặt bằng lệnh `java -version`.

##### 2. Cài đặt một Integrated Development Environment (IDE)

-   **Giới thiệu**: IDE là một công cụ giúp lập trình viên viết, kiểm thử và gỡ lỗi mã nguồn một cách hiệu quả. Một số IDE phổ biến cho Java bao gồm VSCode, IntelliJ IDEA, và Eclipse.

-   **Cài đặt VSCode**:
    1.  Tải VSCode từ [trang web chính thức của Microsoft](https://code.visualstudio.com/).
    2.  Cài đặt VSCode theo hướng dẫn.
    3.  Mở VSCode và cài đặt các extension cần thiết:
        -   Mở Extensions Marketplace bằng cách nhấn `Ctrl+Shift+X`.
        -   Tìm và cài đặt "Java Extension Pack".
        -   Tìm và cài đặt "Spring Boot Extension Pack".

##### 3. Cài đặt Maven/Gradle

-   **Giới thiệu**: Maven và Gradle là các công cụ quản lý dự án và tự động hóa xây dựng, giúp quản lý các dependencies và quá trình build ứng dụng Java.

-   **Cài đặt Maven**:
    1.  Tải Maven từ [trang web chính thức của Apache Maven](https://maven.apache.org/download.cgi).
    2.  Giải nén file tải về.
    3.  Cấu hình biến môi trường M2_HOME:
        -   Tìm kiếm "Environment Variables" trên Windows và mở.
        -   Thêm biến hệ thống mới với tên `M2_HOME` và giá trị là đường dẫn đến thư mục giải nén Maven.
        -   Thêm đường dẫn `%M2_HOME%\bin` vào biến môi trường `PATH`.

-   **Cài đặt Gradle**:
    1.  Tải Gradle từ [trang web chính thức của Gradle](https://gradle.org/install/).
    2.  Giải nén file tải về.
    3.  Cấu hình biến môi trường GRADLE_HOME:
        -   Tìm kiếm "Environment Variables" trên Windows và mở.
        -   Thêm biến hệ thống mới với tên `GRADLE_HOME` và giá trị là đường dẫn đến thư mục giải nén Gradle.
        -   Thêm đường dẫn `%GRADLE_HOME%\bin` vào biến môi trường `PATH`.

##### 4. Sử dụng Spring Initializr để tạo một dự án mới

-   **Giới thiệu**: Spring Initializr là một công cụ web giúp tạo nhanh một dự án Spring Boot với các cấu hình và dependencies cần thiết.

-   **Các bước thực hiện**:
    1.  Truy cập [Spring Initializr](https://start.spring.io/).
    2.  Chọn các thông số cho dự án:
        -   **Project Metadata**:
            -   Group: `com.example`
            -   Artifact: `demo`
            -   Name: `Demo`
            -   Description: `Demo project for Spring Boot`
            -   Package name: `com.example.demo`
            -   Packaging: `Jar`
            -   Java Version: `11`
        -   **Dependencies**: Thêm các dependencies cần thiết như:
            -   Spring Web
            -   Spring Data JPA
            -   H2 Database
    3.  Nhấn "Generate" để tải về dự án được tạo.
    4.  Giải nén file tải về vào thư mục làm việc.

##### 5. Khám phá cấu trúc dự án Spring Boot

-   **Giới thiệu cấu trúc dự án**:
    -   **src/main/java**: Chứa mã nguồn Java của ứng dụng.
    -   **src/main/resources**: Chứa các file cấu hình và tài nguyên của ứng dụng.
    -   **src/test/java**: Chứa các bài kiểm thử cho ứng dụng.
    -   **pom.xml** hoặc **build.gradle**: File cấu hình Maven hoặc Gradle, chứa thông tin về dependencies và các plugin cần thiết cho dự án.

-   **Các thành phần chính**:
    -   **Application class**: Lớp chính để khởi động ứng dụng Spring Boot (annotated with `@SpringBootApplication`).
    -   **Controller class**: Các lớp xử lý yêu cầu HTTP.
    -   **Repository class**: Các lớp làm việc với cơ sở dữ liệu.

##### Ví dụ

-   **Application class**:

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

-   **Controller class**:

```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
}
```

-   **Repository class**:

```java
package com.example.demo.repository;

import com.example.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```

-   **Entity class**:

```java
package com.example.demo.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;

    // getters and setters
}
```

Với các bước trên, học viên sẽ có thể cài đặt và thiết lập môi trường phát triển cho Spring Boot, tạo và khám phá cấu trúc một dự án Spring Boot cơ bản. Các ví dụ cụ thể sẽ giúp học viên hiểu rõ hơn về cách viết và tổ chức mã nguồn trong một dự án Spring Boot.
