---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-5-basic-linux-commands.webp
images: [/courses/ubuntu-basics-using-virtualbox/lesson-5-basic-linux-commands.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [ubuntu-basics-using-virtualbox]
tags: [ubuntu-server, virtualbox]
title: Lesson 5 - Các Lệnh Cơ Bản Trên Linux
description: Học cách sử dụng các lệnh cơ bản trên Linux để quản lý hệ thống và tương tác với hệ điều hành, bao gồm điều hướng thư mục, quản lý tập tin và thư mục, xem và chỉnh sửa tệp.
date: 2024-06-28T19:24:30.000Z
weight: 5
---

Trong bài học này, chúng ta sẽ tìm hiểu về các lệnh cơ bản của Linux để điều hướng thư mục, quản lý tập tin và thư mục, cũng như xem và chỉnh sửa tệp. Những lệnh này là nền tảng quan trọng giúp bạn thao tác và quản lý hệ thống Linux một cách hiệu quả.

### 1. Điều Hướng Thư Mục

| Lệnh  | Mô tả                                             | Ví dụ                     |
| ----- | ------------------------------------------------- | ------------------------- |
| `ls`  | Liệt kê các tệp và thư mục trong thư mục hiện tại | `ls`                      |
| `cd`  | Thay đổi thư mục làm việc hiện tại                | `cd /home/user/Documents` |
| `pwd` | Hiển thị đường dẫn đầy đủ của thư mục hiện tại    | `pwd`                     |

#### Ví dụ Chi Tiết:

1.  **Liệt Kê Tệp và Thư Mục (`ls`):**

    ```bash
    ls
    ```

    Lệnh này liệt kê tất cả các tệp và thư mục trong thư mục hiện tại.

    ```plaintext
    Documents  Downloads  Music  Pictures  Videos
    ```

    Để hiển thị thông tin chi tiết bao gồm quyền truy cập, chủ sở hữu, kích thước và ngày sửa đổi:

    ```bash
    ls -l
    ```

    ```plaintext
    drwxr-xr-x 2 user user 4096 Apr 10 09:00 Documents
    -rw-r--r-- 1 user user  512 Apr  8 12:30 file1.txt
    ```

    Để hiển thị các tệp ẩn (những tệp bắt đầu bằng dấu chấm):

    ```bash
    ls -a
    ```

    ```plaintext
    .bashrc  .profile  Documents  Downloads
    ```

2.  **Thay Đổi Thư Mục (`cd`):**

    ```bash
    cd /home/user/Documents
    ```

    Lệnh này thay đổi thư mục làm việc hiện tại thành `/home/user/Documents`.

    ```bash
    cd ..
    ```

    Lệnh này di chuyển lên một cấp thư mục.

    ```bash
    cd ~
    ```

    Lệnh này thay đổi thư mục thành thư mục chính của người dùng.

3.  **Hiển Thị Đường Dẫn Thư Mục Hiện Tại (`pwd`):**

    ```bash
    pwd
    ```

    Lệnh này hiển thị đường dẫn đầy đủ của thư mục hiện tại.

    ```plaintext
    /home/user/Documents
    ```

### 2. Quản Lý Tập Tin và Thư Mục

| Lệnh    | Mô tả                                                        | Ví dụ                                |
| ------- | ------------------------------------------------------------ | ------------------------------------ |
| `cp`    | Sao chép các tệp hoặc thư mục từ vị trí này sang vị trí khác | `cp file1.txt /home/user/backup/`    |
| `mv`    | Di chuyển hoặc đổi tên tệp và thư mục                        | `mv file1.txt /home/user/Documents/` |
| `rm`    | Xóa các tệp hoặc thư mục                                     | `rm file1.txt`                       |
| `mkdir` | Tạo thư mục mới                                              | `mkdir /home/user/newfolder`         |

#### Ví dụ Chi Tiết:

1.  **Sao Chép Tệp và Thư Mục (`cp`):**

    ```bash
    cp file1.txt /home/user/backup/
    ```

    Lệnh này sao chép `file1.txt` vào thư mục `/home/user/backup/`.

    Để sao chép một thư mục và nội dung của nó:

    ```bash
    cp -r /home/user/Documents /home/user/backup/
    ```

    Lệnh này sao chép thư mục `Documents` và nội dung của nó vào thư mục `/home/user/backup/`.

2.  **Di Chuyển hoặc Đổi Tên Tệp và Thư Mục (`mv`):**

    ```bash
    mv file1.txt /home/user/Documents/
    ```

    Lệnh này di chuyển `file1.txt` vào thư mục `/home/user/Documents/`.

    Để đổi tên một tệp:

    ```bash
    mv oldname.txt newname.txt
    ```

    Lệnh này đổi tên `oldname.txt` thành `newname.txt`.

    Để di chuyển và đổi tên một tệp:

    ```bash
    mv oldname.txt /home/user/Documents/newname.txt
    ```

    Lệnh này di chuyển `oldname.txt` vào thư mục `/home/user/Documents/` và đổi tên thành `newname.txt`.

3.  **Xóa Tệp và Thư Mục (`rm`):**

    ```bash
    rm file1.txt
    ```

    Lệnh này xóa `file1.txt`.

    Để xóa một thư mục và tất cả nội dung bên trong:

    ```bash
    rm -r /home/user/Documents/
    ```

    Lệnh này xóa thư mục `/home/user/Documents/` và tất cả các tệp và thư mục con bên trong.

4.  **Tạo Thư Mục Mới (`mkdir`):**

    ```bash
    mkdir /home/user/newfolder
    ```

    Lệnh này tạo một thư mục mới có tên là `newfolder` trong thư mục `/home/user/`.

    Để tạo các thư mục lồng nhau:

    ```bash
    mkdir -p /home/user/newfolder/subfolder
    ```

    Lệnh này tạo `newfolder` và `subfolder` bên trong nếu chúng chưa tồn tại.

### 3. Xem và Chỉnh Sửa Tệp

| Lệnh   | Mô tả                                         | Ví dụ            |
| ------ | --------------------------------------------- | ---------------- |
| `cat`  | Hiển thị nội dung của một tệp                 | `cat file1.txt`  |
| `nano` | Mở một tệp trong trình soạn thảo văn bản Nano | `nano file1.txt` |
| `vim`  | Mở một tệp trong trình soạn thảo văn bản Vim  | `vim file1.txt`  |

#### 3.1. Vim Editor

| Lệnh       | Mô tả                                        | Ví dụ                                                        |
| ---------- | -------------------------------------------- | ------------------------------------------------------------ |
| `vim`      | Mở một tệp trong trình soạn thảo văn bản Vim | `vim file1.txt`                                              |
| `Esc`      | Chế độ lệnh                                  | Nhấn `Esc` để vào chế độ lệnh                                |
| `i`        | Chế độ chèn                                  | Nhấn `i` để vào chế độ chèn văn bản                          |
| `:wq`      | Lưu và thoát                                 | Nhập `:wq` và nhấn `Enter` để lưu tệp và thoát Vim           |
| `:q!`      | Thoát mà không lưu                           | Nhập `:q!` và nhấn `Enter` để thoát mà không lưu tệp         |
| `/text`    | Tìm kiếm văn bản                             | Nhập `/text` và nhấn `Enter` để tìm kiếm từ "text" trong tệp |
| `dd`       | Xóa dòng hiện tại                            | Nhấn `dd` để xóa dòng hiện tại                               |
| `yy`       | Sao chép dòng hiện tại                       | Nhấn `yy` để sao chép dòng hiện tại                          |
| `p`        | Dán dòng đã sao chép hoặc cắt                | Nhấn `p` để dán dòng đã sao chép hoặc cắt sau dòng hiện tại  |
| `u`        | Hoàn tác                                     | Nhấn `u` để hoàn tác hành động cuối cùng                     |
| `Ctrl + r` | Quay lại hành động vừa hoàn tác              | Nhấn `Ctrl + r` để quay lại hành động vừa hoàn tác           |

**Ví dụ Chi Tiết:**

1.  **Mở tệp trong Vim:**

    ```bash
    vim file1.txt
    ```

2.  **Chế độ lệnh và chèn trong Vim:**

    -   Nhấn `Esc` để vào chế độ lệnh.
    -   Nhấn `i` để vào chế độ chèn văn bản.

3.  **Lưu và thoát Vim:**

    -   Nhập `:wq` và nhấn `Enter` để lưu tệp và thoát.
    -   Nhập `:q!` và nhấn `Enter` để thoát mà không lưu.

4.  **Tìm kiếm và thay thế văn bản:**

    -   Nhập `/text` và nhấn `Enter` để tìm kiếm từ "text".
    -   Để thay thế, sử dụng lệnh `:%s/oldtext/newtext/g`.

5.  **Xóa, sao chép và dán dòng:**

    -   Nhấn `dd` để xóa dòng hiện tại.
    -   Nhấn `yy` để sao chép dòng hiện tại.
    -   Nhấn `p` để dán dòng đã sao chép hoặc cắt.

6.  **Hoàn tác và quay lại hành động:**

    -   Nhấn `u` để hoàn tác hành động cuối cùng.
    -   Nhấn `Ctrl + r` để quay lại hành động vừa hoàn tác.

#### 3.2. Nano Editor

| Lệnh       | Mô t

| ả          | Ví dụ                                         |                                                                         |
| ---------- | --------------------------------------------- | ----------------------------------------------------------------------- |
| `nano`     | Mở một tệp trong trình soạn thảo văn bản Nano | `nano file1.txt`                                                        |
| `Ctrl + O` | Lưu tệp                                       | Nhấn `Ctrl + O`, sau đó nhấn `Enter` để lưu                             |
| `Ctrl + X` | Thoát Nano                                    | Nhấn `Ctrl + X` để thoát Nano                                           |
| `Ctrl + K` | Cắt văn bản                                   | Di chuyển con trỏ đến dòng cần cắt, nhấn `Ctrl + K`                     |
| `Ctrl + U` | Dán văn bản                                   | Di chuyển con trỏ đến vị trí cần dán, nhấn `Ctrl + U`                   |
| `Ctrl + W` | Tìm kiếm văn bản                              | Nhấn `Ctrl + W`, nhập từ khóa cần tìm, nhấn `Enter`                     |
| `Ctrl + \` | Thay thế văn bản                              | Nhấn `Ctrl + \`, nhập từ khóa cần thay thế và từ khóa mới, nhấn `Enter` |
| `Ctrl + G` | Hiển thị trợ giúp                             | Nhấn `Ctrl + G` để hiển thị hướng dẫn sử dụng Nano                      |

**Ví dụ Chi Tiết:**

1.  **Mở tệp trong Nano:**

    ```bash
    nano file1.txt
    ```

2.  **Lưu tệp trong Nano:**

    -   Nhấn `Ctrl + O`.
    -   Nhấn `Enter` để xác nhận lưu tệp.

3.  **Thoát Nano:**

    -   Nhấn `Ctrl + X`.

4.  **Cắt và dán văn bản:**

    -   Để cắt, di chuyển con trỏ đến dòng cần cắt và nhấn `Ctrl + K`.
    -   Để dán, di chuyển con trỏ đến vị trí cần dán và nhấn `Ctrl + U`.

5.  **Tìm kiếm và thay thế văn bản:**

    -   Nhấn `Ctrl + W` để tìm kiếm từ khóa.
    -   Nhấn `Ctrl + \` để thay thế từ khóa.

6.  **Hiển thị trợ giúp:**

    -   Nhấn `Ctrl + G` để hiển thị hướng dẫn sử dụng Nano.

### Kết Luận

Qua bài học này, bạn đã nắm vững các lệnh cơ bản để điều hướng thư mục, quản lý tập tin và thư mục, cũng như xem và chỉnh sửa tệp trên hệ thống Linux. Những lệnh này là nền tảng quan trọng giúp bạn thao tác và quản lý hệ thống một cách hiệu quả. Chúc bạn thực hành thành công và áp dụng kiến thức này vào công việc hàng ngày của mình.
