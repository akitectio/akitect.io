---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-9-package-management-with-apt.webp
images: [/courses/ubuntu-basics-using-virtualbox/lesson-9-package-management-with-apt.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [ubuntu-basics-using-virtualbox]
tags: [ubuntu-server, virtualbox]
title: Lesson 9 - Quản lý Packages với APT
description: Trong bài học này, chúng ta sẽ tìm hiểu cách quản lý các gói phần mềm trên Ubuntu Server bằng APT, một công cụ quản lý gói phổ biến trên các hệ điều hành dựa trên Debian, bao gồm Ubuntu.
date: 2024-06-29T01:24:30.000Z
weight: 9
---

Trong bài học này, chúng ta sẽ tìm hiểu về cách quản lý gói phần mềm trên hệ thống Linux sử dụng công cụ `apt`. Việc quản lý gói phần mềm là một phần quan trọng trong việc duy trì và vận hành hệ thống, giúp bạn cài đặt, cập nhật và gỡ bỏ các phần mềm một cách hiệu quả. Chúng ta sẽ học cách sử dụng các lệnh `apt-get install`, `apt-get update`, `apt-get upgrade` và cách quản lý kho lưu trữ phần mềm.

### 1. Cài Đặt và Cập Nhật Gói Phần Mềm

#### `apt-get install`: Cài Đặt Gói Phần Mềm

Lệnh `apt-get install` được sử dụng để cài đặt các gói phần mềm mới trên hệ thống.

| Lệnh              | Mô tả                    | Ví dụ                          |
| ----------------- | ------------------------ | ------------------------------ |
| `apt-get install` | Cài đặt gói phần mềm mới | `sudo apt-get install apache2` |

**Ví dụ chi tiết:**

1.  **Cài đặt Apache2:**

    ```bash
    sudo apt-get install apache2
    ```

    Lệnh này sẽ tải và cài đặt Apache2, một trong những máy chủ web phổ biến nhất.

    ```plaintext
    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    The following additional packages will be installed:
      apache2-bin apache2-data apache2-utils
    Suggested packages:
      www-browser apache2-doc apache2-suexec-pristine | apache2-suexec-custom
    The following NEW packages will be installed:
      apache2 apache2-bin apache2-data apache2-utils
    0 upgraded, 4 newly installed, 0 to remove and 0 not upgraded.
    Need to get 1,720 kB of archives.
    After this operation, 6,859 kB of additional disk space will be used.
    Do you want to continue? [Y/n]
    ```

    Nhấn `Y` và Enter để tiếp tục quá trình cài đặt.

#### `apt-get update`: Cập Nhật Danh Sách Gói

Lệnh `apt-get update` được sử dụng để cập nhật danh sách các gói có sẵn từ các kho lưu trữ phần mềm.

| Lệnh             | Mô tả                                                  | Ví dụ                 |
| ---------------- | ------------------------------------------------------ | --------------------- |
| `apt-get update` | Cập nhật danh sách các gói phần mềm từ các kho lưu trữ | `sudo apt-get update` |

**Ví dụ chi tiết:**

1.  **Cập nhật danh sách gói:**

    ```bash
    sudo apt-get update
    ```

    Lệnh này sẽ cập nhật danh sách các gói phần mềm có sẵn từ các kho lưu trữ đã được cấu hình trong tệp `/etc/apt/sources.list`.

    ```plaintext
    Hit:1 http://us.archive.ubuntu.com/ubuntu focal InRelease
    Get:2 http://us.archive.ubuntu.com/ubuntu focal-updates InRelease [111 kB]
    Get:3 http://us.archive.ubuntu.com/ubuntu focal-backports InRelease [98.3 kB]
    Get:4 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]
    Fetched 323 kB in 2s (142 kB/s)
    Reading package lists... Done
    ```

#### `apt-get upgrade`: Nâng Cấp Các Gói Đã Cài Đặt

Lệnh `apt-get upgrade` được sử dụng để nâng cấp các gói phần mềm đã được cài đặt lên phiên bản mới nhất.

| Lệnh              | Mô tả                                                       | Ví dụ                  |
| ----------------- | ----------------------------------------------------------- | ---------------------- |
| `apt-get upgrade` | Nâng cấp các gói phần mềm đã cài đặt lên phiên bản mới nhất | `sudo apt-get upgrade` |

**Ví dụ chi tiết:**

1.  **Nâng cấp các gói phần mềm:**

    ```bash
    sudo apt-get upgrade
    ```

    Lệnh này sẽ tải về và cài đặt các phiên bản mới nhất của các gói phần mềm đã được cài đặt trên hệ thống.

    ```plaintext
    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    Calculating upgrade... Done
    The following packages will be upgraded:
      apache2 apache2-bin apache2-data apache2-utils
    4 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
    Need to get 1,320 kB of archives.
    After this operation, 1,024 B of additional disk space will be used.
    Do you want to continue? [Y/n]
    ```

    Nhấn `Y` và Enter để tiếp tục quá trình nâng cấp.

### 2. Quản Lý Kho Lưu Trữ Phần Mềm

Các kho lưu trữ phần mềm là nguồn cung cấp các gói phần mềm cho hệ thống. Bạn có thể thêm, xóa và quản lý các kho lưu trữ này thông qua tệp cấu hình `/etc/apt/sources.list`.

#### Thêm Kho Lưu Trữ Mới

Để thêm một kho lưu trữ mới, bạn cần chỉnh sửa tệp `/etc/apt/sources.list` hoặc tạo một tệp mới trong thư mục `/etc/apt/sources.list.d/`.

**Ví dụ:**

1.  **Mở tệp cấu hình kho lưu trữ:**

    ```bash
    sudo nano /etc/apt/sources.list
    ```

2.  **Thêm dòng sau để thêm kho lưu trữ mới:**

    ```plaintext
    deb http://repository.example.com/ubuntu focal main
    ```

3.  **Lưu tệp và cập nhật danh sách gói:**

    ```bash
    sudo apt-get update
    ```

#### Xóa Kho Lưu Trữ

Để xóa một kho lưu trữ, bạn cần xóa dòng tương ứng trong tệp `/etc/apt/sources.list` hoặc xóa tệp trong thư mục `/etc/apt/sources.list.d/`.

**Ví dụ:**

1.  **Mở tệp cấu hình kho lưu trữ:**

    ```bash
    sudo nano /etc/apt/sources.list
    ```

2.  **Xóa dòng tương ứng với kho lưu trữ cần xóa.**

3.  **Lưu tệp và cập nhật danh sách gói:**

    ```bash
    sudo apt-get update
    ```

### Kết Luận

Qua bài học này, bạn đã nắm vững cách cài đặt, cập nhật và nâng cấp gói phần mềm trên hệ thống Linux sử dụng `apt`. Bạn cũng đã học cách quản lý kho lưu trữ phần mềm, một phần quan trọng trong việc duy trì hệ thống luôn cập nhật và bảo mật. Chúc bạn thực hành thành công và áp dụng kiến thức này vào công việc hàng ngày của mình.
