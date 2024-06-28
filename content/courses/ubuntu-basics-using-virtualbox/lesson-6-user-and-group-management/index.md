---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-6-user-and-group-management.webp
images:
  - /courses/ubuntu-basics-using-virtualbox/lesson-6-user-and-group-management.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - ubuntu-basics-using-virtualbox
tags:
  - ubuntu-server
  - virtualbox
title: Lesson 6 - Quản Lý Người Dùng và Nhóm
description: Học cách quản lý người dùng và nhóm trên Ubuntu Server để tạo, sửa đổi và xóa người dùng, cũng như gán quyền truy cập cho người dùng trong hệ thống.
date: 2024-06-28T10:24:30-09:00
weight: 6
---

Trong bài học này, chúng ta sẽ tìm hiểu cách quản lý người dùng và nhóm trong hệ thống Linux. Việc quản lý người dùng và nhóm là một phần quan trọng để đảm bảo an ninh và sự tổ chức trong hệ thống. Chúng ta sẽ học cách tạo và quản lý người dùng, quản lý nhóm, và phân quyền truy cập bằng các lệnh cơ bản như `adduser`, `deluser`, `usermod`, `groupadd`, `groupdel`, `groupmod`, `chmod`, `chown`, và `chgrp`.

### 1. Tạo và Quản Lý Người Dùng

| Lệnh     | Mô tả                                  | Ví dụ                                         |
|----------|----------------------------------------|-----------------------------------------------|
| `adduser`| Tạo một người dùng mới trong hệ thống  | `sudo adduser username`                       |
| `deluser`| Xóa một người dùng khỏi hệ thống       | `sudo deluser username`                       |
| `usermod`| Sửa đổi thông tin của người dùng hiện có| `sudo usermod -aG groupname username`         |

### 2. Quản Lý Nhóm

| Lệnh      | Mô tả                                    | Ví dụ                                                |
|-----------|------------------------------------------|------------------------------------------------------|
| `groupadd`| Tạo một nhóm mới trong hệ thống          | `sudo groupadd groupname`                            |
| `groupdel`| Xóa một nhóm khỏi hệ thống               | `sudo groupdel groupname`                            |
| `groupmod`| Sửa đổi thông tin của nhóm hiện có       | `sudo groupmod -n newgroupname oldgroupname`         |

### 3. Phân Quyền và Quyền Truy Cập

| Lệnh      | Mô tả                                       | Ví dụ                                                   |
|-----------|---------------------------------------------|---------------------------------------------------------|
| `chmod`   | Thay đổi quyền truy cập của tệp hoặc thư mục| `sudo chmod 755 filename`                               |
| `chown`   | Thay đổi chủ sở hữu của tệp hoặc thư mục    | `sudo chown user:group filename`                        |
| `chgrp`   | Thay đổi nhóm sở hữu của tệp hoặc thư mục   | `sudo chgrp groupname filename`                         |

### 1. Tạo và Quản Lý Người Dùng

#### `adduser`: Tạo Người Dùng Mới

Lệnh `adduser` được sử dụng để tạo một người dùng mới trong hệ thống.

| Lệnh            | Mô tả                               | Ví dụ                        |
|-----------------|-------------------------------------|------------------------------|
| `adduser`       | Tạo một người dùng mới              | `sudo adduser username`      |

**Ví dụ:**

```bash
sudo adduser username
```

Lệnh này sẽ tạo một người dùng mới có tên là `username` và yêu cầu bạn nhập thông tin chi tiết như mật khẩu, tên đầy đủ, và thông tin liên hệ.

#### `deluser`: Xóa Người Dùng

Lệnh `deluser` được sử dụng để xóa một người dùng khỏi hệ thống.

| Lệnh            | Mô tả                               | Ví dụ                        |
|-----------------|-------------------------------------|------------------------------|
| `deluser`       | Xóa một người dùng                  | `sudo deluser username`      |

**Ví dụ:**

```bash
sudo deluser username
```

Lệnh này sẽ xóa người dùng `username` khỏi hệ thống.

#### `usermod`: Sửa Đổi Thông Tin Người Dùng

Lệnh `usermod` được sử dụng để sửa đổi thông tin của một người dùng hiện có.

| Lệnh            | Mô tả                               | Ví dụ                        |
|-----------------|-------------------------------------|------------------------------|
| `usermod`       | Sửa đổi thông tin người dùng        | `sudo usermod -aG groupname username` |

**Ví dụ:**

```bash
sudo usermod -aG groupname username
```

Lệnh này sẽ thêm người dùng `username` vào nhóm `groupname`.

### 2. Quản Lý Nhóm

#### `groupadd`: Tạo Nhóm Mới

Lệnh `groupadd` được sử dụng để tạo một nhóm mới trong hệ thống.

| Lệnh            | Mô tả                               | Ví dụ                        |
|-----------------|-------------------------------------|------------------------------|
| `groupadd`      | Tạo một nhóm mới                    | `sudo groupadd groupname`    |

**Ví dụ:**

```bash
sudo groupadd groupname
```

Lệnh này sẽ tạo một nhóm mới có tên là `groupname`.

#### `groupdel`: Xóa Nhóm

Lệnh `groupdel` được sử dụng để xóa một nhóm khỏi hệ thống.

| Lệnh            | Mô tả                               | Ví dụ                        |
|-----------------|-------------------------------------|------------------------------|
| `groupdel`      | Xóa một nhóm                        | `sudo groupdel groupname`    |

**Ví dụ:**

```bash
sudo groupdel groupname
```

Lệnh này sẽ xóa nhóm `groupname` khỏi hệ thống.

#### `groupmod`: Sửa Đổi Thông Tin Nhóm

Lệnh `groupmod` được sử dụng để sửa đổi thông tin của một nhóm hiện có.

| Lệnh            | Mô tả                               | Ví dụ                        |
|-----------------|-------------------------------------|------------------------------|
| `groupmod`      | Sửa đổi thông tin nhóm              | `sudo groupmod -n newgroupname oldgroupname` |

**Ví dụ:**

```bash
sudo groupmod -n newgroupname oldgroupname
```

Lệnh này sẽ đổi tên nhóm từ `oldgroupname` thành `newgroupname`.

### 3. Phân Quyền và Quyền Truy Cập

#### `chmod`: Thay Đổi Quyền Truy Cập

Lệnh `chmod` được sử dụng để thay đổi quyền truy cập của tệp hoặc thư mục.

| Lệnh            | Mô tả                               | Ví dụ                        |
|-----------------|-------------------------------------|------------------------------|
| `chmod`         | Thay đổi quyền truy cập             | `sudo chmod 755 filename`    |

**Ví dụ:**

```bash
sudo chmod 755 filename
```

Lệnh này sẽ thay đổi quyền truy cập của `filename` thành `755`.

#### `chown`: Thay Đổi Chủ Sở Hữu

Lệnh `chown` được sử dụng để thay đổi chủ sở hữu của tệp hoặc thư mục.

| Lệnh            | Mô tả                               | Ví dụ                        |
|-----------------|-------------------------------------|------------------------------|
| `chown`         | Thay đổi chủ sở hữu                 | `sudo chown user:group filename` |

**Ví dụ:**

```bash
sudo chown user:group filename
```

Lệnh này sẽ thay đổi chủ sở hữu của `filename` thành `user` và nhóm `group`.

#### `chgrp`: Thay Đổi Nhóm Sở Hữu

Lệnh `chgrp` được sử dụng để thay đổi nhóm sở hữu của tệp hoặc thư mục.

| Lệnh            | Mô tả                               | Ví dụ                        |
|-----------------|-------------------------------------|------------------------------|
| `chgrp`         | Thay đổi nhóm sở hữu                | `sudo chgrp groupname filename` |

**Ví dụ:**

```bash
sudo chgrp groupname filename
```

Lệnh này sẽ thay đổi nhóm sở hữu của `filename` thành `groupname`.

### Kết Luận

Qua bài học này, bạn đã nắm vững cách quản lý người dùng và nhóm trong hệ thống Linux, cũng như cách phân quyền truy cập cho các tệp và thư mục. Những kỹ năng này rất quan trọng để đảm bảo an ninh và sự tổ chức trong hệ thống. Chúc bạn thực hành thành công và áp dụng kiến thức này vào công việc hàng ngày của mình.