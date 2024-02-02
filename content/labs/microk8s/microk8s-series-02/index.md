---
categories:
  - microk8s
date: 2023-01-05T08:00:00+08:00
description: Ở bài trước mình có hướng dẫn các bạn cài đặt Oracle VM VirtualBox 7 trên ubuntu 22.04, bài này mình sẽ hướng dẫn các bạn tạo các VM Ubuntu để thực hành series này
draft: false
featuredImage: /series/microk8s/lesson-2-create-an-ubuntu-2204-virtual-machine-using-oracle-vm-virtualbox-7.webp
images:
  - /series/microk8s/lesson-2-create-an-ubuntu-2204-virtual-machine-using-oracle-vm-virtualbox-7.webp
  - /lesson-2-create-an-ubuntu-2204-virtual-machine-using-oracle-vm-virtualbox-7/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - kubernetes
  - k8s
  - ubuntu
  - virtualbox
  - virtualbox 7
  - virtualbox 7 on ubuntu 22.04
title: Bài 2 - Tạo máy ảo ubuntu 22.04 bằng Oracle VM VirtualBox 7
url: /bai-2-tao-may-ao-ubuntu-2204-bang-oracle-vm-virtualbox-7
weight: 2
---

Các thuật ngữ sử dụng trong bài viết

| Viết tắt | Ý Nghĩa         |
| -------- | --------------- |
| VM       | Virtual Machine |

Ở bài này mình sẽ tạo theo cấu hình VM như sau

| IP           | Hostname          | vCPU   | RAM | DISK |
| ------------ | ----------------- | ------ | --- | ---- |
| 192.168.56.2 | microk8s-master-1 | 1 core | 2G  | 50G  |
| 192.168.56.3 | microk8s-master-2 | 1 core | 2G  | 50G  |
| 192.168.56.4 | microk8s-master-3 | 1 core | 2G  | 50G  |
| 192.168.56.5 | microk8s-worker-1 | 1 core | 2G  | 50G  |
| 192.168.56.6 | microk8s-worker-2 | 1 core | 2G  | 50G  |
| 192.168.56.7 | microk8s-worker-3 | 1 core | 2G  | 50G  |
| 192.168.56.8 | microk8s-worker-4 | 1 core | 2G  | 50G  |

### Bước 1: Tải tiệp cài đặt ubuntu trên trang chủ

- Link tải https://ubuntu.com/download/server

{{< figure src="./fbc3244b-88d1-43fe-bcc8-78c82d5c0515.webp" >}}

{{< figure src="./0652f90d-fd93-4eeb-a04e-3651dc88d8e5.webp" >}}

### Bước 2: Bắt đầu tạo máy ảo Ubuntu 22.04

Nhấp vào tạo mới VM với các thông tin sau

Name: microk8s-master-01 (Tên của VM)
Folder: Thư mục lưu trữ
IOS Image: Đường dẫn đến file đã tạo ở bước 1

{{< figure src="./92e7c5ee-b3ea-4ee0-850d-a744dc904b5e.webp" >}}

Nhập đầy đủ thông tin và bấm next

{{< figure src="./84d85194-2fbd-443e-96f5-e3efffbf5295.webp" >}}

Tiếp tục bấm next

{{< figure src="./d975db34-61a4-4985-825d-9302e000a8ab.webp" >}}

Do là môi trường test nên mình chỉ để 1 core và 2g ram, Tiếp tục bấm next

{{< figure src="./4e3a058e-fb51-46d7-b661-c20dfbddbe18.webp" >}}

Phần này mình sẽ để 50G ổ cứng

{{< figure src="./99793764-81a2-4268-8a3d-06f0b045fc17.webp" >}}

Tiếp tục bấm next

{{< figure src="./b3b40851-534a-42e7-9853-cedaf5669204.webp" >}}

Màn hình xem lại thì mình bấm "Finish" để tạo thành công cấu hình VM

### Bước 3: Start và cấu hình ban đầu cho máy ảo

{{< figure src="./baf58fde-6d2f-44d3-9eec-626b61f7a419.webp" >}}

{{< figure src="./aacb5261-62d2-4d27-93d0-aa53c00b5318.webp" >}}

{{< figure src="./0475c12f-d0f5-4555-916e-7a25aec814f3.webp" >}}

{{< figure src="./5f083762-20ab-4bd0-8fff-6f29860e428c.webp" >}}

{{< figure src="./b53db97b-a2fa-4f74-a6e3-5327a611de4b.webp" >}}

{{< figure src="./db9b38d7-295b-41d4-b359-74cfb3cc6f0f.webp" >}}

{{< figure src="./954f7809-597c-4e08-8b1f-b2d8f3c7158c.webp" >}}

{{< figure src="./6688afff-c902-406b-b9ec-add469fe0bd4.webp" >}}

Đợi cập nhật sau đó bấm reboot

{{< figure src="./9a52d76e-bae4-4f42-b59f-901f4be06e65.webp" >}}

### Bước 4: Cấu hình IP

Mình cấu hình thêm 1 đường mạng nội bộ trên network của virtualbox

Vào phần tools chọn create host-only networks

{{< figure src="./9a52d76e-bae4-4f42-b59f-901f4be06e65.webp" >}}

Ở đây mình tạo 1 lớp mạng C :

IP: 192.168.56.x

Gateway: 192.168.56.1

Subnet max: 255.255.255.0

Vào phần setting của microk8s-master-01 chọn Adapter 2 và chọn theo hình

{{< figure src="./8fbf1a50-73b1-4d65-a2e6-418bb55988a9.webp" >}}

sau đó vào giao diện ssh của vm ta làm theo từng bước sau:

1. Kiểm tra tên card mạng vừa add vào

```
ls /sys/class/net/
```

{{< figure src="./31f0f083-d301-4631-8b81-038c1b2e2345.webp" >}}

Ở đây tên của card mạng mới thêm vào là **enp0s8**

2. Tiến hình cấu hình static ip theo bản mô tả đầu bài viết, ở đây là host microk8s-master-01 với ip là 192.168.56.1

Ta sử dụng lệnh ở máy VM để set ip

```
sudo nano /etc/netplan/00-installer-config.yaml
```

{{< figure src="./c0664f29-8cbe-4e20-a026-c1911bd6dd0d.webp" >}}

sau đó thêm vào File theo như hình:

{{< figure src="./0474bfbd-8efa-49cf-a602-3ce2e6d7a3d3.webp" >}}

```
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      dhcp4: no
      addresses: [192.168.56.2/24]
  version: 2
```

sau đó sử dụng lệnh `sudo netplan apply` để apply cấu hình IP

sau đó mình sử dụng lệnh `ip addr` để kiểm tra cấu hình ip đã được apply chưa

{{< figure src="./81cccf0d-3681-4959-9a25-a040b5392f10.webp" >}}

Như hình trên thì mình thấy ip **192.168.56.2** đã ăn rồi

Tới đây thì mình cũng đã thiết lập thành công static ip cho VM.

### Bước 5: Clone VM microk8s-master-01 và cấu hình ip, hostname cho 6 VM còn lại

{{< youtube ntnTN4BSRPc >}}

Cảm ơn các bạn đã theo dõi bài viết <3
