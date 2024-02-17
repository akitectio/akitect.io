---
draft: false
featuredImage: /courses/docker-basic/run-docker-container/featured-image.webp
images:
  - /courses/docker-basic/run-docker-container/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
  - apple-m1-silicon
title: Lesson 4 - Chạy Container trong Docker
description: Hướng dẫn chi tiết về cách chạy một container Docker. Bạn sẽ học cách chọn hình ảnh Docker, sử dụng lệnh `docker run`, và các tùy chọn thường dùng như `-it`, `--rm`, `-p`, và `-v`.
date: 2024-02-15T10:23:30-09:00
weight: 4
---

Chạy container Docker là quy trình cốt lõi trong việc sử dụng Docker, cho phép bạn triển khai và quản lý các ứng dụng của mình một cách linh hoạt. Dưới đây là hướng dẫn chi tiết về cách chạy một container Docker.

### Chạy Container Docker

#### 1. **Chọn Hình Ảnh Docker:**

Trước tiên, bạn cần chọn hình ảnh Docker mà bạn muốn sử dụng để chạy container. Bạn có thể chọn một hình ảnh từ [Docker Hub](https://hub.docker.com/) hoặc sử dụng một hình ảnh mà bạn đã xây dựng từ trước.

#### 2. **Sử dụng Lệnh `docker run`:**

Lệnh `docker run` là lệnh cơ bản để tạo và khởi động một container mới từ một hình ảnh Docker.

Cú pháp cơ bản của lệnh này là:

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

Trong đó:
- `[OPTIONS]` là các tùy chọn khác nhau có thể áp dụng, như ánh xạ cổng hoặc gắn volume.
- `IMAGE` là tên của hình ảnh Docker bạn muốn chạy.
- `[COMMAND]` là lệnh mà bạn muốn thực thi bên trong container khi nó khởi động.
- `[ARG...]` là danh sách các tham số cho lệnh.

#### 3. **Tùy Chọn Thường Dùng:**

- **Tương tác (`-it`):** Sử dụng `-it` để chạy container trong chế độ tương tác, cho phép bạn tương tác với terminal của container. `i` đại diện cho `--interactive` giữ STDIN mở cả khi không gắn vào, và `t` đại diện cho `--tty` phân bổ một pseudo-TTY.

  ```bash
  docker run -it ubuntu bash
  ```

- **Xóa tự động (`--rm`):** Sử dụng `--rm` để Docker tự động xóa container khi nó dừng, giúp giữ hệ thống sạch sẽ.

  ```bash
  docker run --rm nginx
  ```

- **Ánh xạ cổng (`-p`):** Sử dụng `-p` để ánh xạ cổng từ container ra máy chủ. Điều này hữu ích cho các ứng dụng web hoặc dịch vụ cần truy cập từ bên ngoài.

  ```bash
  docker run -p 80:80 nginx
  ```

  Trong ví dụ này, cổng 80 trên máy chủ được ánh xạ đến cổng 80 trong container.

- **Gắn kết volume (`-v`):** Sử dụng `-v` để gắn một thư mục từ máy chủ của bạn vào container, cho phép chia sẻ dữ liệu.

  ```bash
  docker run -v /my/host/folder:/my/container/folder nginx
  ```

  Trong đó `/my/host/folder` là đường dẫn trên máy chủ và `/my/container/folder` là đường dẫn trong container.

### Kết luận

Chạy container Docker đơn giản nhưng mạnh mẽ, cho phép bạn triển khai các ứng dụng và dịch vụ nhanh chóng. Bằng cách sử dụng các tùy chọn như `-it`, `--rm`, `-p`, và `-v`, bạn có thể điều chỉnh chính xác cách container của mình được chạy và tương tác với hệ thống của bạn.