---
categories:
  - devops
  - zabbix
date: 2023-03-02T08:00:00+08:00
description: Ở bài này mình sẽ hướng dẩn  các bạn cài đặt Zabbix Agent 2 trên  Ubuntu 22.04 để theo dỗi máy chủ PostgreSQL
draft: false
featuredImage: /series/zabbix/zabbix-agent-2-on-centos-7-server-used-to-monitor-mongodb-replica-set.webp
images:
  - /series/zabbix/zabbix-agent-2-on-centos-7-server-used-to-monitor-mongodb-replica-set.webp
  - /zabbix-agent-2-tren-ubuntu-2204-theo-doi-may-chu-postgresql/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - zabbix-tutorial
tags:
  - zabbix-agent
  - zabbix-server
  - zabbix-agent-6.2
title: Lesstion 3 - Zabbix Agent 2 trên Ubuntu 22.04 theo dõi máy chủ PostgreSQL
url: /zabbix-agent-2-tren-ubuntu-2204-theo-doi-may-chu-postgresql
weight: 3
---

Bước 1: Cài Zabbix Agent 2

- Cài đặt kho lưu trữ Zabbix

```shell
wget https://repo.zabbix.com/zabbix/6.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.2-4%2Bubuntu22.04_all.deb
dpkg -i zabbix-release_6.2-4+ubuntu22.04_all.deb
apt update
```

- Cài đặt Zabbix Agent2

```shell
 apt install zabbix-agent2 zabbix-agent2-plugin-*
```

- Bắt đầu quy trình Zabbix Agent2

```shell
systemctl restart zabbix-agent2
systemctl enable zabbix-agent2
```

Bước 2: Cấu hình Zabbix Agent 2 trỏ vào Zabbix Server

Như ở bài cài đặt zabbix server ở địa chỉ 10.19.2.1

Mình dùng lệnh để mở tiệp cấu hình zabbix

```shell
nano /etc/zabbix/zabbix_agent2.conf
```

trong tiệp tìm và đổi lại những chổ cấu hình

```shell
ListenIP=0.0.0.0
Server=10.19.2.1
Hostname=Zabbix PostgreSQL
```

lưu lại và khởi động lại dịch vụ

```shell
 systemctl restart zabbix-agent2
```

Bước 2: đăng nhập vào zabbix và add vào hosts : http://10.19.2.1/zabbix/zabbix.php?action=host.view

Chọn theo như hình

{{< figure src="/images/3d973b40-9232-4ba5-bd28-36ab5bb27a34.png" >}}

Bước 3: Cấu hình template **PostgreSQL by Zabbix agent 2**

1. Tạo người dùng PostgreSQL để theo dõi (password ở đây mình đặt là **Password@123**):

```shell
CREATE USER zbx_monitor WITH PASSWORD 'Password@123' INHERIT;
GRANT EXECUTE ON FUNCTION pg_catalog.pg_ls_dir(text) TO zbx_monitor;
GRANT EXECUTE ON FUNCTION pg_catalog.pg_stat_file(text) TO zbx_monitor;
GRANT EXECUTE ON FUNCTION pg_catalog.pg_ls_waldir() TO zbx_monitor;
```

2. Chỉnh sửa pg_hba.conf để cho phép kết nối từ Zabbix:

```shell
# TYPE  DATABASE        USER            ADDRESS                 METHOD
  host       all        zbx_monitor     localhost               md5
```

Để biết thêm thông tin, vui lòng đọc tài liệu PostgreSQL https://www.postgresql.org/docs/civerse/auth-pg-hba-conf.html

3. Đặt trong macro **{$PG.URI}** tên nguồn dữ liệu hệ thống của phiên bản PostgreSQL, chẳng hạn như **<protocol(host:port)>**
4. Đặt tên người dùng và mật khẩu trong macro máy chủ **({$PG.USER}** và **{$PG.PASSWORD}**) nếu bạn muốn ghi đè các tham số từ tệp cấu hình tác nhân Zabbix

{{< figure src="/images/003c2e76-a17a-4a8c-ab41-3c544e8a3417.png" >}}

Link tham khảo : https://www.zabbix.com/integrations/postgresql#postgresql_agent2
