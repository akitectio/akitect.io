---
draft: false
featuredImage: /courses/docker-basic/docker-storage/featured-image.webp
images:
  - /courses/docker-basic/docker-storage/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
title: Lesson 8 - Docker Storage
description:  Docker Storage là một phần quan trọng của Docker, giúp quản lý dữ liệu và lưu trữ trong các container. Trong bài viết này, chúng ta sẽ tìm hiểu về các loại lưu trữ trong Docker, cách quản lý và sử dụng lưu trữ trong các container.
date: 2024-02-15T10:23:30-09:00
weight: 8
---


Quản lý dữ liệu trong Docker đòi hỏi sự hiểu biết về các cơ chế lưu trữ mà Docker cung cấp. Mỗi cơ chế có những ưu và nhược điểm riêng, phù hợp với các tình huống sử dụng cụ thể. Dưới đây là một cái nhìn chi tiết hơn về cách sử dụng và quản lý dữ liệu trong Docker thông qua Volumes, Bind Mounts, và tmpfs Mounts.

### Sử dụng Volumes trong Docker

Volumes là lựa chọn ưu tiên để lưu trữ dữ liệu liên tục vì chúng được quản lý bởi Docker và tách biệt khỏi lifecycle của container.

**Tạo và quản lý Volumes:**

1. **Tạo một Volume mới:**

   ```bash
   docker volume create my_volume
   ```

2. **Liệt kê các Volumes hiện có:**

   ```bash
   docker volume ls
   ```

3. **Chạy container và gắn Volume:**

   ```bash
   docker run -v my_volume:/app/data <image_name>
   ```
   
   Trong đó, `my_volume` là tên của volume và `/app/data` là điểm gắn kết trong container.

4. **Xem thông tin chi tiết về một Volume:**

   ```bash
   docker volume inspect my_volume
   ```

### Sử dụng Bind Mounts

Bind Mounts cho phép bạn gắn bất kỳ thư mục nào trên máy host vào container. Dữ liệu được lưu trữ trực tiếp trên filesystem của host và có thể dễ dàng truy cập từ cả host và container.

**Cách sử dụng Bind Mounts:**

```bash
docker run -v /path/on/host:/path/in/container <image_name>
```

Ví dụ, để gắn thư mục `/src` trên máy host vào thư mục `/app` trong container:

```bash
docker run -v /src:/app <image_name>
```

Lưu ý: Bind Mounts có thể tạo ra rủi ro bảo mật nếu không được sử dụng cẩn thận, vì chúng cho phép container truy cập trực tiếp vào hệ thống file của host.

### Sử dụng tmpfs Mounts

Tmpfs Mounts tạo ra một phân vùng bộ nhớ tạm thời trong RAM của host, cung cấp một cách lưu trữ tạm thời mà không ghi dữ liệu ra đĩa.

**Khởi chạy container với tmpfs Mounts:**

```bash
docker run --tmpfs /path/in/container <image_name>
```

Ví dụ, để tạo một tmpfs mount trong `/app/cache` trong container:

```bash
docker run --tmpfs /app/cache <image_name>
```

Dữ liệu trong tmpfs mounts sẽ bị xóa khi container dừng hoặc bị xóa, làm cho chúng trở thành lựa chọn lý tưởng cho dữ liệu cần tính tạm thời hoặc cao về bảo mật.

### Lựa chọn Phương Thức Lưu Trữ Phù Hợp

Khi quyết định cách lưu trữ dữ liệu trong Docker, hãy xem xét những điều sau:

- **Dữ liệu có cần bền vững qua các phiên khởi động container không?** Sử dụng Volumes.
- **Dữ liệu có cần được quản lý bên ngoài Docker và dễ dàng truy cập từ host không?** Cân nhắc sử dụng Bind Mounts.
- **Dữ liệu có chỉ là tạm thời và không cần lưu trữ sau khi container dừng không?** Tmpfs Mounts là sự lựa chọn tốt.

Nhớ rằng, mỗi phương thức lưu trữ có những ưu và nhược điểm riêng, và việc lựa chọn phương thức phù hợp phụ thuộc vào yêu cầu cụ thể của ứng dụng và môi trường triển khai của bạn.