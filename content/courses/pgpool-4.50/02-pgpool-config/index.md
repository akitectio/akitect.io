---
categories:
  - database
date: 2024-02-24T08:00:00+08:00
draft: false
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images:
  - /labs/postgresql/postgresql-pgpool.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - postgresql
  - ubuntu
  - pgpool
title: Lesson 2 - Cấu hình  Pgpool-II  
url: /lesson-2-pgpool-ii-configuration
description: PGpool-II là một giải pháp trung gian độc đáo, được thiết kế đặc biệt để tối ưu hóa và mở rộng khả năng của hệ quản trị cơ sở dữ liệu PostgreSQL. Nó mang lại nhiều lợi ích như việc tối ưu hóa kết nối, phân phối tải đều và thực hiện sao chép dữ liệu, biến PGpool-II thành công cụ không thể thiếu trong quản lý các triển khai PostgreSQL. Trong hướng dẫn chi tiết này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.
weight: 2
---


Cấu hình PGpool-II là một bước quan trọng trong quá trình triển khai cụm cơ sở dữ liệu PostgreSQL. Trong hướng dẫn này, chúng ta sẽ đi qua các bước để cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.


# Kiến trúc cài đặt

{{< figure src="./images/postgresql-pgpool.jpeg" >}}


Trước khi bắt đầu ta cần chuẩn bị 4 máy chủ

| IP           | Hostname             | vCPU   | RAM | DISK | OS           |
| ------------ | -------------------- | ------ | --- | ---- | ------------ |
| 192.168.56.5 | pgpool2              | 2 core | 4G  | 50G  | Ubuntu 22.04 |
| 192.168.56.2 | postgresql-master    | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave-01  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | postgresql-slave-02  | 4 core | 8G  | 50G  | Ubuntu 22.04 |

### Bước 1: Tài khoản người dùng PGpool-II

Đầu tiên, tạo một tài khoản người dùng `pgpool` trên tất cả các máy chủ `pgpool2`, `postgresql-master`, `postgresql-slave-01`, `postgresql-slave-02`:

```bash
sudo useradd -m -s /bin/bash pgpool
sudo passwd pgpool
```

Sau đó, thêm tài khoản người dùng `pgpool` vào nhóm `sudo`:

```bash
sudo usermod -aG sudo pgpool
```


#### Bước 2: Cấu hình `pcp.conf`

Đầu tiên, tạo một tệp cấu hình `pcp.conf` trên máy chủ `pgpool2`:

```bash
password_hash=$(pg_md5 Ehi@123)
echo "pgpool:$password_hash" >> /etc/pgpool2/pcp.conf
```

Trong đó `akitect@123` là mật khẩu của tài khoản `pgpool`, bạn có thể thay đổi mật khẩu theo ý muốn.

Sau đó, thêm quyền truy cập vào tệp `pcp.conf`:

```bash
sudo chown pgpool:pgpool /etc/pgpool2/pcp.conf
sudo chmod 0600 /etc/pgpool2/pcp.conf
```

#### Bước 3: Cấu hình `pgpool.conf`

Tiếp theo, cấu hình tệp `pgpool.conf` trên máy chủ `pgpool2`:

```bash
sudo chown pgpool:pgpool /etc/pgpool2/pgpool.conf
sudo chmod 0600 /etc/pgpool2/pgpool.conf
```

Sau đó, mở tệp `pgpool.conf` bằng trình soạn thảo văn bản yêu thích của bạn:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```


Thêm cấu hình sau vào tệp `pgpool.conf`:

```bash
listen_addresses = '*'
port = 5433

backend_hostname0 = '192.168.56.2'
backend_port0 = 5432
backend_weight0 = 1
backend_data_directory0 = '/home/ubuntu/postgresql-master'
backend_flag0 = 'ALLOW_TO_FAILOVER'
backend_application_name0 = 'postgresql-master'

backend_hostname1 = '192.168.56.3'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/home/ubuntu/postgresql-slave-01'
backend_flag1 = 'ALLOW_TO_FAILOVER'
backend_application_name1 = 'postgresql-slave-01'

backend_hostname2 = '192.168.56.4'
backend_port2 = 5432
backend_weight2 = 1
backend_data_directory2 = '/home/ubuntu/postgresql-slave-02'
backend_flag2 = 'ALLOW_TO_FAILOVER'
backend_application_name2 = 'postgresql-slave-02'

```

Trong đó `listen_addresses` là địa chỉ IP mà PGpool-II sẽ lắng nghe, `port` là cổng mà PGpool-II sẽ lắng nghe.

- `backend_hostname0`, `backend_port0`, `backend_weight0`, `backend_data_directory0`, `backend_flag0`, `backend_application_name0` là thông tin kết nối đến máy chủ `postgresql-master`.

- `backend_hostname1`, `backend_port1`, `backend_weight1`, `backend_data_directory1`, `backend_flag1`, `backend_application_name1` là thông tin kết nối đến máy chủ `postgresql-slave-01`.

- `backend_hostname2`, `backend_port2`, `backend_weight2`, `backend_data_directory2`, `backend_flag2`, `backend_application_name2` là thông tin kết nối đến máy chủ `postgresql-slave-02`.

Sau khi cấu hình xong, lưu và đóng tệp cấu hình.

#### Bước 4: Cấu hình Load Balancing và Failover 

- `Load Balancing` là một kỹ thuật phân phối tải giữa các máy chủ cơ sở dữ liệu, giúp tối ưu hóa hiệu suất và tăng cường khả năng chịu lỗi của hệ thống. 

- `Failover` là quá trình tự động chuyển đổi từ máy chủ cơ sở dữ liệu chính sang máy chủ cơ sở dữ liệu dự phòng khi máy chủ cơ sở dữ liệu chính gặp sự cố.

Để cấu hình `Load Balancing` và `Failover`, thêm cấu hình sau vào tệp `pgpool.conf`:

```bash
load_balance_mode = on
master_slave_mode = on
```

#### Bước 5: Cấu hình `pool_hba.conf`

Tiếp theo, cấu hình quản lý kết nối bằng cách chỉnh sửa tệp cấu hình `/etc/pgpool2/pool_hba.conf`:

```bash
sudo nano /etc/pgpool2/pool_hba.conf
```

Thêm cấu hình sau vào tệp `pool_hba.conf`:

```bash
host    all             all
```