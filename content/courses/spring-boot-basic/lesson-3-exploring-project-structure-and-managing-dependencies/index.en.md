---
draft: false
featuredImage: /courses/spring-boot-basic/lesson-3-exploring-project-structure-and-managing-dependencies.webp
images: [/courses/spring-boot-basic/lesson-3-exploring-project-structure-and-managing-dependencies.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [spring-boot-basic]
tags: [spring-boot, spring-framework, spring-boot-features, spring-boot-history]
title: Lesson 3 - Exploring Project Structure and Managing Dependencies
description: In this lesson, we will explore the default project structure of a Spring Boot project and how to manage dependencies in a Spring Boot project, helping you understand how Spring Boot projects are organized and dependencies are managed.
date: 2024-06-30T19:23:30.000Z
weight: 3
---

##### 1. Spring Boot Project Structure

-   **Introduction**: A Spring Boot project is organized in a way that facilitates easy management of source code, resources, and configuration files.

-   **Key components**:
    -   **src/main/java**: This directory contains the main source code of the application. This is where you will write Java classes such as controllers, services, repositories, and entities.
    -   **src/main/resources**: This directory contains the resources and configuration files of the application, such as properties files, XML configuration files, and static files like HTML, CSS, and JavaScript.
    -   **src/test/java**: This directory contains the test source code. This is where you will write unit tests and integration tests.
    -   **pom.xml** or **build.gradle**: The Maven or Gradle configuration file that contains information about dependencies and necessary plugins for the project.

-   **Standard directory structure**:

Below is the updated description of the Spring Boot project directory structure in Vietnamese, along with detailed examples for Java classes in the project:

### Detailed Spring Boot Project Directory Structure:

```plaintext
my-spring-boot-app
├── src
│   ├── main
│   │   ├── java
│   │   │   └── io
│   │   │       └── akitect
│   │   │           ├── Application.java                # Main Spring Boot application class
│   │   │           ├── ApplicationConstants.java       # Constants used throughout the application
│   │   │           ├── configuration                   # Configuration-related classes
│   │   │           │   └── ApplicationConfiguration.java
│   │   │           ├── controller                      # REST Controllers handling requests
│   │   │           │   └── ApplicationController.java
│   │   │           ├── dao                             # Data access objects for interacting with the DB
│   │   │           │   ├── impl                        # Implementations of DAO interfaces
│   │   │           │   │   └── ApplicationDaoImpl.java
│   │   │           │   └── ApplicationDao.java
│   │   │           ├── dto                             # Data transfer objects for wrapping data
│   │   │           │   └── ApplicationDto.java
│   │   │           ├── service                         # Service classes for business logic
│   │   │           │   ├── impl                        # Implementations of service interfaces
│   │   │           │   │   └── ApplicationServiceImpl.java
│   │   │           │   └── ApplicationService.java
│   │   │           ├── util                            # Utility classes with static methods
│   │   │           │   └── ApplicationUtils.java
│   │   │           └── validation                      # Validation logic for input data
│   │   │               ├── impl                        # Implementations of validation interfaces
│   │   │               │   └── ApplicationValidationImpl.java
│   │   │               └── ApplicationValidation.java
│   │   └── resources                                   # Contains all resources such as properties and XML configuration
│   │       ├── application.properties                  # Application properties for configuration settings
│   │       ├── static                                  # Static resources (CSS, JS, images)
│   │       └── templates                               # Template files for views
│   └── test
│       ├── java
│       │   └── io
│       │       └── akitect
│       │           └── ApplicationTests.java           # Tests for the application
├── mvnw                                                # Maven wrapper script for Unix-based systems
├── mvnw.cmd                                            # Maven wrapper script for Windows
├── pom.xml                                             # Maven configuration file
└── README.md                                           # Project documentation
```

### Components in the Directory Structure:

-   **Application.java**: The main class to bootstrap the Spring Boot application.
-   **ApplicationConstants.java**: Defines application-wide constants.
-   **configuration**: Contains configuration classes, such as security or database configuration.
-   **controller**: Contains controller classes that handle user requests.
-   **dao**: Defines interfaces and implementation classes for data access.
-   **dto**: Defines Data Transfer Objects used to transfer data between classes.
-   **service**: Contains business logic, handling the main application logic.
-   **util**: Provides utility classes that can be used by various components of the application.
-   **validation**: Handles input data validation logic, ensuring data meets the specified requirements.

This directory structure ensures that the application is organized and easy to maintain, with each component having clear responsibilities, separated step-by-step application logic to promote cleaner code and easier management.

### Example

1.  **Main Application Class (Application)**:

```java
package io.akitect;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * The main starting point of the Spring Boot application.
 */
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

2.  **Application Constants Class (ApplicationConstants)**:

```java
package io.akitect;

/**
 * Defines constants used throughout the application.
 */
public class ApplicationConstants {
    public static final String API_VERSION = "/api/v1";
    public static final int MAX_PAGE_SIZE = 50;
}
```

3.  **Application Configuration Class (ApplicationConfiguration)**:

```java
package io.akitect.configuration;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

/**
 * Application configuration.
 */
@Configuration
public class ApplicationConfiguration {
    @Bean
    public BCryptPasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

4.  **Controller Class (ApplicationController)**:

```java
package io.akitect.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * Handles incoming HTTP requests.
 */
@RestController
@RequestMapping(ApplicationConstants.API_VERSION + "/users")
public class ApplicationController {

    @GetMapping("/hello")
    public String sayHello() {
        return "Hello from API!";
    }
}
```

5.  **Service Interface (Service)**:

```java
package io.akitect.service;

/**
 * Service interface for business logic.
 */
public interface ApplicationService {
    String performLogic();
}
```

6.  **Service Implementation Class (Service)**:

```java
package io.akitect.service.impl;

import io.akitect.service.ApplicationService;
import org.springframework.stereotype.Service;

/**
 * Implementation of ApplicationService.
 */
@Service
public class ApplicationServiceImpl implements ApplicationService {

    @Override
    public String performLogic() {
        return "Business logic performed";
    }
}
```

7.  **Data Access Object Interface (DAO)**:

```java
package io.akitect.dao;

/**
 * DAO interface for data access operations.
 */
public interface ApplicationDao {
    void saveData();
}
```

8.  **DAO Implementation Class**:

```java
package io.akitect.dao.impl;

import io.akitect.dao.ApplicationDao;
import org.springframework.stereotype.Repository;

/**
 * Implementation of ApplicationDao.
 */
@Repository
public class ApplicationDaoImpl implements ApplicationDao {

    @Override
    public void saveData() {
        // Logic to save data
        System.out.println("Data saved!");
    }
}
```

9.  **Data Transfer Object Class (DTO)**:

```java
package io.akitect.dto;

/**
 * Data transfer object to encapsulate user data.
 */
public class ApplicationDto {
    private String username;
    private String email;

    // Getters and Setters
    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```

10. **Utility Class (Utility)**:

```java
package io.akitect.util;

import java.time.LocalDate;
import java.time.format.DateTimeFormatter;

/**
 * Utility class providing common functionalities.
 */
public class ApplicationUtils {
    public static String formatDate(LocalDate date) {
        return date.format(DateTimeFormatter.ISO_DATE);
    }
}
```

11. **Validation Interface (Validation)**:

```java
package io.akitect.validation;

/**
 * Interface for validation logic.
 */
public interface ApplicationValidation {
    boolean validateData(Object data);
}
```

12. **Validation Implementation Class (Validation)**:

```java
package io.akitect.validation.impl;

import io.akitect.validation.ApplicationValidation;

/**
 * Implementation of the ApplicationValidation interface.
 */
public class ApplicationValidationImpl implements ApplicationValidation {

    @Override
    public boolean validateData(Object data) {
        // Perform data validation
        return data != null; // Simple example
    }
}
```

These examples are designed to illustrate the code structure in an actual Spring Boot project. Each class and interface plays a specific role in the application architecture, ensuring separation of concerns and clean, maintainable code.

##### 2. Dependency Management with Maven and Gradle

-   **Introduction**: Spring Boot uses Maven or Gradle for dependency management and automatic bean configuration. This simplifies the configuration and management of necessary libraries for the application.

-   **Maven**:

-   **pom.xml**: Maven configuration file, containing information about dependencies, plugins, and build parameters.

-   **Adding a dependency**: To add a dependency to the project, you just need to add a `<dependency>` tag to the pom.xml file.

-   **Example**:

```xml
<dependencies>
    <!-- Dependency for Spring Data JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <!-- Dependency for Spring Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

-   **Gradle**:

-   **build.gradle**: Gradle configuration file, containing information about dependencies, plugins, and build parameters.

-   **Adding a dependency**: To add a dependency to the project, you just need to add an `implementation` line to the dependencies section in the build.gradle file.

-   **Example**:

```groovy
dependencies {
    // Dependency for Spring Data JPA
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    // Dependency for Spring Web
    implementation 'org.springframework.boot:spring-boot-starter-web'
}
```

##### 3. Non-Automatic Dependency Configuration

-   **Introduction**: Not all dependencies are automatically configured by Spring Boot. Sometimes, you will need to manually configure beans and other settings for those dependencies.

-   **Example**: Adding and configuring dependencies spring-boot-starter-data-jpa and spring-boot-starter-web.

-   **Adding dependencies**: Add the dependencies to the pom.xml or build.gradle file as presented above.

-   **Application configuration**:

    -   **application.properties**:

```properties
# Configuring the datasource for H2 database
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

# Configuring JPA properties
spring.jpa.hibernate.ddl-auto=update
```

-   **Entity class**:

```java
package io.akitect.demo.model;

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

    // Getters and setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

-   **Repository class**:

```java
package io.akitect.demo.repository;

import io.akitect.demo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```

-   **Controller class**:

```java
package io.akitect.demo.controller;

import io.akitect.demo.model.User;
import io.akitect.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class UserController {

    @Autowired
    private UserRepository userRepository;

    @GetMapping("/users")
    public List<User> getUsers() {
        return userRepository.findAll();
    }

    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        return userRepository.save(user);
    }
}
```

With these specific contents and examples, students will grasp the structure of a Spring Boot project, how to manage dependencies and automatically configure beans, as well as how to add and configure non-automatic dependencies.
