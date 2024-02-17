---
draft: false
featuredImage: /courses/docker-basic/install-docker/featured-image.webp
images:
  - /courses/docker-basic/install-docker/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
title: Lesson 7 - Docker Networking
description: Docker Networking là một chủ đề quan trọng mà mọi người sử dụng Docker cần phải quan tâm. Trong bài học này, chúng ta sẽ tìm hiểu về các vấn đề liên quan đến Network trong Docker và cách giải quyết chúng.
date: 2024-02-15T10:23:30-09:00
weight: 7
---

Docker Network là một phần không thể thiếu khi triển khai các ứng dụng container hóa, giúp tạo ra môi trường liên kết và giao tiếp giữa các container. Để hiểu sâu hơn về Docker Networking, hãy cùng đi qua từng bước cấu hình Network và một số tình huống thực tế bạn có thể gặp phải.

### Kiểu Network trong Docker

Docker cung cấp vài kiểu Network cơ bản như sau:

- **bridge**: Network cầu nối mặc định cho các container. Các container trong cùng một Network bridge có thể giao tiếp với nhau thông qua địa chỉ IP.
- **host**: Loại bỏ lớp cách ly Network giữa container và Docker host, cho phép container sử dụng stack Network của host.
- **overlay**: Tạo Network trên nhiều Docker daemon, thích hợp cho các ứng dụng phân tán trên nhiều máy chủ.
- **macvlan**: Cho phép gán một địa chỉ MAC cho container, làm cho nó xuất hiện như một thiết bị vật lý trong Network.

### Bước 1: Cấu Hình Network Bridge Tùy Chỉnh

1. **Tạo Network Bridge:**

   ```bash
   docker network create --driver bridge my_custom_network
   ```

   Lệnh này tạo một Network bridge mới có tên là `my_custom_network`.

2. **Chạy Container và Kết Nối vào Network Tùy Chỉnh:**

   ```bash
   docker run -d --name my_container --network my_custom_network nginx
   ```

   Container `my_container` sẽ chạy và tự động kết nối vào `my_custom_network`.

### Bước 2: Giao Tiếp giữa các Container

- **Liên kết Container:**

  ```bash
  docker run -d --name db_container --network my_custom_network mysql
  docker run -d --name app_container --network my_custom_network --link db_container:db my_app
  ```

  `app_container` sẽ liên kết với `db_container` thông qua biệt danh `db`, cho phép `app_container` giao tiếp với `db_container` sử dụng biệt danh này.

### Bước 3: Expose và Ánh Xạ Cổng

- **Expose Container ra Ngoài:**

  ```bash
  docker run -d --name web_container --network my_custom_network -p 8080:80 nginx
  ```

  Lệnh này sẽ ánh xạ cổng 80 của `web_container` vào cổng 8080 trên Docker host, cho phép truy cập `web_container` từ bên ngoài thông qua cổng 8080 của host.

### Bước 4: Sử Dụng Docker Compose

Docker Compose giúp quản lý các container và Network phức tạp dễ dàng hơn. Dưới đây là ví dụ về file `docker-compose.yml` để cấu hình Network và các dịch vụ:

```yaml
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
    networks:
      - my_app_network

  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
    networks:
      - my_app_network

networks:
  my_app_network:
    driver: bridge
```

File này định nghĩa một Network bridge `my_app_network` và hai dịch vụ `web` và `db` được kết nối với Network này.

### Kết luận

Docker Network cung cấp sự linh hoạt và mạnh mẽ trong cách giao tiếp và quản lý các container. Việc hiểu rõ các kiểu Network và biết cách sử dụng chúng trong các t

ình huống cụ thể là chìa khóa để xây dựng và triển khai các ứng dụng hiệu quả, bảo mật trong môi trường container hóa. Đừng quên kết hợp với các công cụ như Docker Compose để quản lý dự án Docker của bạn một cách dễ dàng và hiệu quả hơn.