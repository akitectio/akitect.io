---
categories:
  - linux
  - network
date: 2024-02-17T10:23:30-09:00
description: Open VPN là một phần mềm mã nguồn mở giúp tạo ra một mạng riêng ảo (VPN) giữa các máy tính. OpenVPN có thể chạy trên nhiều hệ điều hành khác nhau, bao gồm cả Linux, Windows, Mac OS X, và các thiết bị di động.
draft: false
featuredImage: /labs/open-vpn/featured-image.webp
images:
  - /labs/open-vpn/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - vpn
title: Cài đặt Open VPN Server trên ubuntu 22.04
weight: 1
---

> Để kết nối từ mạng ngoài trong các máy chủ trong mạng LAN của các máy chủ trong mạng, chúng ta cần cấu hình VPN Server để truy cập, ở bài này mình sẽ sử dụng Open VPN Server

## Khái niệm
* OpenVPN là SSL VPN đầy đủ tính năng (mạng riêng ảo). Nó thực hiện các tiện ích mở rộng mạng ASI Lớp 2 hoặc 3 bằng giao thức SSL/TLS. Nó là phần mềm nguồn mở và được phân phối theo GNU GPL. VPN cho phép bạn kết nối an toàn với mạng công cộng không an toàn như mạng WiFi tại sân bay hoặc khách sạn. VPN cũng được yêu cầu truy cập tài nguyên công ty, doanh nghiệp hoặc máy chủ gia đình của bạn. Bạn có thể bỏ qua trang web bị chặn địa lý và tăng quyền riêng tư hoặc an toàn trực tuyến. Hướng dẫn này cung cấp các hướng dẫn từng bước để định cấu hình máy chủ OpenVPN trên máy chủ Ubuntu Linux 22.04.

## Hướng dẫn

Để thực hiện cài đặt ta thực hiện các bước sau:

### Bước 1: Cập nhật hệ thống, hệ điều hành của bạn, ở đây mình sử dụng Ubuntu 22.04

Ta thực hiện update các gói bằng lệnh apt

```shell
sudo apt update && sudo apt upgrade -y
```
{{<figure src="./images/7f32aed5-2f3a-4396-8b09-3da3f714073f.webp" >}}

### Bước 2: Tìm và ghi nhận lại ip của bạn 

Để tìm ra ip của bạn ta sử dụng lệnh: 

```html
ip a
ip a show enp4s0
```

{{<figure src="./images/e0f12e13-e3ca-4aae-9333-0720b8e56d2c.webp" >}}


### Bước 3: Tải và chạy file  ubuntu-22.04-lts-vpn-server.sh script

```shell
wget https://raw.githubusercontent.com/Nyr/openvpn-install/master/openvpn-install.sh -O ubuntu-22.04-lts-vpn-server.sh
```

{{<figure src="./images/78e79b5b-c865-4532-8f96-8148504edba2.webp" >}}

sau đó ta set quyền cho tiệp mới tải về bằng lệnh 

```shell
chmod -v +x ubuntu-22.04-lts-vpn-server.sh
```

Chạy **ubuntu-22.04-lts-vpn-server.sh** để cài đặt  OpenVPN Server

```shell
sudo ./ubuntu-22.04-lts-vpn-server.sh
```

{{<figure src="./images/759f014d-721a-4a67-a41f-b0fce57005b3.webp" >}}
{{<figure src="./images/5a573a80-35f2-411c-bbf8-fb1837822fa7.webp" >}}


sau đó ta kiểm tra trạng thái của openVPN

```shell
sudo systemctl status openvpn-server@server
```
{{<figure src="./images/6f81e43a-4a69-41a8-b41c-6f4355e8e4f7.webp" >}}

Sau đó ta tải tiệp **client.ovpn** về máy cần vô VPN 

Chúng ta tìm tiệp .ovpn bằng lệnh 

```sql
sudo find / -iname "*.ovpn" -ls
```

Như vậy ta đã cài đặt thành cong OpenVPN

Tiếp theo ta cài đặt Open VPN Client cho máy cần vào VPN

1. [Apple IOS client ](https://apps.apple.com/us/app/openvpn-connect/id590379981)
2. [ Android client ](https://play.google.com/store/apps/details?id=net.openvpn.openvpn&hl=en)
3. [Apple MacOS client](https://openvpn.net/client-connect-vpn-for-mac-os/)

Ở đây mình sử dụng Mac M1 và kết nối thành công

{{<figure src="./images/ca0bfdb6-495f-4cb6-a333-2dbef13d098e.webp" >}}

### Link tham khảo
* https://www.cyberciti.biz/faq/ubuntu-22-04-lts-set-up-openvpn-server-in-5-minutes/