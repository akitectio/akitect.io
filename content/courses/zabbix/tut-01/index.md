---
categories:
  - devops
  - zabbix
date: 2023-03-01T08:00:00+08:00
description: Một ngày đẹp trời sau khi đi ăn sáng uống cà phê và lên công ty, sếp triệu tập một cuộc họp urgent và các câu hỏi liên quan về sự cố tối qua, mình cũng không biết nói gì, vì không có logs, không có tool monitoring system, và sau đó mình đã bắt đầu nghiên cứu và tìm ra Zabbix có thể làm được, sau một thời gian mình theo dõi, thì lỗi là do dàn SAN bị tràn bộ nhớ.
draft: false
featuredImage: /series/zabbix/lession-1-monitoring-the-server-and-notifying-when-the-server-has-problems-has-never-been-difficult-with-zabbix.webp
images:
  - /series/zabbix/lession-1-monitoring-the-server-and-notifying-when-the-server-has-problems-has-never-been-difficult-with-zabbix.webp
  - /giam-sat-may-chu-va-thong-bao-khi-may-chu-gap-su-co-chua-bao-gio-kho-voi-zabbix/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - zabbix-tutorial
tags:
  - zabbix-agent
  - Zabbix Server
  - zabbix-agent-6.2
title: Lesstion 1 - Giám sát máy chủ và thông báo khi máy chủ gặp sự cố chưa bao giờ khó với Zabbix
url: /giam-sat-may-chu-va-thong-bao-khi-may-chu-gap-su-co-chua-bao-gio-kho-voi-zabbix
weight: 1
---

Để cài đặt chúng ta cần chuẩn bị 3 con máy chủ, ở đây mình có 3 con máy chủ

| HĐH          | IP        | Hostname      | vCPU   | RAM | DISK     |
| ------------ | --------- | ------------- | ------ | --- | -------- |
| Ubuntu 22.04 | 10.19.2.1 | zabbix-server | 6 core | 12G | 100G SSD |
| Ubuntu 22.04 | 10.19.2.2 | mongodb       | 6 core | 12G | 100G SSD |
| Cent OS 7    | 10.19.2.3 | postgresql    | 6 core | 12G | 100G SSD |

Ở đay mình cài đặt zabbix server trên hệ điều hành ubuntu 22.04 bạn có thể tham khảo ở trang chủ qua đường dẫn: https://www.zabbix.com/download

Bước 1: Cài đặt zabbix 6.2

```
sudo su
apt update && apt -y upgrade
wget https://repo.zabbix.com/zabbix/6.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.2-1+ubuntu22.04_all.deb
dpkg -i zabbix-release_6.2-1+ubuntu22.04_all.deb
apt update
root@zabbix-serverr:~# apt install -y zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-sql-scripts zabbix-agent
```

Bước 2: Cài đặt và cấu hình mysql

```
sudo su
apt install -y mysql-server
systemctl start mysql
systemctl enable mysql
systemctl status mysql
```

Bước 3: Tạo database và user zabbix

```
sudo su
 mysql -uroot -p
```

Khi đăng nhập thành công thì ta chạy lệnh bên dưới để tạo database và user

```
create database zabbix_db character set utf8 collate utf8_bin;
create user zabbix_user@localhost identified by 'zabbix@123';
GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'zabbix_user'@'localhost' WITH GRANT OPTION;
grant all privileges on zabbix_db.* to zabbix_user@localhost;
SET GLOBAL log_bin_trust_function_creators = 1;
FLUSH PRIVILEGES;
\q
```

Bước 4: Import database mặt định của zabbix

```
sudo su
zcat /usr/share/doc/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uroot -p zabbix_db
```

Bước 5: Tìm và thay đổi cấu hình zabbix

Ta dùng lệnh để mở tiệp cấu hình zabbix

```
sudo su
nano /etc/zabbix/zabbix_server.conf
```

Thềm vào dòng cuối cùng và lưu lại

```
DBName=zabbix_db
DBUser=zabbix_user
DBPassword=zabbix@123
```

Bước 6: Cấu hình nginx trỏ vào IP của máy chủ ở đây ip máy chủ là 10.19.2.1

Dùng lệnh để mở tiệp cấu hình

```
sudo su
nano /etc/zabbix/nginx.conf
```

Tìm dòng và thay đổi cấu hình

```
listen          80;
server_name	10.19.2.1
```

Bước 7: Khởi đọng lại dịch vụ

```
sudo su
systemctl restart zabbix-server zabbix-agent nginx php8.1-fpm
systemctl enable zabbix-server zabbix-agent nginx php8.1-fpm
```

Sau khi khởi động lại dịch vụ thành công ta vào đường dẫn http://10.19.2.1/setup.php để thực hiện cấu hình ban đầu

{{< figure src="./images/7e7f508a-2cd5-4807-abaf-49ae0bfb57fb.png" >}}

Nhấp vào Bước tiếp theo để kiểm tra các điều kiện yêu cầu cài đặt của hệ thống

{{< figure src="./images/fc27ce52-156f-4a4f-adbf-21897df87804.png" >}}

Nhấp vào Bước tiếp theo để cấu hình kết nối database

{{< figure src="./images/d3eab79b-53f6-4e22-beea-93c86f93a834.png" >}}

Nhấp vào bước tiếp theo để chọn chủ đề hoặc bỏ qua bước này.

{{< figure src="./images/2ea5c846-7a45-4f3b-8178-5406aa22ddbf.png" >}}

Nhấp vào bước tiếp theo để cài đặt xong!

{{< figure src="./images/69ead9a3-7652-4250-bbe8-7d7d670d5417.png" >}}

Nhấp vào Kết thúc để chuyển hướng đăng nhập mặc định

username: Admin

password: zabbix

{{< figure src="./images/50835f26-bca6-4fa8-847e-3591cb0fe516.png" >}}

Kiểm tra bảng điều khiển Zabbix như bên dưới

{{< figure src="./images/14ed06e7-6b5b-4602-9641-034ab141d806.png" >}}
