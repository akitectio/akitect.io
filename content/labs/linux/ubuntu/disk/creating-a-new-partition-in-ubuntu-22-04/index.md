---
categories:
  - Linux
date: 2023-12-04T08:00:00+08:00
description: Bài viết này hướng dẫn cách tạo thêm một phân vùng mới trong Ubuntu 22.04. Bài viết bao gồm các bước từ việc kiểm tra phân vùng ổ cứng, xác định ổ đĩa, tạo phân vùng mới, tạo Physical Volume, tạo điểm gắn kết, kiểm tra Volume Group (VG), tạo Logical Volume (LV), tạo File System, và cuối cùng là gắn kết Logical Volume. Mỗi bước đều được hướng dẫn chi tiết kèm theo các lệnh cần thiết và hình ảnh minh họa.
draft: false
featuredImage: /series/linux/disk/ubuntu/creating-a-new-partition-in-ubuntu-22-04.png
images:
  - /series/linux/disk/ubuntu/creating-a-new-partition-in-ubuntu-22-04.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  [
    "Ubuntu",
    "Phân vùng ổ cứng",
    "Logical Volume",
    "File System",
    "Volume Group",
    "Physical Volume",
  ]
title: Tạo thêm một phân vùng mới trong Ubuntu 22.04
url: /tao-them-mot-phan-vung-moi-trong-ubuntu-22-04
weight: 2
---

## Bước 1: Kiểm tra phân vùng ổ cứng

```shell
sudo df -h
```

{{<figure src="./images/extend-new-disk-01.png" >}}

Như vậy mình có ổ cứng là `/dev/sda` và dung lượng là 20G

## Bước 2: Xác Định Ổ Đĩa

Sử dụng lệnh `lsblk` hoặc sudo `fdisk -l` để liệt kê tất cả các ổ đĩa và phân vùng. Tìm ổ đĩa hoặc phân vùng bạn muốn gắn kết. Nó sẽ có dạng như `/dev/sda1`, `/dev/sdb`, v.v.

```shell
sudo fdisk -l
```

{{<figure src="./images/extend-new-disk-02.png" >}}

Ở đây mình có ổ đĩa là `/dev/sdb` và dung lượng là 480G

## Bước 3: Tạo phân vùng mới

Sử dụng lệnh `cfdisk` để tạo phân vùng mới trên ổ đĩa `/dev/sdb`

```shell
sudo cfdisk /dev/sdb
```

chọn "New" để tạo phân vùng mới

{{<figure src="./images/extend-new-disk-03.png" >}}

Nhập số dung lượng cần tạo phân vùng mới, ở đây mình chọn `480G`

{{<figure src="./images/extend-new-disk-04.png" >}}

Chọn "Write" để lưu lại

{{<figure src="./images/extend-new-disk-05.png" >}}

Nhập `yes` để xác nhận

{{<figure src="./images/extend-new-disk-06.png" >}}

sau đó chọn "Quit" để thoát

{{<figure src="./images/extend-new-disk-07.png" >}}

Kiểm tra lại bằng lệnh `fdisk -l`

{{<figure src="./images/extend-new-disk-08.png" >}}

Như vậy đã tạo thành công phân vùng mới là `/dev/sdb1`

# Bước 4: Tạo Physical Volume

Sử dụng lệnh `pvcreate` để tạo physical volume

```shell
sudo pvcreate /dev/sdb1
```

{{<figure src="./images/extend-new-disk-09.png" >}}

Kiểm tra lại bằng lệnh `pvdisplay`

{{<figure src="./images/extend-new-disk-10.png" >}}

Như vậy đã tạo thành công physical volume là `/dev/sdb1`

# Bước 5: Tạo điểm gắn kết

Chọn thứ mục để gắn kết, ở đây mình chọn `/dev/sdb1`

```shell
mkdir /mnt/data
mkdir /mnt/data
```

{{<figure src="./images/extend-new-disk-09.png" >}}

# Bước 6: Kiểm tra Volume Group (VG)

Sử dụng lệnh `vgdisplay` để kiểm tra Volume Group

```shell
sudo vgdisplay
```

{{<figure src="./images/extend-new-disk-10.png" >}}

ở đây mình thấy có 1 Volume Group là `ubuntu-vg` có size 20G, còn sdb1 chưa được gắn kết, mình dùng lệnh kiểm tra `vgdisplay /dev/sdb1`

```shell
vgdisplay /dev/sdb1
```

{{<figure src="./images/extend-new-disk-11.png" >}}

Theo ta thấy thì `/dev/sdb1` chưa được gắn kết vào Volume Group nào cả, tiếp tục ta sẽ tạo Volume Group mới

```shell
sudo vgcreate soft-vg /dev/sdb1
```

{{<figure src="./images/extend-new-disk-12.png" >}}

Như vậy đã tạo thành công Volume Group mới là `soft-vg`, dùng lệnh `vgdisplay` để kiểm tra lại

```shell
sudo vgdisplay
```

{{<figure src="./images/extend-new-disk-13.png" >}}

Như vậy đã tạo thành công Volume Group mới là `soft-vg` và đã gắn kết vào `/dev/sdb1`

# Bước 7: Tạo Logical Volume (LV)

Sử dụng lệnh `lvcreate` để tạo Logical Volume

```shell
sudo lvcreate -n soft-lv -l 100%FREE soft-vg
```

{{<figure src="./images/extend-new-disk-14.png" >}}

Nhu vậy đã tạo thành công Logical Volume mới là `soft-lv` và đã gắn kết vào Volume Group `soft-vg`, kiểm tra lại bằng lệnh `lvdisplay`

```shell
sudo lvdisplay
```

{{<figure src="./images/extend-new-disk-15.png" >}}

# Bước 8: Tạo File System

Sử dụng lệnh `mkfs.ext4` để tạo file system

```shell
sudo mkfs.ext4 /dev/soft-vg/soft-lv
```

{{<figure src="./images/extend-new-disk-16.png" >}}

# Bước 9: Gắn kết Logical Volume

Sử dụng lệnh `mount` để gắn kết Logical Volume

```shell
sudo mount /dev/soft-vg/soft-lv /mnt/data
```

{{<figure src="./images/extend-new-disk-17.png" >}}

Kiểm tra lại bằng lệnh `df -h`

```shell
df -h
```

{{<figure src="./images/extend-new-disk-18.png" >}}

Như vậy đã gắn kết thành công Logical Volume `soft-lv` vào `/mnt/data`

# Bước 10: Tự động gắn kết Logical Volume khi khởi động

Sử dụng lệnh `blkid` để lấy UUID của Logical Volume

```shell
sudo blkid
```

{{<figure src="./images/extend-new-disk-19.png" >}}

Sau đó thêm vào file `/etc/fstab`

```shell
sudo nano /etc/fstab
```

Thêm vào dòng sau

```shell
UUID=e32fee80-371a-4c92-899a-c004d1d79060 /mnt/data ext4 defaults 0 2
```

{{<figure src="./images/extend-new-disk-20.png" >}}

Như vậy đã cấu hình thành công, khởi động lại máy bằng lệnh `sudo reboot` và kiểm tra lại bằng lệnh `df -h`

```shell
df -h
```

{{<figure src="./images/extend-new-disk-21.png" >}}
