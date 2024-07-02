---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-7-managing-services-and-processes.webp
images: [/courses/ubuntu-basics-using-virtualbox/lesson-7-managing-services-and-processes.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [ubuntu-basics-using-virtualbox]
tags: [ubuntu-server, virtualbox]
title: Lesson 7 - Quản Lý Services và Processes trên Ubuntu
description: Học cách quản lý dịch vụ và tiến trình trên Ubuntu Server để khởi động, dừng, và quản lý các dịch vụ, cũng như xem và quản lý các tiến trình đang chạy trên hệ thống.
date: 2024-06-28T19:24:30.000Z
weight: 7
---

Trong bài học này, chúng ta sẽ tìm hiểu cách quản lý dịch vụ và quy trình trong hệ thống Linux. Việc quản lý dịch vụ và quy trình là một phần quan trọng trong việc duy trì hoạt động ổn định và hiệu quả của hệ thống. Chúng ta sẽ học cách sử dụng các lệnh như `systemctl`, `service` để quản lý dịch vụ và các lệnh như `ps`, `top`, `htop`, và `kill` để quản lý quy trình.

### 1. Quản Lý Dịch Vụ

#### `systemctl`: Quản Lý Dịch Vụ Hệ Thống

Lệnh `systemctl` là công cụ chính để quản lý dịch vụ trong các hệ thống sử dụng systemd.

| Lệnh                | Mô tả                                                | Ví dụ                          |
| ------------------- | ---------------------------------------------------- | ------------------------------ |
| `systemctl start`   | Khởi động một dịch vụ                                | `sudo systemctl start nginx`   |
| `systemctl stop`    | Dừng một dịch vụ                                     | `sudo systemctl stop nginx`    |
| `systemctl restart` | Khởi động lại một dịch vụ                            | `sudo systemctl restart nginx` |
| `systemctl enable`  | Kích hoạt dịch vụ để tự động khởi động khi boot      | `sudo systemctl enable nginx`  |
| `systemctl disable` | Vô hiệu hóa dịch vụ không tự động khởi động khi boot | `sudo systemctl disable nginx` |
| `systemctl status`  | Kiểm tra trạng thái của dịch vụ                      | `sudo systemctl status nginx`  |

**Ví dụ chi tiết:**

1.  **Cài đặt Nginx:**

    ```bash
    sudo apt update
    sudo apt install nginx
    ```

2.  **Khởi động Nginx:**

    ```bash
    sudo systemctl start nginx
    ```

3.  **Kiểm tra trạng thái của Nginx:**

    ```bash
    sudo systemctl status nginx
    ```

    Kết quả sẽ hiển thị trạng thái hiện tại của dịch vụ Nginx, bao gồm thông tin về PID, thời gian chạy và các thông báo lỗi nếu có.

4.  **Dừng Nginx:**

    ```bash
    sudo systemctl stop nginx
    ```

5.  **Khởi động lại Nginx:**

    ```bash
    sudo systemctl restart nginx
    ```

6.  **Kích hoạt Nginx để tự động khởi động khi hệ thống khởi động:**

    ```bash
    sudo systemctl enable nginx
    ```

7.  **Vô hiệu hóa Nginx để không tự động khởi động khi hệ thống khởi động:**

    ```bash
    sudo systemctl disable nginx
    ```

#### `service`: Quản Lý Dịch Vụ Truyền Thống

Lệnh `service` được sử dụng để quản lý các dịch vụ trên các hệ thống không sử dụng systemd.

| Lệnh              | Mô tả                           | Ví dụ                        |
| ----------------- | ------------------------------- | ---------------------------- |
| `service start`   | Khởi động một dịch vụ           | `sudo service nginx start`   |
| `service stop`    | Dừng một dịch vụ                | `sudo service nginx stop`    |
| `service restart` | Khởi động lại một dịch vụ       | `sudo service nginx restart` |
| `service status`  | Kiểm tra trạng thái của dịch vụ | `sudo service nginx status`  |

**Ví dụ chi tiết:**

1.  **Khởi động Nginx:**

    ```bash
    sudo service nginx start
    ```

2.  **Kiểm tra trạng thái của Nginx:**

    ```bash
    sudo service nginx status
    ```

3.  **Dừng Nginx:**

    ```bash
    sudo service nginx stop
    ```

4.  **Khởi động lại Nginx:**

    ```bash
    sudo service nginx restart
    ```

### 2. Quản Lý Quy Trình

#### `ps`: Hiển Thị Quy Trình Đang Chạy

Lệnh `ps` được sử dụng để hiển thị các quy trình đang chạy trong hệ thống.

| Lệnh     | Mô tả                                          | Ví dụ    |
| -------- | ---------------------------------------------- | -------- |
| `ps`     | Hiển thị các quy trình của người dùng hiện tại | `ps`     |
| `ps aux` | Hiển thị tất cả các quy trình đang chạy        | `ps aux` |
| `ps -ef` | Hiển thị các quy trình với định dạng đầy đủ    | `ps -ef` |

**Ví dụ chi tiết:**

1.  **Hiển thị các quy trình của người dùng hiện tại:**

    ```bash
    ps
    ```

2.  **Hiển thị tất cả các quy trình đang chạy:**

    ```bash
    ps aux
    ```

3.  **Hiển thị các quy trình với định dạng đầy đủ:**

    ```bash
    ps -ef
    ```

#### `top`: Theo Dõi Quy Trình Thời Gian Thực

Lệnh `top` cung cấp một giao diện tương tác để theo dõi các quy trình đang chạy trong thời gian thực.

| Lệnh  | Mô tả                                                 | Ví dụ |
| ----- | ----------------------------------------------------- | ----- |
| `top` | Hiển thị các quy trình đang chạy trong thời gian thực | `top` |

**Ví dụ chi tiết:**

1.  **Chạy `top`:**

    ```bash
    top
    ```

    Giao diện `top` sẽ hiển thị danh sách các quy trình đang chạy trong thời gian thực, bao gồm thông tin về CPU, bộ nhớ và thời gian chạy. Bạn có thể sử dụng các phím tắt như `q` để thoát, `k` để giết quy trình, và `r` để thay đổi độ ưu tiên của quy trình.

#### `htop`: Giao Diện Theo Dõi Quy Trình Nâng Cao

Lệnh `htop` là phiên bản nâng cao của `top` với giao diện người dùng thân thiện hơn.

| Lệnh   | Mô tả                                                                        | Ví dụ  |
| ------ | ---------------------------------------------------------------------------- | ------ |
| `htop` | Hiển thị các quy trình đang chạy trong thời gian thực với giao diện nâng cao | `htop` |

**Ví dụ chi tiết:**

1.  **Cài đặt `htop`:**

    ```bash
    sudo apt install htop
    ```

2.  **Chạy `htop`:**

    ```bash
    htop
    ```

    Giao diện `htop` sẽ hiển thị danh sách các quy trình đang chạy với giao diện đồ họa nâng cao, cho phép bạn dễ dàng theo dõi và quản lý các quy trình. Bạn có thể sử dụng các phím tắt như `F10` để thoát, `F9` để giết quy trình, và `F2` để cấu hình `htop`.

#### `kill`: Dừng Quy Trình

Lệnh `kill` được sử dụng để dừng một quy trình đang chạy.

| Lệnh      | Mô tả                                                   | Ví dụ         |
| --------- | ------------------------------------------------------- | ------------- |
| `kill`    | Gửi tín hiệu để dừng một quy trình                      | `kill PID`    |
| `kill -9` | Gửi tín hiệu SIGKILL để dừng ngay lập tức một quy trình | `kill -9 PID` |

**Ví dụ chi tiết:**

1.  **Hiển thị danh sách các quy trình đang chạy:**

    ```bash
    ps aux
    ```

    Tìm ID quy trình (PID) của quy trình bạn muốn dừng.

2.  **Dừng quy trình:**

    ```bash
    kill 1234
    ```

    Lệnh này sẽ dừng quy trình có ID là `1234`.

3.  **Dừng ngay lập tức quy trình:**

    ```bash
    kill -9 1234
    ```

    Lệnh này sẽ gửi tín hiệu SIGKILL để dừng ngay lập tức quy trình có ID là `1234`.

### Kết Luận

Qua bài học này, bạn đã nắm vững cách quản lý dịch vụ và quy trình trong hệ thống Linux. Những lệnh này là công cụ quan trọng giúp bạn duy trì và điều hành hệ thống một cách hiệu quả. Chúc bạn thực hành thành công và áp dụng kiến thức này vào công việc hàng ngày của mình.
