---
draft: false
featuredImage: /courses/docker-basic/build-docker-image/featured-image.webp
images:
  - /courses/docker-basic/build-docker-image/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
  - apple-m1-silicon
title: Lesson 3 - Xây dựng Docker Image
description:  Để cài đặt và sử dụng Docker trên Mac M1 một cách hiệu quả, bạn cần thực hiện các bước sau đây một cách cẩn thận và chi tiết. Mac M1 sử dụng kiến trúc ARM, vì vậy có một số điều cần lưu ý để đảm bảo Docker hoạt động ổn định.
date: 2024-02-15T10:23:30-09:00
weight: 3
---

Để xây dựng Docker Image một cách hiệu quả, việc hiểu rõ từng bước và áp dụng các kỹ thuật tối ưu hóa là rất quan trọng. Dưới đây là hướng dẫn chi tiết hơn với thêm ví dụ về cách tạo Dockerfile, xây dựng Image, tối ưu hóa và chia sẻ Docker Image.

### Tạo Dockerfile:

`Dockerfile` là một tập tin văn bản không có định dạng tên mở rộng, được sử dụng bởi Docker để tự động hóa quá trình xây dựng các hình ảnh Docker. Nó chứa một tập hợp các lệnh mà Docker sử dụng để thiết lập và cấu hình một hình ảnh. Khi bạn chạy lệnh docker build, Docker sẽ đọc các lệnh từ Dockerfile và thực thi chúng từng bước một để tạo ra một hình ảnh Docker mới.

#### 1. Khởi tạo Dockerfile:
- Tạo một thư mục mới trên máy tính của bạn và gọi tên nó theo dự án, ví dụ `my-docker-app`.
- Trong thư mục `my-docker-app`, tạo một file mới và đặt tên nó là `Dockerfile`. Đây sẽ là file mà bạn định nghĩa cách xây dựng Docker Image của mình.

#### 2. Định nghĩa Image cơ sở:
- Mở `Dockerfile` bằng trình soạn thảo văn bản và bắt đầu bằng lệnh `FROM` để chọn Image cơ sở. Ví dụ, nếu bạn muốn xây dựng một ứng dụng web sử dụng Python, bạn có thể bắt đầu với:
  ```docker
  FROM python:3.8-alpine
  ```
  Điều này sẽ sử dụng Python 3.8 trên Image Alpine Linux nhỏ gọn làm cơ sở của Docker Image của bạn.

#### 3. Sao chép tệp và cài đặt phụ thuộc:
- Tiếp theo, sao chép các tệp cần thiết từ máy tính của bạn vào Image và cài đặt bất kỳ phụ thuộc nào. Đối với một ứng dụng Python, bạn có thể có:
  ```docker
  WORKDIR /app
  COPY . /app
  RUN pip install -r requirements.txt
  ```
  Điều này đặt `/app` làm thư mục làm việc, sao chép tất cả tệp từ thư mục hiện tại vào `/app` trong Image, và sau đó chạy `pip install` để cài đặt các gói cần thiết từ `requirements.txt`.

#### 4. Định nghĩa lệnh khởi động:
- Cuối cùng, định nghĩa lệnh mà Docker sẽ thực thi khi container khởi động:
  ```docker
  CMD ["python", "app.py"]
  ```
  Điều này chỉ định rằng Docker nên chạy `app.py` với trình thông dịch Python.

### Xây dựng Docker Image:

#### 1. Xây dựng Image:
- Mở Terminal hoặc Command Prompt, di chuyển đến thư mục chứa `Dockerfile` của bạn và chạy:
  ```bash
  docker build -t my-python-app .
  ```
  Lệnh này sẽ xây dựng Docker Image từ `Dockerfile` của bạn với tag `my-python-app`.

#### 2. Kiểm tra Image mới:
- Kiểm tra Image mới của bạn bằng lệnh:
  ```bash
  docker images
  ```

### Tối ưu hóa Docker Image:

#### 1. Sử dụng Image Alpine:
- Image Alpine Linux nhỏ gọn là một lựa chọn tốt để giảm kích thước Image tổng thể, như đã đề cập ở trên.

#### 

2. Gộp các lệnh `RUN`:
- Giảm số lượng lớp bằng cách gộp các lệnh `RUN`, ví dụ:
  ```docker
  RUN apk add --no-cache python py-pip && \
      pip install -r requirements.txt
  ```

#### 3. Loại bỏ cache và tệp tạm thời:
- Dọn dẹp sau khi cài đặt để loại bỏ cache và các tệp không cần thiết:
  ```docker
  RUN apk add --no-cache python py-pip && \
      pip install --no-cache-dir -r requirements.txt
  ```

### Lưu trữ và Chia sẻ Image:

#### 1. Đẩy lên Docker Hub:
- Đăng nhập vào Docker Hub từ Terminal bằng `docker login`.
- Đẩy Image của bạn lên Docker Hub:
  ```bash
  docker push my-username/my-python-app
  ```

#### 2. Sử dụng kho lưu trữ riêng:
- Cấu hình kho lưu trữ riêng bằng cách sử dụng các dịch vụ như Docker Registry hoặc các giải pháp lưu trữ dựa trên cloud.

Bằng cách tuân thủ các bước và kỹ thuật tối ưu hóa này, bạn có thể xây dựng Docker Image hiệu quả và chia sẻ chúng với cộng đồng hoặc trong tổ chức của mình.