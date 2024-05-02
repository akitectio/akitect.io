---
date: 2019-02-27T09:32:30-07:00
draft: true
title: 01. Giới thiệu về Spring Boot
description: Spring là màu xuân, Boot là khởi động, nên chúng ta có thể gọi là Khởi động mùa xuân, nhưng mà theo mình nên gọi là Spring Boot
video: https://firebasestorage.googleapis.com/v0/b/fireship-app.appspot.com/o/courses%2Fcloud-functions-master-course%2F3-authfun.mp4?alt=media&token=80a508fa-965b-4691-830d-3b240b2e0385
weight: 1
---
Spring là màu xuân, Boot là khởi động, nên chúng ta có thể gọi là Khởi động mùa xuân, nhưng mà theo mình nên gọi là Spring Boot

Spring Boot là một dự án con trong hệ sinh thái lớn của Spring Framework, được thiết kế để giúp các nhà phát triển nhanh chóng tạo ra các ứng dụng Spring dựa trên Java mà không cần phải lo lắng về quá trình cấu hình ứng dụng một cách thủ công. Spring Boot được phát triển và duy trì bởi Pivotal Team (nay là một phần của VMware) và cộng đồng mở rộng, được phát hành lần đầu vào năm 2014.


{{< figure src="/courses/spring-boot-basic/spring-boot-overview/spring-vmware-tanzu.png" >}}

```mermaid
gantt
    title Roadmap Phát Triển Chi Tiết của Spring Boot
    dateFormat  YYYY-MM-DD 
    axisFormat  %Y

    section Khởi đầu
    Khởi tạo Spring Boot :done, 2014-04-01, 30d

    section Spring Boot 1.x
    Hỗ trợ Spring Framework 4.1 :done, 2014-12-01, 180d
    Devtools & Caching :done, 2015-11-01, 180d
    Cải thiện Testing :done, 2016-07-01, 180d
    Bảo mật & Kafka :done, 2017-02-01, 270d

    section Spring Boot 2.x
    Spring Framework 5.0 & Reactive Programming :done, 2018-03-01, 180d
    Java 11 Support :done, 2018-10-01, 180d
    Docker & Kubernetes :done, 2019-10-01, 180d
    Layer cho Docker Images :done, 2020-05-01, 180d
    Cấu hình ứng dụng & Spring Cloud :done, 2020-11-01, 180d
    GraalVM Support :done, 2021-05-01, 180d
    Cải tiến liên tục :done, 2021-11-01, 2022-11-01

    section Spring Boot 3.x
    Spring Framework 6.0 & Jakarta EE 9 :active, 2022-11-01, 2023-05-01
    Tối ưu hóa cho Java 17+ :active, 2023-05-02, 60d
    Cải thiện hỗ trợ Microservices :active, 2023-07-01, 60d
    Mở rộng Reactive Programming :active, 2023-09-01, 60d
    Tăng cường hỗ trợ Containers :active, 2023-11-01, 60d
    Tính năng mới & Cải tiến :active, 2024-01-01, 60d

```

- `04/2014` : Release phiên bản Spring boot 1.0 hổ trợ Hỗ trợ Spring Framework 4.1.
-  `03/2018` : Tiếp tục Release phiên bản Spring boot 1.0m hổ trợ java 11, Docker,Kubernetes, Layer cho docker, tích hợp với Spring cloud, hổ trợ GraalVM.
- `11/2022 `: Bản Spring Boot 3 ra đời, hổ trợ phiên bản Spring Framework 6 và Jakarta EE 9, Tối ưu hoá hơn cho java 17 trở lên, Cả thiện hổ trợ cho Microservices, Tăng cường hoá cho Containers, tung ra thêm nhiều tính năng mới hơn.


<!-- ```mermaid
sequenceDiagram
    participant N as Nhà Phát Triển
    participant M as Phương thức main()
    participant SA as SpringApplication
    participant AC as ApplicationContext
    participant CA as Cấu Hình Ứng Dụng
    participant ACfg as Tự Động Cấu Hình
    participant B as Các Bean
    participant S as Server Nhúng (Tomcat, Jetty,...)

    N->>M: Chạy ứng dụng
    M->>SA: SpringApplication.run()
    SA->>AC: Khởi tạo ApplicationContext
    AC->>CA: Tải cấu hình từ application.properties/yml
    CA->>ACfg: Kiểm tra điều kiện tự động cấu hình
    ACfg->>B: Khởi tạo và cấu hình Các Bean
    B->>AC: Đăng ký Các Bean với ApplicationContext
    AC->>S: Kiểm tra và Khởi động Server Nhúng nếu cần
    S->>AC: Server Nhúng sẵn sàng, thông báo ApplicationContext
    AC->>N: Ứng dụng sẵn sàng để sử dụng
``` -->

```mermaid
sequenceDiagram
    participant D as Developer
    participant M as main() Method
    participant SA as SpringApplication
    participant AC as ApplicationContext
    participant AutoC as Auto-Configuration
    participant Config as Load Application Config
    participant Beans as Initialize Beans
    participant WS as Start Web Server

    D->>M: Chạy ứng dụng
    M->>SA: SpringApplication.run()
    SA->>AC: Tạo ApplicationContext
    AC->>AutoC: Thực hiện Auto-Configuration
    AutoC->>Config: Tải cấu hình (application.properties/yml)
    Config->>Beans: Khởi tạo và cấu hình Beans
    Beans->>WS: Khởi động Web Server (nếu ứng dụng web)
    WS->>D: Ứng dụng sẵn sàng phục vụ
```