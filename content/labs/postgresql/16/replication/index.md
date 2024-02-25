---
categories:
  - database
date: 2024-02-22T08:00:00+08:00
draft: false
featuredImage: /series/postgresql.png
images:
  - /cai-dat-va-bao-mat-postgresql-16-tren-ubuntu-2304/images/index.png
  - /series/postgresql.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - replication 
  - ubuntu
title: Thiết lập PostgreSQL 15 Replication - Hướng chi tiết từng bước
url: /thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc
description : PostgreSQL có tính năng sao chép tầng, cho phép sao chép dữ liệu từ DB này sang DB khác, tạo nhiều bản sao dữ liệu. Tính năng này giúp phân phối dữ liệu, đảm bảo dữ liệu mới nhất và hỗ trợ thay thế máy chủ chính.
weight: 1
---

## PostgreSQL Replication (Sao Chép) là gì

PostgreSQL là một hệ thống quản lý cơ sở dữ liệu quan hệ mã nguồn mở linh hoạt, nổi tiếng với hiệu suất và độ bền vững của nó. Thiết lập replication (sao chép) giữa máy chủ chính (master) và máy chủ phụ (slave) là bước quan trọng trong việc đảm bảo khả năng sẵn sàng cao và dự phòng dữ liệu trong môi trường PostgreSQL. Trong hướng dẫn này, chúng tôi sẽ hướng dẫn bạn quy trình cài đặt PostgreSQL trên máy chủ phụ và cấu hình sao chép để nhân bản dữ liệu từ máy chủ chính.

## Kiến trúc cài đặt

{{< figure src="./images/postgresql-15-replication.png" >}}

## Chuẩn bị môi trường

Trước khi bắt đầu ta cần chuẩn bị 2                                      máy chủ

| IP           | Hostname          | vCPU   | RAM | DISK | OS           |
| ------------ | ----------------- | ------ | --- | ---- | ------------ |
| 192.168.56.2 | postgresql-master | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave  | 4 core | 8G  | 50G  | Ubuntu 22.04 |

## Cài đặt PostgreSQL 16

[Cài đặt và bảo mật PostgreSQL 16 trên Ubuntu 22.04](/cai-dat-va-bao-mat-postgresql-16-tren-ubuntu-2304) trên 2 máy chủ `postgresql-master` và `postgresql-slave`

## Cấu hình PostgreSQL 16 Replication

### Bước 1: Cấu hình PostgreSQL Master

Đầu tiên, chúng ta sẽ cấu hình máy chủ chính để cho phép sao chép dữ liệu từ máy chủ chính sang máy chủ phụ. Để bắt đầu, hãy mở tệp cấu hình PostgreSQL trên máy chủ chính bằng trình soạn thảo văn bản yêu thích của bạn. Trong hướng dẫn này, chúng tôi sẽ sử dụng trình soạn thảo nano.

```shell
sudo nano /etc/postgresql/16/main/postgresql.conf
```

Tìm và sửa các dòng sau:

```shell
listen_addresses = '*'
wal_level = replica
max_wal_senders = 3
wal_keep_segments = 8
```

- `wal_level` là mức độ của các bản ghi WAL (Write-Ahead Log) mà máy chủ chính sẽ tạo ra. Mức độ này cần được thiết lập thành `replica` để cho phép máy chủ chính gửi bản ghi WAL đến máy chủ phụ.

- `max_wal_senders` là số lượng kết nối máy chủ phụ có thể kết nối đến máy chủ chính để nhận bản ghi WAL.

- `wal_keep_segments` là số lượng bản ghi WAL mà máy chủ chính sẽ giữ lại trước khi xóa chúng.

Sau khi sửa xong, lưu và thoát tệp cấu hình.

### Bước 2: Cấu hình PostgreSQL Slave

Tiếp theo, chúng ta sẽ cấu hình máy chủ phụ để nhận dữ liệu từ máy chủ chính. Mở tệp cấu hình PostgreSQL trên máy chủ phụ bằng trình soạn thảo văn bản yêu thích của bạn.

```shell
sudo nano /etc/postgresql/16/main/postgresql.conf
```

Tìm và sửa các dòng sau:

```shell
listen_addresses = '*'
hot_standby = on
```

Sau khi sửa xong, lưu và thoát tệp cấu hình.

### Bước 3: Cấu hình phân quyền sao chép

Tiếp theo, chúng ta sẽ cấu hình tệp `pg_hba.conf` để cho phép máy chủ phụ kết nối đến máy chủ chính và nhận dữ liệu sao chép. Mở tệp `pg_hba.conf` bằng trình soạn thảo văn bản yêu thích của bạn trên máy chủ chính.

```shell
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

Thêm dòng sau vào cuối tệp:

```shell
host replication replicator
```

Sau khi sửa xong, lưu và thoát tệp cấu hình.

### Bước 4: Khởi động lại PostgreSQL

Cuối cùng, hãy khởi động lại dịch vụ PostgreSQL trên cả máy chủ chính và máy chủ phụ để áp dụng các thay đổi cấu hình.

```shell
sudo systemctl restart postgresql
```

## Kiểm tra PostgreSQL Replication

### Bước 1: Kiểm tra trạng thái sao chép

Để kiểm tra trạng thái sao chép, chúng ta sẽ sử dụng tiện ích `pg_stat_replication` trên máy chủ chính.

```shell
sudo -u postgres psql -c "SELECT * FROM pg_stat_replication;"
```

### Bước 2: Kiểm tra trạng thái sao chép

Để kiểm tra trạng thái sao chép, chúng ta sẽ sử dụng tiện ích `pg_stat_replication` trên máy chủ chính.

```shell
sudo -u postgres psql -c "SELECT * FROM pg_stat_replication;"
```

