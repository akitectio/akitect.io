---
categories:
  - Linux
  - ubuntu
date: 2023-10-26T08:00:00+08:00
description: LVM (Logical Volume Manager) là một công cụ trên hệ điều hành Linux cho phép quản lý ổ đĩa và phân vùng ổ đĩa một cách linh hoạt hơn. Thay vì phân chia ổ đĩa thành các phân vùng cố định, LVM cho phép tạo ra các phân vùng ảo (logical volume) có thể thay đổi kích thước, tạo ra nhiều khối lưu trữ nhỏ hơn để tận dụng tối đa không gian trống.
draft: false
featuredImage: /series/ubuntu-lvm.png
images:
  - /extend-lvm-disk-tren-ubuntu-22-04/images/index.png
  - /series/ubuntu-lvm.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - extend-disk
  - lvm-ubuntu
  - extend-lvm
  - linux-command
title: Extend LVM Disk trên Linux Ubuntu 22.04
url: /extend-lvm-disk-tren-ubuntu-22-04
weight: 1
---

{{< youtube 10Ox6s2DsZQ >}}

## LVM là gì

- LVM (Logical Volume Manager) là một công cụ trên hệ điều hành Linux cho phép quản lý ổ đĩa và phân vùng ổ đĩa một cách linh hoạt hơn. Thay vì phân chia ổ đĩa thành các phân vùng cố định, LVM cho phép tạo ra các phân vùng ảo (logical volume) có thể thay đổi kích thước, tạo ra nhiều khối lưu trữ nhỏ hơn để tận dụng tối đa không gian trống.
- Với LVM, ta có thể thực hiện các thao tác như tạo mới, thay đổi kích thước hoặc di chuyển các phân vùng ảo một cách linh hoạt, mà không cần phải thay đổi hoặc di chuyển các tập tin trên phân vùng đó. Điều này giúp cho việc quản lý lưu trữ trên hệ thống dễ dàng và linh hoạt hơn.
- LVM được sử dụng phổ biến trên các hệ thống máy chủ hoặc hệ thống lưu trữ lớn, trong đó việc quản lý không gian lưu trữ là một thách thức lớn.

## Các khái niệm

- Physical Volumes (PVs): là các phân vùng vật lý trên ổ đĩa. Các PVs có thể là các ổ đĩa vật lý, phân vùng trên ổ đĩa hoặc các thiết bị lưu trữ khác.
- Volume Groups (VGs): là một nhóm các PVs đã được kết hợp lại với nhau. Tất cả các PVs trong VG đó được quản lý như một thể thống nhất. Với VG, người dùng có thể tạo ra các phân vùng ảo (LVs) với kích thước linh hoạt bằng cách sử dụng không gian lưu trữ trên các PVs.
- Logical Volumes (LVs): là các phân vùng ảo được tạo ra từ không gian lưu trữ của VG. LVs được quản lý như các phân vùng trên các ổ đĩa vật lý thông thường, có thể tạo, xóa, sửa đổi kích thước và định dạng tương tự như phân vùng thông thường.

## Thực hành

Cho cấu hình máy chủ như sau

| IP           | Hostname          | vCPU   | RAM | DISK |
| ------------ | ----------------- | ------ | --- | ---- |
| 192.168.56.2 | microk8s-master-1 | 8 core | 16G | 100G |

sau khi tạo máy chủ ubuntu thành công thì mình dùng lệnh

```shell
sudo su
df -h
```

{{< figure src="./images/6f66fe3b-f000-42b1-975b-96327eb4fa82.webp" >}}

sau đó ta dùng lệnh **fdisk -l** để kiểm tra toàn bộ dung lượng ổ cứng

{{< figure src="./images/adc80763-6526-492d-9f9c-526d1c3fe083.webp" >}}

Như vậy tổng dung lượng ổ cứng của mình là 100G như chỉ mới nhận là 50G như vậy chúng ta cần Extend thêm 50G nữa

## Để mở rộng ổ đĩa LVM trên Linux Ubuntu, các bạn vui lòng thực hiện các bước sau:

### 1. Kiểm tra tình trạng hiện tại của các phân vùng LVM trên hệ thống bằng lệnh sau

```shell
lvdisplay
```

Lệnh này sẽ hiển thị danh sách các phân vùng LVM hiện có trên hệ thống của bạn.

{{< figure src="./images/135942a9-0278-45a7-b039-2c82dca1f9d6.webp" >}}

### 2. Kiểm tra dung lượng sẵn có trên phân vùng vật lý (PV) bằng lệnh sau

```shell
pvdisplay
```

Lệnh này sẽ hiển thị thông tin về dung lượng sẵn có và sử dụng trên PV.

{{< figure src="./images/78b7ccdb-65f6-4ecc-859d-4a60644df7e9.webp" >}}

### 3. Kiểm tra vị trí của phân vùng LVM muốn mở rộng bằng lệnh sau

```shell
df -h
```

Lệnh này sẽ hiển thị danh sách các phân vùng trên hệ thống của bạn.

{{< figure src="./images/b31cd305-1b32-4be8-98e8-236703c7affe.webp" >}}

sau đó mình chọn phân vùng name có tên là

```shell
/dev/mapper/ubuntu--vg-ubuntu--lv
```

### 4.Như vậy phân vùng LVM đã có bây giờ mình chỉ cần thêm dung lượng logical volume bằng lệnh sau

```shell
sudo lvm lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

{{< figure src="./images/c0d47f4b-f6d2-4ff9-b1e0-6cbacb1e5e6f.webp" >}}

Tiếp tục lệnh

```shell
sudo resize2fs -p /dev/mapper/ubuntu--vg-ubuntu--lv
```

{{< figure src="./images/0326e82e-bca5-4a1c-b168-7451df7bc59f.webp" >}}

sau khi chạy thành công ta dùng lệnh **df -h** để kiểm tra xem ổ cững đã được thêm vào chưa

```shell
df -h
```

{{< figure src="./images/50bd60c3-08a5-40e7-b987-e3d26925d174.webp" >}}

Như vậy ta đã Extend thêm 50G vào phân dùng LVM thành công

Cảm ơn các bạn đã theo dõi bài viết của mình

