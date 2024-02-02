---
categories:
  - database
date: 2023-10-26T08:00:00+08:00
description: Cài đặt và bảo mật PostgreSQL 14 trên Ubuntu 22.04
draft: false
featuredImage: /series/postgresql.png
images:
  - /series/postgresql.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - Database
  - PostgreSQL
  - Ubuntu
  - PostgreSQL 14
title: Cài đặt và bảo mật PostgreSQL 14 trên Ubuntu 22.04
url: /cai-dat-va-bao-mat-postgresql-14-tren-ubuntu-2404
---

## Bước 1: Cài đặt PostgreSQL 14 trên Ubuntu 22.04.2 LTS

1. Mở terminal và cập nhật danh sách gói:

   ```bash
   sudo apt update
   ```

   {{< figure src="./images/01.png" >}}

2. Cài đặt PostgreSQL 14 bằng lệnh sau:

   ```bash
   sudo apt install postgresql-14 -y
   ```

   {{< figure src="./images/02.png" >}}

3. PostgreSQL sẽ tự động khởi động sau khi cài đặt. Bạn có thể kiểm tra trạng thái của nó bằng lệnh:

   ```bash
   sudo systemctl status postgresql
   ```

   {{< figure src="./images/03.png" >}}

## Bước 2: Bảo mật PostgreSQL

1. Bắt đầu bằng việc đặt mật khẩu cho tài khoản người dùng "postgres" (mặc định):

   ```bash
   sudo passwd postgres
   ```

   {{< figure src="./images/04.png" >}}

2. Chuyển sang tài khoản "postgres" và mở shell PostgreSQL:

   ```bash
   sudo -i -u postgres
   psql
   ```

   {{< figure src="./images/05.png" >}}

3. Đặt mật khẩu cho tài khoản "postgres":

   ```sql
   \password postgres
   ```

   {{< figure src="./images/06.png" >}}

   Nhập mật khẩu mới và xác nhận.

4. Thoát khỏi shell PostgreSQL:

   ```sql
   \q
   exit
   ```

   {{< figure src="./images/07.png" >}}

## Bước 3: Tạo Cơ sở dữ liệu và Người dùng

1. Đăng nhập vào PostgreSQL bằng tài khoản "postgres":

   ```bash
   sudo -i -u postgres
   ```

2. Mở shell PostgreSQL:

   ```bash
   psql
   ```

3. Tạo cơ sở dữ liệu (ví dụ: "mydatabase"):

   ```sql
   CREATE DATABASE mydatabase;
   ```

4. Tạo người dùng (ví dụ: "myuser") và đặt mật khẩu:

   ```sql
   CREATE USER myuser WITH PASSWORD 'mypassword';
   ```

5. Gán quyền cho người dùng để truy cập và quản lý cơ sở dữ liệu:

   ```sql
   GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;
   ```

6. Kết thúc phiên làm việc và thoát:

   ```sql
   \q
   exit
   ```

## Bước 4: Kiểm tra kết nối

```bash
psql -h localhost -U myuser -d mydatabase
```

{{< figure src="./images/08.png" >}}

## Bước 5: Cấu hình bảo mật

1. Mở file cấu hình:

   ```bash
   sudo nano /etc/postgresql/14/main/pg_hba.conf
   ```

   {{< figure src="./images/09.png" >}}

2. Thay đổi phương thức xác thực của tất cả các kết nối thành "md5":

   ```bash
   local   all             all                                     md5
   host    all             all
   ```

   {{< figure src="./images/10.png" >}}

3. Khởi động lại PostgreSQL:

   ```bash
   sudo systemctl restart postgresql
   ```

Cảm ơn các bạn đã đọc bài viết này.
