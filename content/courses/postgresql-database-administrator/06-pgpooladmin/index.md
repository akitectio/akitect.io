---
categories:
  - database
date: 2024-02-25T08:00:00+08:00
draft: true
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images:
  - /labs/postgresql/postgresql-pgpool.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - postgreSQL
  - ubuntu
  - pgpool
title: Lesson 6 - Pgpool Admin Ubuntu 
url: /cai-dat-va-cau-hinh-pgpool-admin-tren-ubuntu
description: Pgpool Admin là một công cụ quản lý cấu hình và giám sát PGpool-II, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu PostgreSQL. Trong hướng dẫn này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool Admin trên hệ điều hành Ubuntu Linux, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu của mình.
weight: 6
---

## Pgpool Admin là gì

Pgpool Admin là một công cụ quản lý cấu hình và giám sát PGpool-II, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu PostgreSQL. Trong hướng dẫn này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool Admin trên hệ điều hành Ubuntu Linux, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu của mình.


### Cài đặt PGpool Admin trên Ubuntu

#### Bước 1: Cài đặt apache2 và php

Thêm repository php

```bash
sudo apt-add-repository ppa:ondrej/php
sudo apt update
```

Đầu tiên, cài đặt Apache2 và PHP trên máy chủ `pgpool2` bằng cách thực hiện các bước sau:

```bash
sudo apt update
sudo apt install -y apache2 php sudo apt install php7.4 libapache2-mod-php7.4 php7.4-cli php7.4-common php7.4-pgsql 
```

Sau khi cài đặt xong, kiểm tra phiên bản PHP:

```bash
php -v
```

{{< figure src="./images/php-version.jpg" >}}

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

{{< figure src="./images/pgpooladmin-directory.jpg" >}}

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

{{< figure src="./images/pgpooladmin-apache2.jpg" >}}

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

### Bước 5: Truy cập PGpool Admin và cấu hình ban đầu

Sau khi cài đặt xong, truy cập PGpool Admin bằng cách mở trình duyệt web và truy cập địa chỉ IP của máy chủ `pgpool2` : [http://192.168.56.5/pgpooladmin/install](http://192.168.56.5/pgpooladmin/install)

{{< figure src="./images/pgpooladmin-install.jpg" >}}

chọn ngôn ngữ và nhấn `Next`

{{< figure src="./images/pgpooladmin-install-2.jpg" >}}

Tiếp tục thêm thông tin cấu hình kết nối đến pgpool2

{{< figure src="./images/pgpooladmin-install-3.jpg" >}}




### Kết luận
Trong hướng dẫn này, chúng ta đã đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux. Bằng cách thực hiện các bước này, bạn có thể tối ưu hóa và mở rộng khả năng của cơ sở dữ liệu PostgreSQL, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.