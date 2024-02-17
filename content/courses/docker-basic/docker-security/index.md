---
draft: false
featuredImage: /courses/docker-basic/docker-security/featured-image.webp
images:
  - /courses/docker-basic/docker-security/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
title: Lesson 6 - Docker Security
description: Docker Security là một chủ đề quan trọng mà mọi người sử dụng Docker cần phải quan tâm. Trong bài học này, chúng ta sẽ tìm hiểu về các vấn đề bảo mật liên quan đến Docker và cách giải quyết chúng.
date: 2024-02-15T10:23:30-09:00
weight: 5
---


Trong thế giới phát triển và triển khai phần mềm đang phát triển nhanh chóng, Docker đã trở thành một giải pháp quan trọng cho Container hóa, mang lại một lựa chọn nhẹ hơn so với các máy ảo truyền thống. Tuy nhiên, với sự phổ biến ngày càng tăng của Docker, vấn đề bảo mật trở nên cực kỳ quan trọng. Trong bài viết này, chúng ta sẽ khám phá bảo mật Docker, khám phá các phương pháp hay nhất để đảm bảo rằng Container của bạn được bảo vệ khỏi các mối đe dọa tiềm ẩn.

## Hiểu Biết về Bảo Mật Docker

Bảo mật Docker bao gồm việc bảo vệ Docker Daemon, Docker Image, Container và hệ thống máy chủ cơ bản. Mặc dù Docker cung cấp một môi trường cô lập cho ứng dụng của bạn, nhưng điều quan trọng cần nhớ là các Container chia sẻ cùng một kernel với máy chủ. Điều này nhấn mạnh tầm quan trọng của việc bảo vệ mọi lớp trong hệ sinh thái Docker.

### Các Phương Pháp Hay Nhất cho Bảo Mật Docker

#### 1. Bảo Vệ Máy Chủ Docker

Nền tảng của bảo mật Docker là bảo vệ máy chủ. Đảm bảo rằng hệ điều hành máy chủ của bạn được cập nhật thường xuyên để vá các lỗ hổng bảo mật đã biết. Sử dụng một hệ điều hành tối thiểu được thiết kế riêng cho việc chạy Container, như CoreOS, RancherOS hoặc Atomic Host, để giảm bề mặt tấn công.

#### 2. Sử Dụng Docker Image Đáng Tin Cậy

Luôn sử dụng Image chính thức hoặc được đánh giá cao từ Docker Hub. Trước khi kéo một Image, xác minh tính xác thực của nó và xem xét Dockerfile của nó để hiểu rõ những gì có trong Container. Tránh sử dụng Image có nguồn gốc không rõ ràng hoặc không được bảo trì định kỳ.

#### 3. Giữ Image Gọn Gàng

Nguyên tắc của sự tối giản áp dụng tốt cho Docker Image. Sử dụng Image cơ sở nhỏ như Alpine Linux, nhẹ và giảm khả năng cho các lỗ hổng bảo mật. Ngoài ra, tránh cài đặt các gói hoặc dịch vụ không cần thiết trong Container của bạn.

#### 4. Quét Image Để Tìm Lỗ Hổng

Quét định kỳ Docker Image của bạn để tìm lỗ hổng bảo mật đã biết sử dụng các công cụ như Clair, Trivy hoặc dịch vụ quét của chính Docker. Những công cụ này có thể xác định các vấn đề bảo mật đã biết trong các lớp Image và cung cấp thông tin về cách giải quyết chúng.

#### 5. Thực Hiện Docker Content Trust

Docker Content Trust (DCT) cho phép bạn xác minh tính toàn vẹn và nguồn gốc của Docker Image. Bằng cách kích hoạt DCT, bạn có thể đảm bảo rằng Docker client của mình chỉ kéo các Image đã được ký, thêm một lớp bảo mật bằng cách ngăn chặn việc sử dụng các Image đã bị chỉnh sửa.

#### 6. Sử Dụng Docker Secrets và Biến Môi Trường Một Cách Thông Minh

Thông tin nhạy cảm như mật khẩu và khóa API không bao giờ nên được mã hóa cứng vào Docker Image. Thay vào đó, sử dụng Docker secrets hoặc biến môi trường để quản lý dữ liệu nhạy cảm một cách an toàn. Docker secrets cung cấp một giải pháp an toàn hơn bằng cách lưu trữ dữ liệu nhạy cảm trong cửa hàng Raft nội bộ của đàn swarm.

#### 7. An Ninh Mạng và Quy Tắc Tường Lửa

Theo mặc định, Docker điều chỉnh các quy tắc iptables để cung cấp sự cô lập mạng. Tuy nhiên, điều quan trọng là phải xem xét lại các quy tắc này và đảm bảo chúng phù hợp với các chính sách bảo mật của bạn. Sử dụng các trình điều khiển mạng được tích hợp sẵn của Docker để cô lập các mạng Container và thực hiện các quy tắc tường lửa hạn chế giao tiếp nội bộ và bên ngoài không cần thiết.

#### 8. Giới Hạn Quyền Hạn của Container

Chạy Container với ít quyền nhất cần thiết cho hoạt động của chúng. Tránh chạy Container dưới tài khoản root khi có thể. Sử dụng không gian tên người dùng để ánh xạ người dùng Container sang người dùng ít quyền hơn trên hệ thống máy chủ.

#### 9. Kích Hoạt Ghi Nhật Ký và Giám Sát

Giám sát và ghi nhật ký là chìa khóa để phát hiện và phản ứng với các sự cố bảo mật. Kích hoạt ghi nhật ký cho Docker daemon và sử dụng các công cụ giám sát để theo dõi hoạt động của Container. Tích hợp ghi nhật ký của bạn với một giải pháp ghi nhật ký tập trung để phân tích và cảnh báo tốt hơn.

#### 10. Cập Nhật Định Kỳ và Quản Lý Vá

Giữ cho Docker engine, Image và Container của bạn được cập nhật. Áp dụng định kỳ các bản vá bảo mật cho Docker engine, hệ điều hành máy chủ và các ứng dụng trong Container của bạn. Áp dụng một quy trình quản lý vá bao gồm kiểm tra các bản vá trong môi trường staging trước khi triển khai chúng vào sản xuất.

### Ví dụ: Triển khai Ứng Dụng Web an toàn với Docker Compose

Giả sử bạn có một ứng dụng web đơn giản được Container hóa sử dụng Docker và bạn muốn đảm bảo rằng việc triển khai của mình tuân thủ các phương pháp bảo mật tốt nhất. Ứng dụng của bạn có hai thành phần chính:

1. **Frontend**: Một ứng dụng web tĩnh được phục vụ bởi Nginx.
2. **Backend**: Một API server được viết bằng Node.js, kết nối với một cơ sở dữ liệu MongoDB.

#### Bước 1: Chuẩn bị Dockerfile

**Dockerfile cho Backend (Node.js)**:

```Dockerfile
# Sử dụng Image cơ sở Node.js Alpine để giảm kích thước
FROM node:14-alpine

# Thiết lập thư mục làm việc
WORKDIR /app

# Sao chép mã nguồn và cài đặt phụ thuộc
COPY package*.json ./
RUN npm install

COPY . .

# Chạy ứng dụng
CMD ["node", "server.js"]
```

**Dockerfile cho Frontend (Nginx)**:

```Dockerfile
# Sử dụng Image Nginx Alpine
FROM nginx:alpine

# Sao chép nội dung tĩnh vào thư mục phục vụ của Nginx
COPY /path/to/your/web/app /usr/share/nginx/html

# Expose cổng 80
EXPOSE 80
```

#### Bước 2: Tạo docker-compose.yml

Tạo một file `docker-compose.yml` để định nghĩa cách các dịch vụ tương tác với nhau:

```yaml
version: '3.8'
services:
  frontend:
    build: ./path/to/frontend
    ports:
      - "8080:80"
    depends_on:
      - backend

  backend:
    build: ./path/to/backend
    environment:
      - DB_HOST=mongodb
    depends_on:
      - mongodb

  mongodb:
    image: mongo:4.2-bionic
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=secret
```

Trong ví dụ này, chúng ta đã định nghĩa 3 dịch vụ: `frontend`, `backend`, và `mongodb`. Mỗi dịch vụ được cấu hình với các biến môi trường cần thiết, và `frontend` cũng được cấu hình để phụ thuộc vào `backend`, đảm bảo rằng `backend` sẽ khởi động trước `frontend`.

#### Bước 3: Áp dụng Các Biện Pháp Bảo Mật

1. **Sử dụng biến môi trường cho dữ liệu nhạy cảm**: Trong `docker-compose.yml`, chúng tôi đã sử dụng biến môi trường để truyền thông tin nhạy cảm như tên người dùng và mật khẩu MongoDB.

2. **Không chạy ứng dụng với quyền root**: Trong Dockerfile, chúng ta có thể tạo một người dùng không phải root và chạy ứng dụng dưới người dùng này để tăng cường bảo mật.

```Dockerfile
# Tạo một người dùng không phải root và

 chuyển đổi sang người dùng đó
RUN adduser -D myuser
USER myuser
```

3. **Quét Image để tìm lỗ hổng**: Sử dụng công cụ như Clair hoặc Trivy để quét các Image của bạn trước khi triển khai.

4. **Cập nhật thường xuyên**: Đảm bảo rằng bạn thường xuyên cập nhật Docker Image của mình để bao gồm các bản vá bảo mật mới nhất.

Bằng cách áp dụng các biện pháp bảo mật này, bạn có thể giảm thiểu rủi ro và đảm bảo rằng ứng dụng Container hóa của mình được bảo vệ một cách hiệu quả.



### Kết Luận

Bảo vệ Container Docker là một quá trình liên tục đòi hỏi sự cảnh giác, thực hành tốt nhất và cập nhật định kỳ. Bằng cách thực hiện các chiến lược nêu trên, bạn có thể cải thiện đáng kể bảo mật của môi trường Docker và bảo vệ ứng dụng Container hóa của mình


Để làm sâu sắc hơn vấn đề bảo mật Docker và cách áp dụng các biện pháp bảo mật trong thực tế, chúng ta sẽ xem xét một ví dụ cụ thể về cách cấu hình và triển khai một ứng dụng web sử dụng Docker Compose, với sự chú trọng vào các khía cạnh bảo mật.
