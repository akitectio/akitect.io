---
categories: [database]
date: 2024-02-24T00:00:00.000Z
draft: false
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images: [/labs/postgresql/postgresql-pgpool.jpeg]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [database, postgresql, ubuntu, pgpool]
title: Lesson 2 - Load balancing và Replication
description: PGpool-II là một giải pháp trung gian độc đáo, được thiết kế đặc biệt để tối ưu hóa và mở rộng khả năng của hệ quản trị cơ sở dữ liệu PostgreSQL. Nó mang lại nhiều lợi ích như việc tối ưu hóa kết nối, phân phối tải đều và thực hiện sao chép dữ liệu, biến PGpool-II thành công cụ không thể thiếu trong quản lý các triển khai PostgreSQL. Trong hướng dẫn chi tiết này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.
weight: 2
---

Cấu hình PGpool-II là một bước quan trọng trong quá trình triển khai cụm cơ sở dữ liệu PostgreSQL. Trong hướng dẫn này, chúng ta sẽ đi qua các bước để cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.

# Kiến trúc cài đặt

{{< figure src="./images/postgresql-pgpool.jpeg" >}}

Trước khi bắt đầu ta cần chuẩn bị 4 máy chủ

| IP            | Hostname    | vCPU   | RAM | DISK | OS           |
| ------------- | ----------- | ------ | --- | ---- | ------------ |
| 192.168.50.10 | pgpool2     | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.11 | pg-master   | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.12 | pg-slave-01 | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.13 | pg-slave-02 | 2 core | 4G  | 60G  | Ubuntu 22.04 |

#### Bước 3: Cấu hình Load Balancing và Replication

Bạn cần phải cấu hình Replication trước, nếu bạn chưa cấu hình thì tham khảo [Cài đặt PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc)

Thêm user `replicator` vào 

Tạo tệp `pcp.conf`:

```bash
password_hash=$(pg_md5 PassKhongChilaPasss) && echo "postgre:$password_hash" >> /etc/pgpool2/pcp.conf
```

Tiếp theo, cấu hình tệp `pgpool.conf` trên máy chủ `pgpool2`:

```bash
sudo chown pgpool:pgpool /etc/pgpool2/pgpool.conf
sudo chmod 0600 /etc/pgpool2/pgpool.conf
```

Sau đó, mở tệp `pgpool.conf` bằng trình soạn thảo văn bản `nano`:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```

Thêm cấu hình sau vào tệp `pgpool.conf`:

```bash
listen_addresses = '*' 
port = 9999 

backend_hostname0 = '192.168.56.11'
backend_port0 = 5432
backend_weight0 = 0
backend_data_directory0 = '/home/ubuntu/postgresql/master'
backend_flag0 = 'ALLOW_TO_FAILOVER'
backend_application_name0 = 'postgresql-master'

backend_hostname1 = '192.168.50.12'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/home/ubuntu/postgresql/slave-01'
backend_flag1 = 'ALLOW_TO_FAILOVER'
backend_application_name1 = 'postgresql-slave-01'

backend_hostname2 = '192.168.50.13'
backend_port2 = 5432
backend_weight2 = 2
backend_data_directory2 = '/home/ubuntu/postgresql/slave-02'
backend_flag2 = 'ALLOW_TO_FAILOVER'
backend_application_name2 = 'postgresql-slave-02'

log_statement = on
log_per_node_statement = on

sr_check_user = 'replicator'
health_check_user = 'replicator'
health_check_period = 10


# Cấu hình quản lý process 
process_management_mode = static
process_management_strategy = lazy
num_init_children = 320
min_spare_children = 1000
max_spare_children = 5000
max_pool = 1000

```

Trong đó `listen_addresses` là địa chỉ IP mà PGpool-II sẽ lắng nghe, `port` là cổng mà PGpool-II sẽ lắng nghe.

-   `backend_hostname0`, `backend_port0`, `backend_weight0`, `backend_data_directory0`, `backend_flag0`, `backend_application_name0` là thông tin kết nối đến máy chủ `postgresql-master`.

-   `backend_hostname1`, `backend_port1`, `backend_weight1`, `backend_data_directory1`, `backend_flag1`, `backend_application_name1` là thông tin kết nối đến máy chủ `postgresql-slave-01`.

-   `backend_hostname2`, `backend_port2`, `backend_weight2`, `backend_data_directory2`, `backend_flag2`, `backend_application_name2` là thông tin kết nối đến máy chủ `postgresql-slave-02`.

-   `log_statement` và `log_per_node_statement` là cấu hình ghi log truy vấn.

-   `sr_check_user` và `health_check_user` là tên người dùng kiểm tra sức khỏe của máy chủ cơ sở dữ liệu. `replicator` là user mà chúng ta đã tạo ở bước [Cài đặt PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc)

-   `backend_flag` là cờ cho phép `FAILOVER` khi máy chủ `postgresql-master` bị lỗi, `ALLOW_TO_FAILOVER` là cờ cho phép `FAILOVER` khi máy chủ `postgresql-slave-01` và `postgresql-slave-02` bị lỗi. Các cờ khác: 

-   `DISALLOW_TO_FAILOVER` : không cho phép `FAILOVER`

-   `ALWAYS_MASTER` : luôn là máy chủ `postgresql-master`

-   `process_management_mode` là chế độ quản lý process, `static` là chế độ tĩnh, `dynamic` là chế độ động.
    -   `static` : Số lượng process sẽ được quản lý tĩnh, không thay đổi.
    -   `dynamic` : Số lượng process sẽ được quản lý động, tùy thuộc vào tải của hệ thống.

-   `process_management_strategy` là chiến lược quản lý process, `lazy` là chế độ lười biếng, `smart` là chế độ thông minh.
    -   `lazy` : Chỉ tạo process khi cần thiết.
    -   `smart` : Tạo process trước, tránh tình trạng chậm trễ khi cần thiết.

-   `num_init_children` là số lượng process ban đầu, nếu dưới sẽ tạo thêm process.

-   `min_spare_children` là số lượng process tối thiểu, nếu dưới sẽ tạo thêm process.

-   `max_spare_children` là số lượng process tối đa, nếu vượt quá sẽ bị kill.

-   `max_pool` là số lượng kết nối tối đa, nếu vượt quá sẽ bị từ chối kết nối.

#### Bước 4: Chạy kiểm tra cấu hình PGpool-II

Cuối cùng, chạy kiểm tra cấu hình PGpool-II:

```bash
sudo /usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
```

{{< figure src="./images/run-pgpool-configuration.jpg" >}}

Sau khi cấu hình xong, bạn có thể kiểm tra trạng thái của PGpool-II bằng các truy cập vào bằng pg4admin : 

{{< figure src="./images/pg4admin-pgpool.jpg" >}}

-   `ip_address` là địa chỉ IP (192.168.56.5) của máy chủ `pgpool2`.
-   `username`  mặc định là `postgresql`.
-   `password` là mật khẩu của tài khoản `postgresql`.

#### Bước 5: Tạo dữ liệu test

Ở đây mình sẽ tạo một cơ sở dữ liệu `student` và `10000` dữ liệu test :

```bash

# Tạo cơ sở dữ liệu student
CREATE TABLE student (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100),
    date_of_birth DATE,
    grade_level INT CHECK (grade_level BETWEEN 1 AND 12)
);



WITH generated_data AS (
       SELECT 
           substring(md5(random()::text) from 1 for 8) AS first_name,
           substring(md5(random()::text) from 1 for 8) AS last_name,
           concat(substring(md5(random()::text) from 1 for 6), '@example.com') AS email,
           '1990-01-01'::date + random() * (age(now(), '1990-01-01'::date)) AS date_of_birth,
           (random() * 11 + 1)::int AS grade_level
       FROM generate_series(1,1000000) -- Generate 100 rows
   )
   INSERT INTO student (first_name, last_name, email, date_of_birth, grade_level)
   SELECT * FROM generated_data; 
  
```

{{< figure src="./images/pg4admin-pgpool-student.jpg" >}}

Như ảnh ta thấy dữ liệu `INSERT` đã được thực hiện trên  máy chủ `postgresql-master`.

Tiếp tục ta Query bằng lệnh `SELECT` trên PG4Admin:

```bash
SELECT * FROM student;
```

{{< figure src="./images/pg4admin-pgpool-student-select.jpg" >}}

Sau đó ta quay lại xem log của máy chủ `pgpool2` 

{{< figure src="./images/pgpool-log.jpg" >}}

Như ảnh ta thấy `SELECT` đã được thực hiện trên máy chủ `postgresql-slave-01` thông qua máy chủ `pgpool2`.

Như vậy chúng ta đã cấu hình thành công PGpool-II để thực hiện Load Balancing và Replication.
