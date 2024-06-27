---
categories:
  - Linux
date: 2023-12-04T08:00:00+08:00
description: This post guides you on how to create a new partition in Ubuntu 22.04. The post includes steps from checking the hard disk partition, identifying the disk, creating a new partition, creating a Physical Volume, creating a mount point, checking the Volume Group (VG), creating a Logical Volume (LV), creating a File System, and finally mounting the Logical Volume. Each step is detailed with necessary commands and illustrative images.
draft: false
featuredImage: /series/linux/disk/ubuntu/creating-a-new-partition-in-ubuntu-22-04.png
images:
  - /series/linux/disk/ubuntu/creating-a-new-partition-in-ubuntu-22-04.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  [
    "Ubuntu",
    "hard-disk-partitioning",
    "logical-volume",
    "file-system",
    "volume-group",
    "physical-volume",
  ]
title: Creating a new partition in Ubuntu 22.04
url: /creating-a-new-partition-in-ubuntu-22-04
weight: 2
---

{{< youtube 10Ox6s2DsZQ >}}

## Step 1: Check the hard disk partition

```shell
sudo df -h
```

{{<figure src="/images/extend-new-disk-01.png" >}}

So, I have a hard drive named `/dev/sda` with a capacity of 20G

## Step 2: Identify the Disk

Use the `lsblk` command or `sudo fdisk -l` to list all disks and partitions. Find the disk or partition you want to mount. It will look like `/dev/sda1`, `/dev/sdb`, etc.

```shell
sudo fdisk -l
```

{{<figure src="/images/extend-new-disk-02.png" >}}

Here, I have a disk named `/dev/sdb` with a capacity of 480G

## Step 3: Create a new partition

Use the `cfdisk` command to create a new partition on the disk `/dev/sdb`

```shell
sudo cfdisk /dev/sdb
```

Choose "New" to create a new partition

{{<figure src="/images/extend-new-disk-03.png" >}}

Enter the amount of capacity needed to create a new partition, here I choose `480G`

{{<figure src="/images/extend-new-disk-04.png" >}}

Choose "Write" to save

{{<figure src="/images/extend-new-disk-05.png" >}}

Enter `yes` to confirm

{{<figure src="/images/extend-new-disk-06.png" >}}

Then choose "Quit" to exit

{{<figure src="/images/extend-new-disk-07.png" >}}

Check again with the `fdisk -l` command

{{<figure src="/images/extend-new-disk-08.png" >}}

So, I have successfully created a new partition named `/dev/sdb1`

# Step 4: Create a Physical Volume

Use the `pvcreate` command to create a physical volume

```shell
sudo pvcreate /dev/sdb1
```

{{<figure src="/images/extend-new-disk-09.png" >}}

Check again with the `pvdisplay` command

{{<figure src="/images/extend-new-disk-10.png" >}}

So, I have successfully created a physical volume named `/dev/sdb1`

# Step 5: Create a mount point

Choose a directory to mount, here I choose `/dev/sdb1`

```shell
mkdir /mnt/data
mkdir /mnt/data/soft
```

{{<figure src="/images/extend-new-disk-09.png" >}}

# Step 6: Check the Volume Group (VG)

Use the `vgdisplay` command to check the Volume Group

```shell
sudo vgdisplay
```

{{<figure src="/images/extend-new-disk-10.png" >}}

Here, I see there is a Volume Group named `ubuntu-vg` with a size of 20G, and sdb1 has not been mounted yet, I use the `vgdisplay /dev/sdb1` command to check

```shell
vgdisplay /dev/sdb1
```

{{<figure src="/images/extend-new-disk-11.png" >}}

As we can see, `/dev/sdb1` has not been mounted to any Volume Group yet, next we will create a new Volume Group

```shell
sudo vgcreate soft-vg /dev/sdb1
```

{{<figure src="/images/extend-new-disk-12.png" >}}

So, I have successfully created a new Volume Group named `soft-vg`, use the `vgdisplay` command to check again

```shell
sudo vgdisplay
```

{{<figure src="/images/extend-new-disk-13.png" >}}

So, I have successfully created a new Volume Group named `soft-vg` and have mounted it to `/dev/sdb1`

# Step 4: Create a Physical Volume

Use the `pvcreate` command to create a physical volume

```shell
sudo pvcreate /dev/sdb1
```

{{<figure src="/images/extend-new-disk-09.png" >}}

Check again with the `pvdisplay` command

{{<figure src="/images/extend-new-disk-10.png" >}}

So, I have successfully created a physical volume named `/dev/sdb1`

# Step 5: Create a mount point

Choose a directory to mount, here I choose `/dev/sdb1`

```shell
mkdir /mnt/data
mkdir /mnt/data/soft
```

{{<figure src="/images/extend-new-disk-09.png" >}}

# Step 6: Check the Volume Group (VG)

Use the `vgdisplay` command to check the Volume Group

```shell
sudo vgdisplay
```

{{<figure src="/images/extend-new-disk-10.png" >}}

Here, I see there is a Volume Group named `ubuntu-vg` with a size of 20G, and sdb1 has not been mounted yet, I use the `vgdisplay /dev/sdb1` command to check

```shell
vgdisplay /dev/sdb1
```

{{<figure src="/images/extend-new-disk-11.png" >}}

As we can see, `/dev/sdb1` has not been mounted to any Volume Group yet, next we will create a new Volume Group

```shell
sudo vgcreate soft-vg /dev/sdb1
```

{{<figure src="/images/extend-new-disk-12.png" >}}

So, I have successfully created a new Volume Group named `soft-vg`, use the `vgdisplay` command to check again

```shell
sudo vgdisplay
```

{{<figure src="/images/extend-new-disk-13.png" >}}

So, I have successfully created a new Volume Group named `soft-vg` and have mounted it to `/dev/sdb1`

# Step 7: Create a Logical Volume (LV)

Use the `lvcreate` command to create a Logical Volume

```shell
sudo lvcreate -n soft-lv -l 100%FREE soft-vg
```

{{<figure src="/images/extend-new-disk-14.png" >}}

So, I have successfully created a new Logical Volume named `soft-lv` and have mounted it to the Volume Group `soft-vg`, check again with the `lvdisplay` command

```shell
sudo lvdisplay
```

{{<figure src="/images/extend-new-disk-15.png" >}}

# Step 8: Create a File System

Use the `mkfs.ext4` command to create a file system

```shell
sudo mkfs.ext4 /dev/soft-vg/soft-lv
```

{{<figure src="/images/extend-new-disk-16.png" >}}

# Step 9: Mount the Logical Volume

Use the `mount` command to mount the Logical Volume

```shell
sudo mount /dev/soft-vg/soft-lv /mnt/data
```

{{<figure src="/images/extend-new-disk-17.png" >}}

Check again with the `df -h` command

```shell
df -h
```

{{<figure src="/images/extend-new-disk-18.png" >}}

So, I have successfully mounted the Logical Volume `soft-lv` to `/mnt/data`

# Step 10: Automatically mount the Logical Volume when booting

Use the `blkid` command to get the UUID of the Logical Volume

```shell
sudo blkid
```

{{<figure src="/images/extend-new-disk-19.png" >}}

Then add it to the `/etc/fstab` file

```shell
sudo nano /etc/fstab
```

Add the following line

```shell
UUID=e32fee80-371a-4c92-899a-c004d1d79060 /mnt/data ext4 defaults 0 2
```

{{<figure src="/images/extend-new-disk-20.png" >}}

So, I have successfully configured it, reboot the machine with the `sudo reboot` command and check again with the `df -h` command

```shell
df -h
```

{{<figure src="/images/extend-new-disk-21.png" >}}
