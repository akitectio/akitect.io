---
categories:
  - devops
  - zabbix
date: 2023-03-02T08:00:00+08:00
description: Ở bài này mình sẽ hướng dẩn  các bạn cài đặt Zabbix Agent 2 trên CentOS 7 để theo dỗi máy chủ Mongodb
draft: false
featuredImage: /series/zabbix/zabbix-62-canh-bao-qua-telegram.webp
images:
  - /series/zabbix/zabbix-62-canh-bao-qua-telegram.webp
  - /zabbix-agent-2-tren-may-chu-centos-7-dung-de-theo-doi-mongodb-replica-set/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - zabbix-tutorial
tags:
  - zabbix-agent
  - zabbix-server
  - zabbix-agent-6.2
title: Lesstion 2 - Zabbix Agent 2 trên máy chủ CentOS 7 dùng để theo dõi Mongodb Replica Set
url: /zabbix-agent-2-tren-may-chu-centos-7-dung-de-theo-doi-mongodb-replica-set
weight: 2
---

Bước 1: Cài Zabbix Agent 2

- Cài đặt kho lưu trữ Zabbix

```shell
yum update -y
 rpm -Uvh https://repo.zabbix.com/zabbix/6.2/rhel/7/x86_64/zabbix-release-6.2-3.el7.noarch.rpm
yum clean all
```

- Cài đặt Zabbix Agent2

```shell
yum install zabbix-agent2 zabbix-agent2-plugin-* -y
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
Hostname=Zabbix Mongodb
```

lưu lại và khởi động lại dịch vụ

```shell
systemctl restart zabbix-agent2
```

Bước 2: đăng nhập vào zabbix và add vào hosts : http://10.19.2.1/zabbix/zabbix.php?action=host.view

Chọn theo như hình

{{< figure src="/images/c1706075-2222-4358-b3bf-9b97e719b869.webp" >}}

Bước 3: Cấu hình template **MongoDB node by Zabbix agent 2**

1. Tạo người dùng MongoDB để theo dõi

Khi tác nhân đã được triển khai và cấu hình, bạn cần đảm bảo rằng bạn có người dùng cơ sở dữ liệu MongoDB mà chúng tôi có thể sử dụng cho mục đích giám sát. Dưới đây bạn có thể tìm thấy một ví dụ ngắn gọn về cách bạn có thể tạo người dùng MongoDB:

Truy cập vào mongodb ta dùng lệnh

```shell
mongosh
```

Chuyển sang cơ sở dữ liệu quản trị MongoDB:

```shell
use admin
```

Tạo người dùng với quyền ‘userAdMinanyDatabase‘ :

```shell
db.createUser(
... {
..... user: "zabbix_mon",
..... pwd: "zabbix_mon",
..... roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
..... }
... )
```

Tên người dùng cho người dùng mới được tạo là ‘zabbix_mon'
Mật khẩu cũng là ‘zabbix_mon

2. Cấu hình Macros theo hình bên dưới

{{< figure src="/images/2040661e-6603-4394-8e2e-d941b3104a3d.webp" >}}

- **{$MONGODB.PASSWORD}** –  tên người dùng MongoDB. Ví dụ của chúng tôi, chúng tôi sẽ đặt cái này thành zabbix_mon
- **{$MONGODB.USER}** – mật khẩu MongoDB. Ví dụ của chúng tôi, chúng tôi sẽ đặt giá trị này thành zabbix_mon
- **{$MONGODB.CONNSTRING}** – chuỗi kết nối MongoDB. Chỉ định địa chỉ MongoDB và cổng tại đây mà Zabbix Agent 2 sẽ kết nối và thực hiện việc thu thập số liệu

Bạn có thể thảm khảo thêm 2 bài viêt :

- https://blog.zabbix.com/monitoring-mongodb-nodes-and-clusters-with-zabbix/16031/
- https://www.zabbix.com/integrations/mongodb#mongodb
