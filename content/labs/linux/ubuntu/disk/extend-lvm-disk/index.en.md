---
categories:
  - Linux
  - ubuntu
date: 2023-10-26T08:00:00+08:00
description: LVM (Logical Volume Manager) is a tool on the Linux operating system that allows more flexible management of drives and drive partitions. Instead of dividing the drive into fixed partitions, LVM allows the creation of virtual volumes (logical volumes) that can be resized, creating many smaller storage blocks to make the most of free space.
draft: false
featuredImage: /series/ubuntu-lvm.png
images:
  - /extend-lvm-disk-on-ubuntu-22-04/images/index.en.png
  - /series/ubuntu-lvm.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - extend disk
  - lvm ubuntu
  - "Extend LVM "
  - ubuntu 22.04
  - "Linux command "
  - build react native
title: Extend LVM Disk on Linux Ubuntu 22.04
url: /extend-lvm-disk-on-ubuntu-22-04
weight: 1
---

## What is LVM?

- LVM (Logical Volume Manager) is a tool on the Linux operating system that allows for more flexible management of disk drives and partitions. Instead of dividing the disk into fixed partitions, LVM allows for the creation of virtual partitions (logical volumes) that can change in size, creating smaller storage blocks to maximize unused space.
- With LVM, you can perform operations such as creating new, resizing, or moving virtual partitions flexibly, without having to change or move files on that partition. This makes storage management on the system easier and more flexible.
- LVM is widely used on server systems or large storage systems, where managing storage space is a major challenge.

## Concepts

- Physical Volumes (PVs): are physical partitions on the disk. PVs can be physical disks, partitions on the disk, or other storage devices.
- Volume Groups (VGs): are a group of PVs that have been combined together. All PVs in that VG are managed as a unified whole. With VG, users can create virtual partitions (LVs) with flexible sizes by using storage space on PVs.
- Logical Volumes (LVs): are virtual partitions created from the storage space of VG. LVs are managed like partitions on regular physical drives, can be created, deleted, resized, and formatted similar to regular partitions.

## Practice

Given the server configuration as follows:

| IP           | Hostname          | vCPU   | RAM | DISK |
| ------------ | ----------------- | ------ | --- | ---- |
| 192.168.56.2 | microk8s-master-1 | 8 core | 16G | 100G |

After successfully creating an Ubuntu server, I use the command

```
sudo su
df -h
```

{{< figure src="./images/6f66fe3b-f000-42b1-975b-96327eb4fa82.webp" >}}

Then we use the command **fdisk -l** to check the entire hard drive capacity.

{{< figure src="./images/adc80763-6526-492d-9f9c-526d1c3fe083.webp" >}}

So the total hard drive capacity is 100G, as we only received 50G, we need to extend another 50G.

## To extend LVM disk on Linux Ubuntu, please follow these steps:

### 1. Check the current status of LVM partitions on the system with the following command:

```
lvdisplay
```

This command will display a list of existing LVM partitions on your system.

{{< figure src="./images/135942a9-0278-45a7-b039-2c82dca1f9d6.webp" >}}

### 2. Check the available capacity on the physical volume (PV) with the following command:

```
pvdisplay
```

This command will display information about the available and used capacity on the PV.

{{< figure src="./images/78b7ccdb-65f6-4ecc-859d-4a60644df7e9.webp" >}}

### 3. Check the location of the LVM partition you want to extend with the following command:

```
df -h
```

This command will display a list of partitions on your system.

{{< figure src="./images/b31cd305-1b32-4be8-98e8-236703c7affe.webp" >}}

Then, select the partition with the name:

```
/dev/mapper/ubuntu--vg-ubuntu--lv
```

### 4. So the LVM partition already exists, now I just need to add more logical volume capacity with the following command

```
sudo lvm lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

{{< figure src="./images/c0d47f4b-f6d2-4ff9-b1e0-6cbacb1e5e6f.webp" >}}

Continue command

```
sudo resize2fs -p /dev/mapper/ubuntu--vg-ubuntu--lv
```

{{< figure src="./images/0326e82e-bca5-4a1c-b168-7451df7bc59f.webp" >}}

After running successfully, we use the command **df -h** to check if the hard drive has been added

```
df -h
```

{{< figure src="./images/50bd60c3-08a5-40e7-b987-e3d26925d174.webp" >}}

So we have successfully extended 50G to the LVM partition

Thank you for following my article

{{< youtube e1GJBgqs2lM >}}
