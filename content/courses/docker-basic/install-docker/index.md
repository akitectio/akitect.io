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
  - apple-m1-silicon
  - mac-m1
title: Lesson 2 - Cài đặt Docker trên Mac M1
description:  Để cài đặt và sử dụng Docker trên Mac M1 một cách hiệu quả, bạn cần thực hiện các bước sau đây một cách cẩn thận và chi tiết. Mac M1 sử dụng kiến trúc ARM, vì vậy có một số điều cần lưu ý để đảm bảo Docker hoạt động ổn định.
date: 2024-02-15T10:23:30-09:00
weight: 2
---

Để cài đặt và sử dụng Docker trên Mac M1 một cách hiệu quả, bạn cần thực hiện các bước sau đây một cách cẩn thận và chi tiết. Mac M1 sử dụng kiến trúc ARM, vì vậy có một số điều cần lưu ý để đảm bảo Docker hoạt động ổn định.


### Cài đặt Docker trên Mac M1:

#### Bước 1: Tải xuống Docker Desktop cho Mac
- Truy cập trang web chính thức của Docker tại [Docker Desktop for Mac](https://docs.docker.com/desktop/install/mac-install/) và tải xuống phiên bản dành cho chip Apple Silicon.

{{< figure src="/images/download-docker.png" >}}

- Đảm bảo rằng bạn đã chọn đúng phiên bản cho Mac M1, vì có phiên bản riêng biệt cho chip Intel và Apple Silicon.

#### Bước 2: Cài đặt Docker Desktop
- Mở file `.dmg` đã tải xuống và kéo biểu tượng Docker vào thư mục Applications.
- Mở Docker từ thư mục Applications. Lần đầu tiên bạn mở Docker, bạn có thể cần phải qua một số bước bảo mật của macOS để cho phép ứng dụng chạy.

#### Bước 3: Khởi động và cấu hình Docker Desktop
- Sau khi cài đặt, mở Docker Desktop. Ứng dụng sẽ tự động khởi động và chạy ở background.
- Bạn có thể cần điều chỉnh cấu hình tài nguyên như CPU, bộ nhớ và không gian lưu trữ trong Preferences để tối ưu hóa hiệu suất.

#### Bước 4: Kiểm tra việc cài đặt
- Mở Terminal và gõ lệnh `docker --version` để kiểm tra phiên bản Docker, đảm bảo rằng Docker đã được cài đặt đúng cách.

### Sử dụng Docker:

#### Bước 1: Kéo một hình ảnh Docker
- Mở Terminal và sử dụng lệnh `docker pull hello-world` để kéo một hình ảnh Docker đơn giản nhằm kiểm tra.
- Lệnh này sẽ tải hình ảnh `hello-world` từ Docker Hub về máy của bạn.

#### Bước 2: Chạy một container
- Sử dụng lệnh `docker run hello-world` để tạo và chạy một container từ hình ảnh `hello-world`.
- Điều này sẽ khởi chạy container và bạn sẽ thấy một thông điệp chào mừng từ Docker, chứng tỏ rằng container đang hoạt động.

#### Bước 3: Quản lý container và hình ảnh
- Sử dụng `docker logs <container-id>` để xem nhật ký của một container cụ thể.
- Dừng container đang chạy bằng lệnh `docker stop <container-name>` và xóa chúng bằng `docker rm <container-name>`.

### Lưu ý quan trọng:
- Đối với Mac M1, bạn có thể cần sử dụng hình ảnh Docker được tối ưu hóa cho kiến trúc ARM. Kiểm tra thông tin về hình ảnh trên Docker Hub để đảm bảo tương thích.
- Nếu gặp lỗi liên quan đến kiến trúc ARM, bạn có thể cần tìm hình ảnh thay thế hoặc sử dụng giải pháp như QEMU để mô phỏng kiến trúc x86.

### Kết luận:
Việc cài đặt và sử dụng Docker trên Mac M1 có thể yêu cầu một số bước điều chỉnh nhỏ do khác biệt kiến trúc phần cứng, nhưng tổng thể quy trình khá đơn giản và trực tiếp. Docker là một công cụ mạnh mẽ cho phép bạn đóng gói, phân phối và chạy ứng dụng của mình một cách nhất quán trên mọi môi trường, giúp đơn giản hóa quy trình phát triển và triển khai ứng dụng.