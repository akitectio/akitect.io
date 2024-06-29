---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-10-backup-and-restore.webp
images: [/courses/ubuntu-basics-using-virtualbox/lesson-10-backup-and-restore.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [ubuntu-basics-using-virtualbox]
tags: [ubuntu-server, virtualbox]
title: Lesson 10 - Sao Lưu và Phục Hồi
description: Trong bài học này, chúng ta sẽ tìm hiểu cách sao lưu và phục hồi dữ liệu trên Ubuntu Server. Việc sao lưu dữ liệu là một phần quan trọng trong việc bảo vệ dữ liệu quan trọng của bạn khỏi mất mát và hỏng hóc, đồng thời giúp bạn phục hồi dữ liệu nhanh chóng khi cần thiết.
date: 2024-06-29T02:24:30.000Z
weight: 10
---

Trong bài học này, chúng ta sẽ tìm hiểu cách sao lưu và khôi phục dữ liệu trên hệ thống Linux bằng cách sử dụng các công cụ `tar` và `rsync`, thiết lập sao lưu tự động bằng `cron`, và cách khôi phục dữ liệu từ bản sao lưu. Sao lưu dữ liệu là một phần quan trọng để bảo vệ dữ liệu của bạn khỏi mất mát do lỗi hệ thống hoặc các sự cố khác.

### 1. Sử Dụng tar và rsync để Sao Lưu Dữ Liệu

#### `tar`: Tạo Bản Sao Lưu

Lệnh `tar` được sử dụng để tạo bản sao lưu của tệp và thư mục bằng cách nén chúng vào một tệp tarball.

| Lệnh        | Mô tả                                     | Ví dụ                                      |
| ----------- | ----------------------------------------- | ------------------------------------------ |
| `tar -cvf`  | Tạo một tệp tarball từ các tệp và thư mục | `tar -cvf backup.tar /home/user/Documents` |
| `tar -czvf` | Tạo một tệp tarball nén với gzip          | `tar -czvf backup.tar.gz /home/user`       |
| `tar -rvf`  | Thêm tệp vào một tệp tarball đã tồn tại   | `tar -rvf backup.tar newfile.txt`          |
| `tar -xvf`  | Giải nén tệp tarball                      | `tar -xvf backup.tar`                      |
| `tar -xzvf` | Giải nén tệp tarball nén với gzip         | `tar -xzvf backup.tar.gz`                  |

**Ví dụ chi tiết:**

1.  **Tạo một bản sao lưu của thư mục `/home/user/Documents`:**

    ```bash
    tar -cvf backup.tar /home/user/Documents
    ```

    Lệnh này sẽ tạo một tệp `backup.tar` chứa tất cả các tệp và thư mục trong `/home/user/Documents`.

2.  **Tạo một bản sao lưu nén của thư mục `/home/user`:**

    ```bash
    tar -czvf backup.tar.gz /home/user
    ```

    Lệnh này sẽ tạo một tệp `backup.tar.gz` nén chứa tất cả các tệp và thư mục trong `/home/user`.

3.  **Giải nén tệp tarball:**

    ```bash
    tar -xvf backup.tar
    ```

    Lệnh này sẽ giải nén tất cả các tệp và thư mục từ `backup.tar` vào thư mục hiện tại.

#### `rsync`: Sao Lưu Đồng Bộ

Lệnh `rsync` được sử dụng để sao lưu và đồng bộ các tệp và thư mục giữa các vị trí khác nhau.

| Lệnh         | Mô tả                                         | Ví dụ                                                 |
| ------------ | --------------------------------------------- | ----------------------------------------------------- |
| `rsync -av`  | Sao lưu và đồng bộ các tệp và thư mục         | `rsync -av /home/user/Documents /backup`              |
| `rsync -avz` | Sao lưu và đồng bộ các tệp và thư mục qua SSH | `rsync -avz /home/user/Documents user@remote:/backup` |

**Ví dụ chi tiết:**

1.  **Sao lưu thư mục `/home/user/Documents` vào thư mục `/backup`:**

    ```bash
    rsync -av /home/user/Documents /backup
    ```

    Lệnh này sẽ sao chép tất cả các tệp và thư mục từ `/home/user/Documents` vào thư mục `/backup`, duy trì các quyền và cấu trúc thư mục.

2.  **Sao lưu thư mục qua SSH:**

    ```bash
    rsync -avz /home/user/Documents user@remote:/backup
    ```

    Lệnh này sẽ sao chép tất cả các tệp và thư mục từ `/home/user/Documents` tới thư mục `/backup` trên máy chủ từ xa `remote`, sử dụng SSH để bảo mật kết nối.

### 2. Thiết Lập Sao Lưu Tự Động Bằng cron

`cron` là một công cụ mạnh mẽ cho phép bạn tự động hóa các tác vụ trên hệ thống Linux.

#### Tạo tác vụ sao lưu tự động bằng cron

1.  **Mở tệp crontab:**

    ```bash
    crontab -e
    ```

2.  **Thêm dòng sau vào tệp crontab để thiết lập sao lưu tự động hàng ngày lúc 2 giờ sáng:**

    ```plaintext
    0 2 * * * tar -czvf /backup/backup_$(date +\%F).tar.gz /home/user
    ```

    Lệnh này sẽ tạo một bản sao lưu nén của thư mục `/home/user` và lưu vào thư mục `/backup` với tên chứa ngày tạo.

**Ví dụ chi tiết:**

1.  **Thiết lập sao lưu tự động hàng ngày:**

    ```bash
    0 2 * * * rsync -av /home/user/Documents /backup
    ```

    Dòng này sẽ chạy lệnh `rsync` để sao lưu thư mục `/home/user/Documents` vào thư mục `/backup` hàng ngày lúc 2 giờ sáng.

### 3. Khôi Phục Dữ Liệu Từ Bản Sao Lưu

#### Giải nén và khôi phục từ tệp tarball

1.  **Giải nén tệp tarball:**

    ```bash
    tar -xzvf /backup/backup_2024-06-28.tar.gz -C /home/user
    ```

    Lệnh này sẽ giải nén tệp `backup_2024-06-28.tar.gz` vào thư mục `/home/user`.

**Ví dụ chi tiết:**

1.  **Khôi phục từ tệp tarball:**

    ```bash
    tar -xzvf /backup/backup_2024-06-28.tar.gz -C /home/user
    ```

    Lệnh này sẽ giải nén tệp `backup_2024-06-28.tar.gz` vào thư mục `/home/user`, khôi phục lại tất cả các tệp và thư mục đã sao lưu.

#### Sử dụng rsync để khôi phục dữ liệu

1.  **Khôi phục dữ liệu từ thư mục sao lưu:**

    ```bash
    rsync -av /backup /home/user/Documents
    ```

    Lệnh này sẽ sao chép tất cả các tệp và thư mục từ thư mục `/backup` vào thư mục `/home/user/Documents`.

**Ví dụ chi tiết:**

1.  **Khôi phục bằng rsync:**

    ```bash
    rsync -av /backup /home/user/Documents
    ```

    Lệnh này sẽ sao chép lại tất cả các tệp và thư mục từ thư mục sao lưu `/backup` vào thư mục `/home/user/Documents`, khôi phục lại các dữ liệu đã sao lưu.

### Kết Luận

Qua bài học này, bạn đã nắm vững cách sử dụng `tar` và `rsync` để sao lưu và khôi phục dữ liệu, cũng như cách thiết lập sao lưu tự động bằng `cron`. Những kỹ năng này rất quan trọng để đảm bảo dữ liệu của bạn luôn được bảo vệ và có thể khôi phục nhanh chóng khi cần thiết. Chúc bạn thực hành thành công và áp dụng kiến thức này vào công việc hàng ngày của mình.
