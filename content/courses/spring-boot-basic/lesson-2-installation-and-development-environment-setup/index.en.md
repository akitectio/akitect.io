---
draft: false
featuredImage: /courses/spring-boot-basic/lesson-2-installation-and-development-environment-setup.webp
images: [/courses/spring-boot-basic/lesson-2-installation-and-development-environment-setup.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [spring-boot-basic]
tags: [spring-boot, spring-framework, spring-boot-features, spring-boot-history]
title: Lesson 2 - Installation and Development Environment Setup
description: In this lesson, we will install and set up the Spring Boot development environment on your personal computer, so you can start developing Spring Boot applications without having to install many things.
date: 2024-06-29T19:23:30.000Z
weight: 2
---

##### 1. Java Development Kit (JDK) Installation Guide

-   **Introduction**: The JDK is a software development kit used to develop Java applications. It includes a compiler (javac), a Java Virtual Machine (JVM), and several other development tools.

-   **JDK Installation**:
    -   **Windows**:
        1.  Download the JDK from the [official Oracle website](https://www.oracle.com/java/technologies/javase-downloads.html) or [OpenJDK](https://jdk.java.net/).
        2.  Run the installer and follow the instructions.
        3.  Configure the JAVA_HOME environment variable:
            -   Search for "Environment Variables" in Windows and open it.
            -   Add a new system variable named `JAVA_HOME` with the value being the path to the JDK installation directory.
            -   Add `%JAVA_HOME%\bin` to the `PATH` environment variable.
    -   **macOS**:
        1.  Download the JDK from the [official Oracle website](https://www.oracle.com/java/technologies/javase-downloads.html) or use Homebrew to install OpenJDK.
        2.  Run the command: `brew install openjdk`.
        3.  Configure the JAVA_HOME environment variable:
            -   Open the `.bash_profile`, `.zshrc`, or `.bashrc` file and add the line: `export JAVA_HOME=$(/usr/libexec/java_home)`.
        4.  Save and reload the configuration file by running: `source ~/.zshrc` or `source ~/.bash_profile`.
    -   **Linux**:
        1.  Use the package manager of your operating system to install OpenJDK:
            -   Ubuntu: `sudo apt update && sudo apt install openjdk-11-jdk`
            -   CentOS: `sudo yum install java-11-openjdk-devel`
        2.  Verify the installation by running `java -version`.

##### 2. Installing an Integrated Development Environment (IDE)

-   **Introduction**: An IDE is a tool that helps developers write, test, and debug code efficiently. Popular IDEs for Java include VSCode, IntelliJ IDEA, and Eclipse.

-   **Installing VSCode**:
    1.  Download VSCode from the [official Microsoft website](https://code.visualstudio.com/).
    2.  Install VSCode by following the instructions.
    3.  Open VSCode and install the necessary extensions:
        -   Open the Extensions Marketplace by pressing `Ctrl+Shift+X`.
        -   Search for and install the "Java Extension Pack".
        -   Search for and install the "Spring Boot Extension Pack".

##### 3. Installing Maven/Gradle

-   **Introduction**: Maven and Gradle are project management and build automation tools that help manage dependencies and the build process for Java applications.

-   **Installing Maven**:
    1.  Download Maven from the [official Apache Maven website](https://maven.apache.org/download.cgi).
    2.  Extract the downloaded file.
    3.  Configure the M2_HOME environment variable:
        -   Search for "Environment Variables" in Windows and open it.
        -   Add a new system variable named `M2_HOME` with the value being the path to the extracted Maven directory.
        -   Add `%M2_HOME%\bin` to the `PATH` environment variable.

-   **Installing Gradle**:
    1.  Download Gradle from the [official Gradle website](https://gradle.org/install/).
    2.  Extract the downloaded file.
    3.  Configure the GRADLE_HOME environment variable:
        -   Search for "Environment Variables" in Windows and open it.
        -   Add a new system variable named `GRADLE_HOME` with the value being the path to the extracted Gradle directory.
        -   Add `%GRADLE_HOME%\bin` to the `PATH` environment variable.

##### 4. Using Spring Initializr to Create a New Project

-   **Introduction**: Spring Initializr is a web-based tool that quickly generates a Spring Boot project with the necessary configurations and dependencies.

-   **Steps to Create a New Project**:
    1.  Visit [Spring Initializr](https://start.spring.io/).
    2.  Choose the project parameters:
        -   **Project Metadata**:
            -   Group: `com.example`
            -   Artifact: `demo`
            -   Name: `Demo`
            -   Description: `Demo project for Spring Boot`
            -   Package name: `com.example.demo`
            -   Packaging: `Jar`
            -   Java Version: `11`
        -   **Dependencies**: Add necessary dependencies such as:
            -   Spring Web
            -   Spring Data JPA
            -   H2 Database
    3.  Click "Generate" to download the generated project.
    4.  Extract the downloaded file into the working directory.

##### 5. Exploring the Spring Boot Project Structure

-   **Introduction to the Project Structure**:
    -   **src/main/java**: Contains the Java source code of the application.
    -   **src/main/resources**: Contains the configuration files and resources of the application.
    -   **src/test/java**: Contains the test cases for the application.
    -   **pom.xml** or **build.gradle**: The Maven or Gradle configuration file, containing information about dependencies and required plugins for the project.

-   **Key Components**:
    -   **Application class**: The main class to launch the Spring Boot application (annotated with `@SpringBootApplication`).
    -   **Controller class**: Classes handling HTTP requests.
    -   **Repository class**: Classes interacting with the database.

#### Example

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

-   **Explanation**:
    -   `@SpringBootApplication` is a special annotation that combines three annotations: `@EnableAutoConfiguration`, `@ComponentScan`, and `@Configuration`. This annotation helps Spring Boot automatically configure the application based on the dependencies present in the classpath.
    -   `SpringApplication.run(DemoApplication.class, args)` launches the Spring Boot application by creating an `ApplicationContext` and starting all the declared beans.

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

-   **Explanation**:
    -   `@RestController` is a special annotation that combines `@Controller` and `@ResponseBody`, allowing you to handle HTTP requests and return data in JSON or XML format.
    -   `@GetMapping("/hello")` maps the GET request to the `hello()` method, which returns the string "Hello, World!".

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

-   **Explanation**:
    -   `@Repository` is an annotation that marks the class as a data access object (DAO).
    -   `JpaRepository<User, Long>` provides standard CRUD methods for the `User` entity.

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

-   **Explanation**:
    -   `@Entity` marks the class as a JPA entity.
    -   `@Id` and `@GeneratedValue(strategy = GenerationType.IDENTITY)` annotate the primary key field and specify that its value should be generated automatically.

With these steps, learners will be able to install and set up the development environment for Spring Boot, create and explore the structure of a basic Spring Boot project. The specific examples will help learners understand how to write and organize code in a Spring Boot project.
