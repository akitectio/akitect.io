---
draft: false
featuredImage: /courses/docker-basic/docker-compose/featured-image.webp
images:
  - /courses/docker-basic/docker-compose/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
title: Lesson 5 - Docker Compose
description: Docker Compose là một công cụ mạnh mẽ giúp bạn định nghĩa và chạy nhiều container Docker một cách dễ dàng và hiệu quả. Bạn sẽ học cách sử dụng Docker Compose để quản lý ứng dụng của mình với tệp cấu hình YAML đơn giản.
date: 2024-02-15T10:23:30-09:00
weight: 5
---

Docker Compose là một công cụ quản lý dự án Docker cho phép bạn chạy và quản lý nhiều container Docker như một ứng dụng duy nhất. Với Docker Compose, bạn có thể dễ dàng cấu hình, khởi tạo và quản lý tất cả các dịch vụ liên quan đến ứng dụng của mình thông qua một file cấu hình YAML đơn giản.

### Bước 1: Cài đặt Docker Compose

- Trên **Docker Desktop for Windows và Mac**: Docker Compose đã được tích hợp sẵn, không cần cài đặt thêm.
- Trên **Linux**: Bạn cần phải cài đặt Docker Compose bằng cách thực hiện các lệnh được cung cấp trên trang web chính thức của Docker.

### Bước 2: Tạo File docker-compose.yml

File `docker-compose.yml` là trái tim của mọi dự án Docker Compose. File này chứa định nghĩa về cách các container trong dự án của bạn được xây dựng, cách chúng liên kết với nhau, và bất kỳ cấu hình nào khác cần thiết.

**Ví dụ:**

```yaml
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  database:
    image: postgres:latest
    environment:
      POSTGRES_DB: exampledb
      POSTGRES_USER: exampleuser
      POSTGRES_PASSWORD: examplepass
```

Trong ví dụ trên, chúng ta có 2 dịch vụ: `web` sử dụng hình ảnh `nginx:latest` và `database` sử dụng hình ảnh `postgres:latest`. Dịch vụ web được cấu hình để ánh xạ cổng 8080 trên máy chủ đến cổng 80 trong container, trong khi dịch vụ database được cung cấp một số biến môi trường để cấu hình cơ sở dữ liệu.

### Bước 3: Chạy Docker Compose

Để khởi chạy dự án, bạn chỉ cần mở terminal, di chuyển đến thư mục chứa file `docker-compose.yml` và chạy lệnh:

```bash
docker-compose up
```

Lệnh này sẽ đọc file `docker-compose.yml`, xây dựng (nếu cần) và khởi chạy tất cả các dịch vụ được định nghĩa. Sử dụng `-d` để chạy các dịch vụ ở chế độ ngầm.

### Bước 4: Quản lý Dịch vụ

- **Xem danh sách các dịch vụ đang chạy:**

```bash
docker-compose ps
```

- **Xem nhật ký của dịch vụ:**

```bash
docker-compose logs [service_name]
```

- **Dừng tất cả các dịch vụ:**

```bash
docker-compose down
```

### Tính năng Nâng cao

- **Xây dựng Hình ảnh Tùy chỉnh:** Nếu bạn muốn Docker Compose xây dựng một hình ảnh tùy chỉnh cho dịch vụ của mình thay vì sử dụng hình ảnh từ Docker Hub, bạn có thể sử dụng phần `build` trong file cấu hình.

```yaml
services:
  myservice:
    build: ./my_service_directory
```

- **Gắn Volume:** Để lưu trữ hoặc chia sẻ dữ liệu giữa container và máy chủ của bạn, bạn có thể sử dụng volumes.

```yaml
services:
  web:
    image: nginx
    volumes:
      - ./my_local_directory:/usr/share/nginx/html
```

- **Mạng Tùy chỉnh:** Docker Compose

 cho phép bạn định nghĩa và sử dụng các mạng tùy chỉnh để tạo ra các mối quan hệ mạng phức tạp giữa các container.

```yaml
services:
  myservice:
    networks:
      - mycustomnetwork

networks:
  mycustomnetwork:
```

### Kết luận

Docker Compose là công cụ không thể thiếu trong quy trình phát triển ứng dụng container hóa, giúp việc quản lý và triển khai các dịch vụ liên quan trở nên đơn giản và hiệu quả. Đảm bảo rằng bạn luôn tham khảo tài liệu chính thức của Docker Compose để hiểu rõ hơn về các tính năng và cách sử dụng chúng một cách tối ưu nhất.