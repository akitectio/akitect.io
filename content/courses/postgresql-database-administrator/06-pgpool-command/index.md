---
categories:
  - database
date: 2024-02-24T08:00:00+08:00
draft: true
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images:
  - /labs/postgresql/postgresql-pgpool.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - postgresql
  - ubuntu
  - pgpool
title: Lesson 5 -  Postgres Pgpool-II Ubuntu 
url: /pgpool-ii-ubuntu-cau-hinh-tung-buoc
description: PGpool-II là một giải pháp trung gian độc đáo, được thiết kế đặc biệt để tối ưu hóa và mở rộng khả năng của hệ quản trị cơ sở dữ liệu PostgreSQL. Nó mang lại nhiều lợi ích như việc tối ưu hóa kết nối, phân phối tải đều và thực hiện sao chép dữ liệu, biến PGpool-II thành công cụ không thể thiếu trong quản lý các triển khai PostgreSQL. Trong hướng dẫn chi tiết này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.
weight: 5
---

# Pgpool-II là gì 

PGpool-II là một giải pháp trung gian độc đáo, được thiết kế đặc biệt để tối ưu hóa và mở rộng khả năng của hệ quản trị cơ sở dữ liệu PostgreSQL. Nó mang lại nhiều lợi ích như việc tối ưu hóa kết nối, phân phối tải đều và thực hiện sao chép dữ liệu, biến PGpool-II thành công cụ không thể thiếu trong quản lý các triển khai PostgreSQL. Trong hướng dẫn chi tiết này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.

# Kiến trúc cài đặt

{{< figure src="./images/postgresql-pgpool.jpeg" >}}


Trước khi bắt đầu ta cần chuẩn bị 4 máy chủ

| IP           | Hostname             | vCPU   | RAM | DISK | OS           |
| ------------ | -------------------- | ------ | --- | ---- | ------------ |
| 192.168.56.5 | pgpool2              | 2 core | 4G  | 50G  | Ubuntu 22.04 |
| 192.168.56.2 | postgresql-master    | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave-01  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | postgresql-slave-02  | 4 core | 8G  | 50G  | Ubuntu 22.04 |

### Cài đặt PostgreSQL Replication 

[Cài đặt PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc) trên 3 máy chủ `postgresql-master` và `postgresql-slave-01`, `postgresql-slave-02`.

### Cài đặt PGpool-II

#### Bước 1: Cài đặt PGpool-II

Đầu tiên, cài đặt PGpool-II trên máy chủ `pgpool2` bằng cách thực hiện các bước sau:

```bash
sudo apt update
sudo apt install -y pgpool2
```

#### Bước 2: Cấu hình PGpool-II

Sau khi cài đặt xong, chúng ta sẽ cấu hình PGpool-II bằng cách chỉnh sửa tệp cấu hình `/etc/pgpool2/pgpool.conf`:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```

Thêm cấu hình sau vào tệp:

```bash
listen_addresses = '*'
port = 5432 

# only write queries are load balanced
backend_hostname0 = '192.168.56.2' 
backend_port0 = 5432
backend_weight0 = 2  
backend_data_directory0 = '/var/lib/postgresql/16/main' 
backend_application_name0 = 'postgresql-master'

# only read queries are load balanced
backend_hostname1 = '192.168.56.3'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/var/lib/postgresql/16/main' 
backend_application_name0 = 'postgresql-slave-01'

backend_hostname2 = '192.168.56.4'
backend_port2 = 5432
backend_weight2 = 1
backend_data_directory2 = '/var/lib/postgresql/16/main'
backend_application_name0 = 'postgresql-slave-02'

load_balance_mode = on
master_slave_mode = on 
master_slave_sub_mode = 'stream'

allow_sql_comments = true
```

Trong đó: 

- `listen_addresses`: Địa chỉ IP mà PGpool-II sẽ lắng nghe các kết nối đến.
- `port`: Cổng mà PGpool-II sẽ lắng nghe các kết nối đến.
- `backend_hostname0`: Địa chỉ IP của máy chủ PostgreSQL master.
- `backend_port0`: Cổng của máy chủ PostgreSQL master.
- `backend_weight0`: Trọng số của máy chủ PostgreSQL master.
- `backend_data_directory0`: Đường dẫn đến thư mục dữ liệu của máy chủ PostgreSQL master.
- `backend_hostname1`: Địa chỉ IP của máy chủ PostgreSQL slave 1.
- `backend_port1`: Cổng của máy chủ PostgreSQL slave 1.
- `backend_weight1`: Trọng số của máy chủ PostgreSQL slave 1.
- `backend_data_directory1`: Đường dẫn đến thư mục dữ liệu của máy chủ PostgreSQL slave 1.
- `backend_hostname2`: Địa chỉ IP của máy chủ PostgreSQL slave 2.
- `backend_port2`: Cổng của máy chủ PostgreSQL slave 2.
- `backend_weight2`: Trọng số của máy chủ PostgreSQL slave 2.
- `backend_data_directory2`: Đường dẫn đến thư mục dữ liệu của máy chủ PostgreSQL slave 2.
- `load_balance_mode`: Chế độ phân phối tải đều.
- `master_slave_mode`: Chế độ master/slave.
- `master_slave_sub_mode`: Chế độ sao chép dữ liệu.

#### Bước 3: Cấu hình quản lý kết nối

Tiếp theo, chúng ta sẽ cấu hình quản lý kết nối bằng cách chỉnh sửa tệp cấu hình `/etc/pgpool2/pool_hba.conf`:

```bash
sudo nano /etc/pgpool2/pool_hba.conf
```

Thêm cấu hình sau vào tệp:

```bash
host    all         all         0.0.0.0/0          trust
```

#### Bước 4: Khởi động PGpool-II

Cuối cùng, khởi động lại dịch vụ PGpool-II để áp dụng các thay đổi:

```bash
sudo systemctl restart pgpool2
```

Để đảm bảo PGpool-II tự động khởi động khi khởi động hệ thống, bạn có thể kích hoạt dịch vụ PGpool-II bằng lệnh sau:

```bash
sudo systemctl enable pgpool2
```

#### Bước 5: Test kết nối, ta dùng pgadmin để kết nối đến pgpool2

Thông tin đăng nhập như sau:

- `Host`: 192.168.56.5
- `Port`: 5432
- `Username`: postgres
- `Password`: ở bước cài đặt PostgreSQL Replication

{{< figure src="./images/pgpool-pgadmin.jpg" >}}

Như vậy ta đã cài đặt và cấu hình PGpool-II thành công.

### Kết luận
Trong hướng dẫn này, chúng ta đã đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux. Bằng cách thực hiện các bước này, bạn có thể tối ưu hóa và mở rộng khả năng của cơ sở dữ liệu PostgreSQL, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.