---
categories:
  - linux
date: 2023-12-06T12:18:39+07:00
description: Cài đặt và cấu hình MAAS (Metal as a Service) trên Ubuntu 22.04 LTS cung cấp một giải pháp quản lý phần cứng vật lý hiệu quả, với khả năng tự động hóa và dễ dàng mở rộng. Quá trình này giúp tối ưu hóa cấu hình máy chủ và quản lý tài nguyên, hỗ trợ môi trường đám mây và trung tâm dữ liệu.
draft: false
featuredImage: /series/mass-101/installation-and-configuration-of-maas-metal-as-a-service-on-ubuntu-22-04-lts.webp
images:
  - /series/mass-101/installation-and-configuration-of-maas-metal-as-a-service-on-ubuntu-22-04-lts.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - maas-101
tags: [
 'maas',
 'ubuntu-22.04-lts',
 'server-management',
 'cloud-infrastructure',
 'data-center-automation',
 'bare-metal-provisioning'
]
title: Cài đặt và cấu hình MAAS (Metal as a Service) trên Ubuntu 22.04 LTS
url: /cai-dat-va-cau-hinh-maas-metal-as-a-service-tren-ubuntu-22-04-lts
weight: 2
---

## Bước 1: Cài đặt postgresql 14, tạo database và user cho MAAS

Bạn tham khảo ở bài viết này: [Cài đặt và cấu hình PostgreSQL 14 trên Ubuntu 22.04 LTS](/cai-dat-va-bao-mat-postgresql-14-tren-ubuntu-2404)

## Bước 3: Cài đặt MAAS

```bash
sudo snap install --channel=3.4 maas
```

{{<figure src="./images/maas-101-02-01.png" >}}

{{< admonition warning "Một số lưu ý khi cài đặt" >}}
Khi cài đặt MAAS trên Ubuntu, có thể có xung đột giữa máy khách NTP hiện tại, `systemd-timesyncd `và máy khách/máy chủ NTP do MAAS cung cấp, chrony. Điều này có thể dẫn đến sự cố đồng bộ hóa thời gian, đặc biệt nếu MAAS được định cấu hình với các máy chủ NTP ngược tuyến khác với các máy chủ được `systemd-timesyncd` sử dụng. Để tránh xung đột, người dùng có thể vô hiệu hóa và dừng `systemd-timesyncd` theo cách thủ công bằng lệnh sau:

```bash
sudo systemctl stop systemd-timesyncd
sudo systemctl disable systemd-timesyncd
```

{{< figure src="./images/maas-101-02-02.png" >}}

{{< /admonition >}}

## Bước 4: Khởi tạo MAAS

```bash

$MAAS_DBUSER = "myuser"
$MAAS_DBPASS = "mypassword"
$MAAS_DBNAME = "mydatabase"
$HOSTNAME = "localhost"

sudo maas init --mode all --database-uri "postgres://$MAAS_DBUSER:$MAAS_DBPASS@$HOSTNAME/$MAAS_DBNAME"
```

{{< figure src="./images/maas-101-02-03.png" >}}

{{< admonition warning "Một số lưu ý khi khởi tạo MAAS" >}}

- `--mode all` sẽ khởi tạo MAAS với tất cả các dịch vụ, bao gồm cả DHCP và DNS.
- `--database-uri` sẽ khai báo đường dẫn đến database của MAAS. Đường dẫn này có dạng `postgres://<username>:<password>@<hostname>/<database_name>`
  {{< /admonition >}}

Dùng lệnh kiểm tra trạng thái của MAAS

```bash
sudo maas status
```

{{< figure src="./images/maas-101-02-04.png" >}}

## Bước 6: Tạo tài khoản người dùng cho MAAS

```bash
sudo maas createadmin --username admin --password admin --email akitect.io@gmail.com
```

{{< admonition warning "Một số lưu ý khi tạo tài khoản người dùng cho MAAS" >}}

- `--username` sẽ khai báo tên người dùng cho MAAS
- `--password` sẽ khai báo mật khẩu cho MAAS
- `--email` sẽ khai báo email cho MAAS
  {{< /admonition >}}

{{< figure src="./images/maas-101-02-05.png" >}}

## Bước 7: Đăng nhập vào MAAS

Truy cập vào địa chỉ `http://10.86.140.147/MAAS` để đăng nhập vào MAAS

{{< admonition warning "Lưu ý" >}}

Để đăng nhập vào MAAS, bạn cần phải sử dụng tài khoản người dùng mà bạn đã tạo ở bước 6
{{< /admonition >}}

Màn hình wellcome của MAAS

{{< figure src="./images/maas-101-02-06.png" >}}

{{< admonition info "Các trường trên màn hình wellcome của MAAS" >}}

- `Region name`: Tên của MAAS server (mặc định là `maas`)
- DNS Forwarding: Địa chỉ IP của DNS server ngoài mà MAAS sẽ sử dụng để truy vấn tên miền
- `Ubuntu archive`: Địa chỉ IP của Ubuntu archive mirror mà MAAS sẽ sử dụng để cài đặt các gói phần mềm

- `Ubuntu extra archvie`: Địa chỉ IP của Ubuntu extra archive mirror mà MAAS sẽ sử dụng để cài đặt các gói phần mềm

- `APT & HTTP proxy`: Địa chỉ IP của proxy server mà MAAS sẽ sử dụng để truy cập vào các Ubuntu archive mirror và Ubuntu extra archive mirror

{{< /admonition >}}

## Bước 8: Chọn image Ubuntu để cài đặt

{{< figure src="./images/maas-101-02-07.png" >}}

{{< admonition info "Các trường trên màn hình chọn image Ubuntu để cài đặt" >}}

- `Ubuntu release`: Phiên bản Ubuntu mà bạn muốn cài đặt
- `Architecture`: Kiến trúc của máy chủ mà bạn muốn cài đặt
- `Sub-architecture`: Phân kiến trúc của máy chủ mà bạn muốn cài đặt
- `Release`: Phiên bản của Ubuntu mà bạn muốn cài đặt
- `Image`: Image của Ubuntu mà bạn muốn cài đặt
- `Sync now`: Đồng bộ image của Ubuntu mà bạn muốn cài đặt
- `Download`: Tải image của Ubuntu mà bạn muốn cài đặt
- `Delete`: Xóa image của Ubuntu mà bạn muốn cài đặt
- `Edit`: Chỉnh sửa image của Ubuntu mà bạn muốn cài đặt
- `Add`: Thêm image của Ubuntu mà bạn muốn cài đặt
- `Save`: Lưu lại image của Ubuntu mà bạn muốn cài đặt
- `Cancel`: Hủy bỏ image của Ubuntu mà bạn muốn cài đặt
- `Update selection`: Cập nhật image của Ubuntu mà bạn muốn cài đặt
  {{< /admonition >}}

## Bước 9: Hoài thành cài đặt MAAS

Chọn `Finish Setup` để hoàn thành cài đặt MAAS
{{< figure src="./images/maas-101-02-08.png" >}}

Như vậy là bạn đã cài đặt và cấu hình MAAS thành công trên Ubuntu 22.04 LTS.
