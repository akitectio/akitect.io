---
categories:
  - database
date: 2023-10-26T08:00:00+08:00
description: Cài đặt và bảo mật PostgreSQL 16 trên Ubuntu 23.04
draft: false
featuredImage: /series/postgresql.png
images:
  - /cai-dat-va-bao-mat-postgresql-16-tren-ubuntu-2304/images/index.png
  - /series/postgresql.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - Database
  - PostgreSQL
  - Ubuntu
  - PostgreSQL 16
title: Cài đặt và bảo mật PostgreSQL 16 trên Ubuntu 23.04
url: /cai-dat-va-bao-mat-postgresql-16-tren-ubuntu-2304
---


### Bước 1: Thêm Package Repository PostgreSQL 16

```shell
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null
```

Để bắt đầu, hãy lấy phiên bản mới nhất của các gói. Chúng ta có thể đạt được điều này bằng cách sử dụng lệnh cập nhật apt như hình dưới đây:

```shell
sudo apt update
```

{{< figure src="/images/a0b112e0-aa5f-493a-ad99-693f029a2c3c.webp" >}}

### Bước 2: Cài dặt PostgreSQL 16 Database Server và Client

Để cài đặt chúng ta sử dụng lệnh

```shell
 sudo apt install postgresql postgresql-client -y
```

Sau khi chạy thành công ta kiểm tra PostgreSQL service có được start chưa:

```shell
 sudo systemctl status postgresql
```

{{< figure src="/images/7182fcd1-3e7f-4420-b3df-79eae97b8c08.webp" >}}

Như vậy ta đã cài đặt PostgreSQL thành công và kiểm tra phiên bản PostgreSQL bằng lệnh

```shell
 psql --version
```

{{< figure src="/images/e052ebdb-9948-4edd-aa8a-f54488fd2425.webp" >}}

Ở đây, chúng ta có thể thấy rằng phiên bản PostgreSQL là 16.

### Bước 3: Cập nhật mật khẩu User Admin PostgreSQL

Theo mặc định, chúng ta có thể kết nối với máy chủ PostgreSQL mà không cần sử dụng bất kỳ mật khẩu nào. Hãy xem điều này hoạt động bằng cách sử dụng tiện ích psql:

```shell
sudo -u postgres psql
```

{{< figure src="/images/485f7ad5-fc47-44d0-94c9-591625a85a45.webp" >}}

Trong đầu ra ở trên, lời nhắc **postgres=#** cho biết kết nối đang hoạt động với máy chủ PostgreSQL.

Tiếp tục chúng ta dùng lệnh để đổi password là **PassKhongChilaPasss**

```shell
ALTER USER postgres PASSWORD 'PassKhongChilaPasss';
```

sau đó chúng ta thoát khỏi bằng lệnh `\q`

{{< figure src="/images/a1eb8fd0-13ee-4273-92b0-9f34b9780fcf.webp" >}}

Bây giờ, hãy kết nối lại với máy chủ cơ sở dữ liệu:

```shell
psql -h localhost -U postgres
```

Hãy nhập chuỗi **PassKhongChilaPasss** làm mật khẩu và bây giờ chúng ta đã kết nối với cơ sở dữ liệu.

{{< figure src="/images/7eb308fa-5c0e-4891-9ae1-b7c6b60bf5c0.webp" >}}

Như vậy chúng ta đã đặt thành công mật khẩu cho user admin **postgres**

### Bước 4: Cấu hình cho PostgreSQL để cho phép kết nối từ xa

Theo mặc định, PostgreSQL chỉ chấp nhận các kết nối từ máy chủ cục bộ. Tuy nhiên, chúng ta có thể dễ dàng sửa đổi cấu hình để cho phép kết nối từ các máy khách từ xa.

PostgreSQL đọc cấu hình của nó từ tệp **postgresql.conf** nằm trong thư mục **/etc/postgresql/16/main/**. Ở đây, phiên bản cho biết phiên bản chính của PostgreSQL.

{{< figure src="/images/89716e24-0d66-4873-8ab1-d5ebe405ea9c.webp" >}}

Bây giờ, hãy mở tệp **postgresql.conf** trong trình soạn thảo văn bản, bỏ ghi chú dòng bắt đầu bằng **listen_addresses** và thay thế '**localhost**' bằng **'\*'**.

{{< figure src="/images/6da30c3e-b236-433e-8577-625b736d6257.webp" >}}

Lưu và đóng tập tin.

Tiếp theo, chỉnh sửa phần kết nối cục bộ IPv4 của tệp **pg_hba.conf** để cho phép kết nối **IPv4** từ tất cả các máy khách. Xin lưu ý rằng tệp này cũng nằm trong thư mục **/etc/postgresql/16/main/**.

{{< figure src="/images/77cb1ee8-37c7-459f-80ce-58493b8b8b47.webp" >}}

Trong trường hợp, tường lửa Ubuntu đang chạy trên hệ thống của bạn thì hãy cho phép cổng PostgreSQL 5432 bằng lệnh sau,

```shell
sudo ufw allow 5432/tcp
```

Sau đó ta dùng công cụ **PgAdmin** để kết nối thử, bạn có thể tải ở đây https://www.pgadmin.org/download/

{{< figure src="/images/7d35b81f-b04d-43e4-a16d-6e9da0382d4b.webp" >}}

Chúng ta điền các thông tin cần thiết như trên ảnh sau đó nhắp **Save**

{{< figure src="/images/b64860e4-bd93-4fae-9a34-b407a1f57b14.webp" >}}

Như vậy chúng ta đã kết nối thành công
