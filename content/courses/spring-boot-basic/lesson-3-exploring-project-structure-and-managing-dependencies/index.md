---
draft: false
featuredImage: /courses/spring-boot-basic/lesson-3-exploring-project-structure-and-managing-dependencies.webp
images: [/courses/spring-boot-basic/lesson-3-exploring-project-structure-and-managing-dependencies.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [spring-boot-basic]
tags: [spring-boot, spring-framework, spring-boot-features, spring-boot-history]
title: Lesson 3 - Cấu trúc dự án và quản lý dependencies
description: Trong bài học này, chúng ta sẽ khám phá cấu trúc dự án Spring Boot mặc định và cách quản lý dependencies trong dự án Spring Boot, giúp bạn hiểu rõ hơn về cách dự án Spring Boot được tổ chức và quản lý dependencies.
date: 2024-06-30T19:23:30.000Z
weight: 3
---

##### 1. Cấu trúc thư mục của dự án Spring Boot

-   **Giới thiệu**: Một dự án Spring Boot được tổ chức theo cách giúp dễ dàng quản lý mã nguồn, tài nguyên và các file cấu hình.

-   **Các thành phần chính**:
    -   **src/main/java**: Thư mục chứa mã nguồn chính của ứng dụng. Đây là nơi bạn sẽ viết các lớp Java như các controller, service, repository, và các entity.
    -   **src/main/resources**: Thư mục chứa các tài nguyên và cấu hình của ứng dụng như các file properties, file cấu hình XML, và các file tĩnh như HTML, CSS, và JavaScript.
    -   **src/test/java**: Thư mục chứa mã nguồn kiểm thử. Đây là nơi bạn sẽ viết các bài kiểm thử đơn vị (unit tests) và kiểm thử tích hợp (integration tests).
    -   **pom.xml** hoặc **build.gradle**: File cấu hình của Maven hoặc Gradle, chứa thông tin về phụ thuộc và các plugin cần thiết cho dự án.

-   **Cấu trúc thư mục chuẩn**:

Dưới đây là mô tả cấu trúc thư mục dự án Spring Boot đã được cập nhật bằng tiếng Việt, cùng với các ví dụ chi tiết cho các lớp Java trong dự án:

### Cấu trúc Thư mục Dự án Spring Boot Chi Tiết:

```plaintext
my-spring-boot-app
├── src
│   ├── main
│   │   ├── java
│   │   │   └── io
│   │   │       └── akitect
│   │   │           ├── Application.java                # Lớp ứng dụng chính Spring Boot
│   │   │           ├── ApplicationConstants.java       # Các hằng số được sử dụng xuyên suốt ứng dụng
│   │   │           ├── configuration                   # Các lớp liên quan đến cấu hình
│   │   │           │   └── ApplicationConfiguration.java
│   │   │           ├── controller                      # Các Controller REST xử lý các yêu cầu đến
│   │   │           │   └── ApplicationController.java
│   │   │           ├── dao                             # Các đối tượng truy cập dữ liệu cho tương tác DB
│   │   │           │   ├── impl                        # Cài đặt của các giao diện DAO
│   │   │           │   │   └── ApplicationDaoImpl.java
│   │   │           │   └── ApplicationDao.java
│   │   │           ├── dto                             # Các đối tượng truyền dữ liệu cho bao bọc dữ liệu
│   │   │           │   └── ApplicationDto.java
│   │   │           ├── service                         # Lớp service cho logic nghiệp vụ
│   │   │           │   ├── impl                        # Cài đặt của các giao diện service
│   │   │           │   │   └── ApplicationServiceImpl.java
│   │   │           │   └── ApplicationService.java
│   │   │           ├── util                            # Các lớp tiện ích với các phương thức tĩnh
│   │   │           │   └── ApplicationUtils.java
│   │   │           └── validation                      # Logic xác thực cho dữ liệu đầu vào
│   │   │               ├── impl                        # Cài đặt của các giao diện xác thực
│   │   │               │   └── ApplicationValidationImpl.java
│   │   │               └── ApplicationValidation.java
│   │   └── resources                                   # Chứa tất cả các tài nguyên như thuộc tính và cấu hình XML
│   │       ├── application.properties                  # Thuộc tính ứng dụng cho các cài đặt cấu hình
│   │       ├── static                                  # Các tài nguyên tĩnh (CSS, JS, hình ảnh)
│   │       └── templates                               # Các file mẫu cho views
│   └── test
│       ├── java
│       │   └── io
│       │       └── akitect
│       │           └── ApplicationTests.java           # Các bài kiểm tra cho ứng dụng
├── mvnw                                                # Kịch bản wrapper Maven cho hệ điều hành Unix
├── mvnw.cmd                                            # Kịch bản wrapper Maven cho Windows
├── pom.xml                                             # File cấu hình Maven
└── README.md                                           # Tài liệu mô tả dự án
```

### Các Thành Phần Trong Cấu Trúc Thư Mục:

-   **Application.java**: Lớp chính để khởi động ứng dụng Spring Boot.
-   **ApplicationConstants.java**: Định nghĩa các hằng số toàn ứng dụng.
-   **configuration**: Chứa các lớp cấu hình, ví dụ như cấu hình bảo mật hoặc cơ sở dữ liệu.
-   **controller**: Chứa các lớp điều khiển, xử lý các yêu cầu từ người dùng.
-   **dao**: Định nghĩa các giao diện và lớp thực thi cho truy cập dữ liệu.
-   **dto**: Định nghĩa các Data Transfer Objects, sử dụng để chuyển dữ liệu giữa các lớp.
-   **service**: Chứa logic nghiệp vụ, xử lý các yêu cầu logic chính của ứng dụng.
-   **util**: Cung cấp các tiện ích có thể được sử dụng bởi nhiều thành phần khác của ứng dụng.
-   **validation**: Xử lý logic xác thực dữ liệu đầu vào, đảm bảo dữ liệu đáp ứng các yêu cầu đặt ra.

Cấu trúc thư mục này giúp đảm bảo rằng ứng dụng được tổ chức gọn gàng và dễ dàng bảo trì, với mỗi thành phần đều có trách nhiệm rõ ràng, từng bước tách biệt logic ứng dụng để thúc đẩy việc viết mã sạch hơn và dễ quản lý hơn.

### Ví dụ

1.  **Lớp Ứng Dụng Chính (Application)**:

    ```java
    package io.akitect;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    /**
     * Điểm khởi đầu chính của ứng dụng Spring Boot.
     */
    @SpringBootApplication
    public class Application {
        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }
    }
    ```

2.  **Lớp Hằng Số Ứng Dụng (ApplicationConstants)**:

    ```java
    package io.akitect;

    /**
     * Định nghĩa các hằng số được sử dụng xuyên suốt ứng dụng.
     */
    public class ApplicationConstants {
        public static final String API_VERSION = "/api/v1";
        public static final int MAX_PAGE_SIZE = 50;
    }
    ```

3.  **Lớp Cấu Hình (ApplicationConfiguration)**:

    ```java
    package io.akitect.configuration;

    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

    /**
     * Cấu hình ứng dụng.
     */
    @Configuration
    public class ApplicationConfiguration {
        @Bean
        public BCryptPasswordEncoder passwordEncoder() {
            return new BCryptPasswordEncoder();
        }
    }
    ```

4.  **Lớp Controller (ApplicationController)**:

    ```java
    package io.akitect.controller;

    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;

    /**
     * Xử lý các yêu cầu HTTP đến.
     */
    @RestController
    @RequestMapping(ApplicationConstants.API_VERSION + "/users")
    public class ApplicationController {

        @GetMapping("/hello")
        public String sayHello() {
            return "Xin chào từ API!";
        }
    }
    ```

5.  **Giao diện Dịch Vụ (Service)**:

    ```java
    package io.akitect.service;

    /**
     * Giao diện dịch vụ cho logic nghiệp vụ.
     */
    public interface ApplicationService {
        String performLogic();
    }
    ```

6.  **Lớp Thực Hiện Dịch Vụ (Service)**:

    ```java
    package io.akitect.service.impl;

    import io.akitect.service.ApplicationService;
    import org.springframework.stereotype.Service;

    /**
     * Thực hiện của ApplicationService.
     */
    @Service
    public class ApplicationServiceImpl implements ApplicationService {

        @Override
        public String performLogic() {
            return "Logic nghiệp vụ được thực hiện";
        }
    }
    ```

7.  **Giao diện Đối Tượng Truy Cập Dữ Liệu (DAO)**:

    ```java
    package io.akitect.dao;

    /**
     * Giao diện DAO cho các hoạt động truy cập dữ liệu.
     */
    public interface ApplicationDao {
        void saveData();
    }
    ```

8.  **Lớp Thực Hiện DAO**:

    ```java
    package io.akitect.dao.impl;

    import io.akitect.dao.ApplicationDao;
    import org.springframework.stereotype.Repository;

    /**
     * Thực hiện của ApplicationDao.
     */
    @Repository
    public class ApplicationDaoImpl implements ApplicationDao {

        @Override
        public void saveData() {
            // Logic để lưu dữ liệu
            System.out.println("Dữ liệu được lưu!");
        }
    }
    ```

9.  **Lớp Đối Tượng Truyền Dữ Liệu (DTO)**:

    ```java
    package io.akitect.dto;

    /**
     * Đối tượng truyền dữ liệu để đóng gói dữ liệu người dùng.
     */
    public class ApplicationDto {
        private String username;
        private String email;

        // Getters và Setters
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

10. **Lớp Tiện Ích (Utility)**:

    ```java
    package io.akitect.util;

    import java.time.LocalDate;
    import java.time.format.DateTimeFormatter;

    /**
     * Lớp tiện ích cung cấp các chức năng chung.
     */
    public class ApplicationUtils {
        public static String formatDate(LocalDate date) {
            return date.format(DateTimeFormatter.ISO_DATE);
        }
    }
    ```

11. **Giao diện Kiểm Tra (Validation)**:

    ```java
    package io.akitect.validation;

    /**
     * Giao diện cho logic kiểm tra.
     */
    public interface ApplicationValidation {
        boolean validateData(Object data);
    }
    ```

12. **Lớp Thực Hiện Kiểm Tra (Validation)**:

    ```java
    package io.akitect.validation.impl;

    import io.akitect.validation.ApplicationValidation;

    /**
     * Thực hiện của giao diện ApplicationValidation.
     */
    public class ApplicationValidationImpl implements ApplicationValidation {

        @Override
        public boolean validateData(Object data) {
            // Thực hiện kiểm tra dữ liệu
            return data != null; // Ví dụ đơn giản
        }
    }
    ```

    Những ví dụ này được thiết kế để minh họa cấu trúc mã trong một dự án Spring Boot thực tế. Mỗi lớp và giao diện đóng một vai trò cụ thể trong kiến trúc ứng dụng, đảm bảo tách biệt các mối quan tâm và mã sạch, dễ bảo trì.

##### 2. Quản lý phụ thuộc (Dependencies)  với Maven và Gradle

-   **Giới thiệu**: Spring Boot sử dụng Maven hoặc Gradle để quản lý các phụ thuộc và tự động cấu hình các bean. Điều này giúp đơn giản hóa việc cấu hình và quản lý các thư viện cần thiết cho ứng dụng.

-   **Maven**:

-   **pom.xml**: File cấu hình của Maven, chứa thông tin về các phụ thuộc, các plugin và các thông số build.

-   **Thêm phụ thuộc**: Để thêm một phụ thuộc vào dự án, bạn chỉ cần thêm một thẻ `<dependency>` vào file `pom.xml`.

-   **Ví dụ**:
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

-   **build.gradle**: File cấu hình của Gradle, chứa thông tin về các phụ thuộc, các plugin và các thông số build.

-   **Thêm phụ thuộc**: Để thêm một phụ thuộc vào dự án, bạn chỉ cần thêm một dòng `implementation` vào phần `dependencies` trong file `build.gradle`.

-   **Ví dụ**:
    ```groovy
    dependencies {
        // Dependency for Spring Data JPA
        implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
        // Dependency for Spring Web
        implementation 'org.springframework.boot:spring-boot-starter-web'
    }
    ```

##### 3. Cấu hình phụ thuộc không tự động

-   **Giới thiệu**: Không phải tất cả các phụ thuộc đều được Spring Boot tự động cấu hình. Đôi khi, bạn sẽ cần phải tự cấu hình các bean và các thiết lập khác cho các phụ thuộc đó.

-   **Ví dụ**: Thêm và cấu hình phụ thuộc `spring-boot-starter-data-jpa` và `spring-boot-starter-web`.

-   **Thêm phụ thuộc**: Thêm các phụ thuộc vào file `pom.xml` hoặc `build.gradle` như đã trình bày ở trên.

-   **Cấu hình ứng dụng**:

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

Với nội dung và ví dụ cụ thể trên, học viên sẽ nắm vững cấu trúc của một dự án Spring Boot, cách quản lý phụ thuộc và tự động cấu hình các bean, cũng như cách thêm và cấu hình các phụ thuộc không tự động.
