---
categories:
  - database
date: 2024-02-22T08:00:00+08:00
draft: false
featuredImage: /labs/postgresql/postgresql-16-replication.jpeg
images:
  - /labs/postgresql/postgresql-16-replication.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - replication 
  - ubuntu
title: Lesson 4 - PostgreSQL 16 Replication 
url: /thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc
description : PostgreSQL có tính năng sao chép tầng, cho phép sao chép dữ liệu từ DB này sang DB khác, tạo nhiều bản sao dữ liệu. Tính năng này giúp phân phối dữ liệu, đảm bảo dữ liệu mới nhất và hỗ trợ thay thế máy chủ chính.
weight: 4
---

## PostgreSQL Replication (Sao Chép) là gì

PostgreSQL là một hệ thống quản lý cơ sở dữ liệu quan hệ mã nguồn mở linh hoạt, nổi tiếng với hiệu suất và độ bền vững của nó. Thiết lập replication (sao chép) giữa máy chủ chính (master) và máy chủ phụ (slave) là bước quan trọng trong việc đảm bảo khả năng sẵn sàng cao và dự phòng dữ liệu trong môi trường PostgreSQL. Trong hướng dẫn này, chúng tôi sẽ hướng dẫn bạn quy trình cài đặt PostgreSQL trên máy chủ phụ và cấu hình sao chép để nhân bản dữ liệu từ máy chủ chính.

## Kiến trúc cài đặt

{{< figure src="./images/postgresql-16-replication.jpeg" >}}

## Chuẩn bị môi trường

Trước khi bắt đầu ta cần chuẩn bị 3 máy chủ

| IP           | Hostname             | vCPU   | RAM | DISK | OS           |
| ------------ | -----------------    | ------ | --- | ---- | ------------ |
| 192.168.56.2 | postgresql-master    | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave-01  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | postgresql-slave-02  | 4 core | 8G  | 50G  | Ubuntu 22.04 |

## Cài đặt PostgreSQL 16

[Cài đặt và bảo mật PostgreSQL 16 trên Ubuntu 22.04](/cai-dat-va-bao-mat-postgresql-16-tren-ubuntu-2204) trên 3 máy chủ `postgresql-master` và `postgresql-slave-01`, `postgresql-slave-02`.

Kiểm tra phiên bản PostgreSQL

```shell
psql --version
```
{{< figure src="./images/check-version-postgresql.png" >}}

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
max_wal_senders = 10
max_replication_slots = 10
```

- `wal_level` là mức độ của các bản ghi WAL (Write-Ahead Log) mà máy chủ chính sẽ tạo ra. Mức độ này cần được thiết lập thành `replica` để cho phép máy chủ chính gửi bản ghi WAL đến máy chủ phụ.

- `max_replication_slots` là số lượng khe sao chép tối đa mà máy chủ chính có thể tạo ra. Khe sao chép là một cơ chế để giữ các bản sao của bản ghi WAL cho máy chủ phụ. Mức độ này cần được thiết lập thành `10` để cho phép máy chủ chính tạo ra tối đa 10 khe sao chép.

Sau khi sửa xong, lưu và thoát tệp cấu hình.

Tiếp theo, chúng ta sẽ cấu hình tệp `pg_hba.conf` để cho phép máy chủ phụ kết nối đến máy chủ chính và nhận dữ liệu sao chép. Mở tệp `pg_hba.conf` bằng trình soạn thảo văn bản yêu thích của bạn trên máy chủ chính.

```shell
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

Thêm dòng sau vào cuối tệp:

```shell
host    replication    replicator              192.168.56.3/32         md5
host    replication    replicator              192.168.56.4/32         md5
```

{{< figure src="./images/pg_hba.conf.jpg" >}}

Sau khi sửa xong, lưu và thoát tệp cấu hình.


Tiếp theo, chúng ta sẽ tạo một user có quyền sao chép dữ liệu từ máy chủ `postgresql-master` sang máy chủ `postgresql-slave` . Đầu tiên, hãy đăng nhập vào máy chủ chính và mở `psql` console.

```shell
sudo -u postgres psql
CREATE USER replicator WITH REPLICATION LOGIN ENCRYPTED PASSWORD '71e1b930e5d53ea4c8f02ccfd255d7cc';
```

`71e1b930e5d53ea4c8f02ccfd255d7cc` là mật khẩu mạnh mà bạn muốn sử dụng cho user replicator. ở đây mình đặt là `71e1b930e5d53ea4c8f02ccfd255d7cc`, bạn vui lòng sử dụng m5 để tạo mật khẩu mạnh hơn.

Sau khi tạo xong, thoát khỏi `psql` console.

Khởi động lại PostgreSQL service

```shell
sudo systemctl restart postgresql
```

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


#### 2.1 : Stop PostgreSQL service

```shell
sudo systemctl stop postgresql
```
#### 2.2 : Xóa dữ liệu hiện tại của PostgreSQL

```shell
sudo rm -rf /var/lib/postgresql/16/main
```
#### 2.2 : Sao lưu dữ liệu từ máy chủ chính sang máy chủ phụ

`postgresql-slave-01` gõ lệnh sau:

```shell
export PGPASSWORD=71e1b930e5d53ea4c8f02ccfd255d7cc

pg_basebackup -h 192.168.56.2 -U replicator -R -X stream -C -S replica_1 -v -R -W -D/var/lib/postgresql/16/main -w

# Set permission
chown -R postgres:postgres /var/lib/postgresql/16/main
```

`postgresql-slave-01` gõ lệnh sau:

```shell
export PGPASSWORD=71e1b930e5d53ea4c8f02ccfd255d7cc

pg_basebackup -h 192.168.56.2 -U replicator -R -X stream -C -S replica_2 -v -R -W -D/var/lib/postgresql/16/main -w

# Set permission
chown -R postgres:postgres /var/lib/postgresql/16/main
```

{{< figure src="./images/pg_basebackup.jpg" >}}

#### 2.3 : Cấu hình file recovery.conf trên máy `postgresql-slave-01` và `postgresql-slave-02`

```shell
sudo nano /var/lib/postgresql/16/main/recovery.conf
```

Thêm nội dung sau vào tệp `recovery.conf`:

```shell
standby_mode = 'on'
primary_conninfo = 'host=192.168.56.2 port=5432 user=replicator password=71e1b930e5d53ea4c8f02ccfd255d7cc'
```

Sau khi sửa xong, lưu và thoát tệp cấu hình.

`71e1b930e5d53ea4c8f02ccfd255d7cc` là mật khẩu mà bạn đã tạo ở bước 1.
`192.168.56.2` là địa chỉ IP của máy `postgresql-master`.

#### 2.4 : Start PostgreSQL service

```shell
sudo systemctl start postgresql
```

## Kiểm tra PostgreSQL Replication

### Bước 1: Kiểm tra trạng thái PostgreSQL Master

Đầu tiên, hãy đăng nhập vào máy chủ `postgresql-master` và mở `psql` console.

```shell
sudo -u postgres psql
```

Kiểm tra trạng thái của máy chủ `postgresql-master`:

```shell
SELECT client_addr, state
FROM pg_stat_replication;
```

{{< figure src="./images/pg_stat_replication.jpg" >}}

Như vậy ta đã cấu hình thành công 2 máy `postgresql-slave-01` và `postgresql-slave-02` nhận dữ liệu từ máy `postgresql-master`.

### Bước 2: Kiểm tra trạng thái PostgreSQL Slave

Đăng nhập vào máy chủ `postgresql-slave` và mở `psql` console.

```shell
sudo -u postgres psql
```

Kiểm tra trạng thái của máy chủ `postgresql-slave`:

```shell
SELECT status, receive_start_lsn, sender_host  FROM pg_stat_wal_receiver;
```

{{< figure src="./images/pg_stat_wal_receiver.jpg" >}}

## Kiểm tra thiết lập sao chép dữ liệu

Để kiểm tra xem sao chép dữ liệu có hoạt động hay không, hãy thêm một số dữ liệu vào máy chủ chính và kiểm tra xem dữ liệu có được sao chép sang máy chủ phụ không.

### Bước 1: Thêm dữ liệu vào máy chủ `postgresql-master`

```shell
sudo -u postgres psql
```

```shell
CREATE DATABASE test;

\c test

CREATE TABLE users (id SERIAL PRIMARY KEY, name VARCHAR(100));

INSERT INTO users (name) VALUES ('DUY TRAN - AKITECT.IO');
```

{{< figure src="./images/check-replication-postgresql-master.jpg" >}}

### Bước 2: Kiểm tra dữ liệu trên máy chủ `postgresql-slave`

```shell
sudo -u postgres psql
```

```shell
\c test

SELECT * FROM users;
```

{{< figure src="./images/check-replication-slave.jpg" >}}

Như vậy ta đã cấu hình thành công PostgreSQL 16 Replication giữ máy chủ `postgresql-master` và `postgresql-slave-01`, `postgresql-slave-02`.

## Kết luận

Trong hướng dẫn này, chúng tôi đã hướng dẫn bạn cách cài đặt và cấu hình PostgreSQL 16 Replication giữa máy chủ chính và máy chủ phụ. Bằng cách cấu hình sao chép dữ liệu, bạn có thể nhân bản dữ liệu từ máy chủ chính sang máy chủ phụ, giúp phân phối dữ liệu, đảm bảo dữ liệu mới nhất và hỗ trợ thay thế máy chủ chính.
