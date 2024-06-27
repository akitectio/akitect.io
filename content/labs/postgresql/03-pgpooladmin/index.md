---
categories:
  - database
date: 2024-02-25T08:00:00+08:00
draft: false
featuredImage: /labs/postgresql/postgresql-pgpooladmin.jpeg
images:
  - /labs/postgresql/postgresql-pgpooladmin.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - postgreSQL
  - ubuntu
  - pgpool
title: Pgpool-II Admin Ubuntu 
url: /cai-dat-va-cau-hinh-pgpool-ii-admin-tren-ubuntu
description: Pgpool Admin là một công cụ quản lý cấu hình và giám sát PGpool-II, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu PostgreSQL. Trong hướng dẫn này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool Admin trên hệ điều hành Ubuntu Linux, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu của mình.
weight: 1
---

## Pgpool Admin là gì

Pgpool Admin là một công cụ quản lý cấu hình và giám sát PGpool-II, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu PostgreSQL. Trong hướng dẫn này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool Admin trên hệ điều hành Ubuntu Linux, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu của mình.

## Kiến trúc cài đặt

{{< figure src="/images/postgresql-pgpooladmin.jpeg" >}}

### Cài đặt PGpool Admin trên Ubuntu

#### Bước 1: Cài đặt apache2 và php

Thêm repository php

```bash
sudo apt-add-repository ppa:ondrej/php
sudo apt update
```

Đầu tiên, cài đặt Apache2 và PHP trên máy chủ `pgpool2` bằng cách thực hiện các bước sau:

```bash
sudo apt install -y apache2  php7.4 libapache2-mod-php7.4 php7.4-cli php7.4-common php7.4-pgsql 
```

Sau khi cài đặt xong, kiểm tra phiên bản PHP:

```bash
php -v
```

{{< figure src="/images/php-version.jpg" >}}

#### Bước 2: Tải PGpool Admin 4.2.0

Đầu tiên, tải PGpool Admin từ trang web chính thức của dự án PGpool-II:

```bash
wget https://www.pgpool.net/mediawiki/download.php?f=pgpoolAdmin-4.2.0.tar.gz -O pgpoolAdmin-4.2.0.tar.gz
```

Sau khi tải xong, giải nén tệp tải về:

```bash
tar -xvf pgpoolAdmin-4.2.0.tar.gz
```

Di chuyển tệp giải nén vào thư mục `/var/www/html`:

```bash
sudo mv pgpoolAdmin-4.2.0 /var/www/html/pgpooladmin
```

sau khi giải nén xong, ta cần phải cấu hình lại quyền truy cập cho thư mục `pgpooladmin`:

```bash
sudo chown -R www-data:www-data /var/www/html/pgpooladmin
```

{{< figure src="/images/pgpooladmin-directory.jpg" >}}

#### Bước 3: Cấu hình apache2

Tiếp theo, cấu hình Apache2 để chạy PGpool Admin bằng cách tạo một tệp cấu hình mới:

```bash
sudo nano /etc/apache2/sites-available/pgpooladmin.conf
```

Thêm cấu hình sau vào tệp:

```bash
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/pgpooladmin
    <Directory /var/www/html/pgpooladmin>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

{{< figure src="/images/pgpooladmin-apache2.jpg" >}}

Sau khi cấu hình xong, lưu và đóng tệp cấu hình.

#### Kích hoạt cấu hình

Tiếp theo, kích hoạt tệp cấu hình mới bằng lệnh sau:

```bash
sudo a2ensite pgpooladmin.conf
```

Cuối cùng, khởi động lại dịch vụ Apache2 để áp dụng các thay đổi:

```bash
sudo systemctl restart apache2
```

### Bước 4: Cấu hình PGpool-II

Tạo password pgpooladmin

```bash
pg_md5 akitect@123
```

{{< figure src="/images/pg_md5.jpg" >}}

Tiếp theo, chúng ta sẽ cấu hình PGpool-II bằng cách chỉnh sửa tệp cấu hình `/etc/pgpool2/pgpool.conf`:

```bash
sudo nano /etc/pgpool2/pcp.conf
```

{{< figure src="/images/pgpool-pcp.jpg" >}}

trong đó `admin` là user mà bạn muốn sử dụng để đăng nhập vào pgpooladmin, `md5` là mật khẩu mà bạn đã tạo ở trên.

Tiếp theo tạo và gán quyền cho tệp `.pcppass`:

```bash
# Tạo tệp .pcppass
touch /var/www/.pcppass

# Gán quyền cho tệp .pcppass
chmod -R 0600 /var/www/.pcppass 
```

### Bước 5: Truy cập PGpool Admin và cấu hình ban đầu

Sau khi cài đặt xong, truy cập PGpool Admin bằng cách mở trình duyệt web và truy cập địa chỉ IP của máy chủ `pgpool2` : [http://192.168.56.5/pgpooladmin/install](http://192.168.56.5/pgpooladmin/install)

{{< figure src="/images/pgpooladmin-install.jpg" >}}

chọn ngôn ngữ và nhấn `Next`

{{< figure src="/images/pgpooladmin-install-2.jpg" >}}

Tiếp tục thêm thông tin cấu hình kết nối đến `pgpool2`

{{< figure src="/images/pgpooladmin-install-3.jpg" >}}

- `pgpool.conf` nhập `/etc/pgpool2/pgpool.conf`
- `pcp.conf` nhập `/etc/pgpool2/pcp.conf`
- `pgpool Command` nhập `/usr/sbin/pgpool`
- `PCP directory` nhập `/usr/sbin`

Tiếp tục nhấn `Next`

{{< figure src="/images/pgpooladmin-install-4.jpg" >}}

Như vậy đã setup ban đầu xong, ta xoá thư mục `install` để bảo mật hơn.

```bash
sudo rm -rf /var/www/html/pgpooladmin/install
```

{{< figure src="/images/pgpooladmin-install-5.jpg" >}}

### Bước 6: Đăng nhập vào PGpool Admin

Sau khi cài đặt xong, truy cập PGpool Admin bằng cách mở trình duyệt web và truy cập địa chỉ IP của máy chủ `pgpool2` : [http://192.168.56.5/pgpooladmin](http://192.168.56.5/pgpooladmin)

{{< figure src="/images/pgpooladmin-install-6.jpg" >}}

nhập thông tin đăng nhập đã tạo ở trên và nhấn `Login`

- `Username`: admin
- `Password`: akitect@123 <- mật khẩu đã tạo ở trên

{{< figure src="/images/pgpooladmin-install-7.jpg" >}}

Như vậy ta đã cấu hình và đăng nhập thành công vào PGpool Admin.

### Kết luận
Trong hướng dẫn này, chúng ta đã đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux. Bằng cách thực hiện các bước này, bạn có thể tối ưu hóa và mở rộng khả năng của cơ sở dữ liệu PostgreSQL, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.