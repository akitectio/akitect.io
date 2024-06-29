---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-8-network-configuration.webp
images: [/courses/ubuntu-basics-using-virtualbox/lesson-8-network-configuration.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [ubuntu-basics-using-virtualbox]
tags: [ubuntu-server, virtualbox]
title: Lesson 8 - Cấu Hình Mạng
description: Học cách cấu hình mạng trên Ubuntu Server để kết nối máy ảo với mạng nội bộ và Internet. Chúng ta sẽ tìm hiểu cách cấu hình IP tĩnh và động, cấu hình DNS, và kiểm tra kết nối mạng.
date: 2024-06-28T19:24:30.000Z
weight: 8
---

Trong bài học này, chúng ta sẽ tìm hiểu cách thiết lập và cấu hình mạng trên hệ thống Linux. Việc cấu hình mạng đúng cách là rất quan trọng để đảm bảo kết nối ổn định và bảo mật cho hệ thống. Chúng ta sẽ học cách thiết lập địa chỉ IP tĩnh, kiểm tra kết nối mạng và cấu hình SSH để bảo mật kết nối từ xa.

### 1. Thiết Lập và Cấu Hình Địa Chỉ IP Tĩnh

#### Thiết Lập Địa Chỉ IP Tĩnh

Để thiết lập địa chỉ IP tĩnh trên hệ thống Linux, bạn cần chỉnh sửa tệp cấu hình mạng. Dưới đây là các bước để cấu hình IP tĩnh trên Ubuntu.

**Ví dụ: Cấu hình địa chỉ IP tĩnh trên Ubuntu**

1.  **Mở tệp cấu hình mạng:**

    ```bash
    sudo nano /etc/netplan/01-netcfg.yaml
    ```

2.  **Chỉnh sửa tệp cấu hình:**

    Thêm hoặc chỉnh sửa nội dung sau trong tệp để thiết lập địa chỉ IP tĩnh:

    ```yaml
    network:
      version: 2
      ethernets:
        eth0:
          dhcp4: no
          addresses:
            - 192.168.1.100/24
          gateway4: 192.168.1.1
          nameservers:
            addresses:
              - 8.8.8.8
              - 8.8.4.4
    ```

3.  **Áp dụng cấu hình:**

    ```bash
    sudo netplan apply
    ```

4.  **Kiểm tra cấu hình:**

    ```bash
    ip addr show eth0
    ```

     Kết quả sẽ hiển thị địa chỉ IP tĩnh đã được cấu hình.

     {{< admonition type="info" title="Chi tiết cấu hình" >}}

    -   `dhcp4: no`: Tắt chế độ tự động cấp phát địa chỉ IP (DHCP) cho giao diện `eth0`.
    -   `addresses`: Đặt địa chỉ IP tĩnh là `192.168.1.100` với mặt nạ mạng `/24`.
    -   `gateway4`: Đặt cổng mặc định là `192.168.1.1`.
    -   `nameservers`: Đặt các máy chủ DNS là `8.8.8.8` và `8.8.4.4` (Google DNS).

    {{< /admonition >}}

### 2. Kiểm Tra Kết Nối Mạng

#### `ping`: Kiểm Tra Kết Nối Mạng

Lệnh `ping` được sử dụng để kiểm tra kết nối mạng đến một địa chỉ IP hoặc tên miền.

| Lệnh   | Mô tả                                                  | Ví dụ             |
| ------ | ------------------------------------------------------ | ----------------- |
| `ping` | Kiểm tra kết nối mạng đến một địa chỉ IP hoặc tên miền | `ping google.com` |

**Ví dụ chi tiết:**

1.  **Kiểm tra kết nối đến google.com:**

    ```bash
    ping google.com
    ```

    Kết quả sẽ hiển thị các gói tin được gửi và nhận, giúp bạn kiểm tra kết nối mạng.

    ```plaintext
    PING google.com (216.58.214.14): 56 data bytes
    64 bytes from 216.58.214.14: icmp_seq=0 ttl=54 time=24.5 ms
    64 bytes from 216.58.214.14: icmp_seq=1 ttl=54 time=24.0 ms
    ```

#### `ifconfig`: Hiển Thị Thông Tin Mạng

Lệnh `ifconfig` được sử dụng để hiển thị cấu hình mạng của các giao diện mạng.

| Lệnh       | Mô tả                                          | Ví dụ      |
| ---------- | ---------------------------------------------- | ---------- |
| `ifconfig` | Hiển thị thông tin mạng của các giao diện mạng | `ifconfig` |

**Ví dụ chi tiết:**

1.  **Hiển thị thông tin mạng của tất cả các giao diện:**

    ```bash
    ifconfig
    ```

    Kết quả sẽ hiển thị thông tin chi tiết về các giao diện mạng, bao gồm địa chỉ IP, mặt nạ mạng và trạng thái của giao diện.

    ```plaintext
    eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 192.168.1.100  netmask 255.255.255.0  broadcast 192.168.1.255
            inet6 fe80::a00:27ff:fe4e:66a1  prefixlen 64  scopeid 0x20<link>
            ether 08:00:27:4e:66:a1  txqueuelen 1000  (Ethernet)
            RX packets 3920  bytes 6294358 (6.2 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 2803  bytes 256392 (256.3 KB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    ```

#### `netstat`: Hiển Thị Thông Tin Kết Nối Mạng

Lệnh `netstat` được sử dụng để hiển thị thông tin về các kết nối mạng, bảng định tuyến và các thống kê giao thức.

| Lệnh      | Mô tả                                                                             | Ví dụ           |
| --------- | --------------------------------------------------------------------------------- | --------------- |
| `netstat` | Hiển thị thông tin về các kết nối mạng, bảng định tuyến và các thống kê giao thức | `netstat -tuln` |

**Ví dụ chi tiết:**

1.  **Hiển thị các kết nối đang lắng nghe:**

    ```bash
    netstat -tuln
    ```

    Kết quả sẽ hiển thị các kết nối TCP và UDP đang lắng nghe, bao gồm số cổng và địa chỉ IP.

    ```plaintext
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State      
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
    tcp6       0      0 :::80                   :::*                    LISTEN     
    udp        0      0 0.0.0.0:68              0.0.0.0:*                          
    ```

#### `ss`: Hiển Thị Thông Tin Kết Nối Mạng Nhanh Hơn

Lệnh `ss` là một công cụ mạnh mẽ hơn `netstat` để hiển thị thông tin về các kết nối mạng.

| Lệnh | Mô tả                                  | Ví dụ      |
| ---- | -------------------------------------- | ---------- |
| `ss` | Hiển thị thông tin về các kết nối mạng | `ss -tuln` |

**Ví dụ chi tiết:**

1.  **Hiển thị các kết nối đang lắng nghe:**

    ```bash
    ss -tuln
    ```

    Kết quả sẽ hiển thị các kết nối TCP và UDP đang lắng nghe, bao gồm số cổng và địa chỉ IP.

    ```plaintext
    Netid   State    Recv-Q   Send-Q     Local Address:Port     Peer Address:Port
    tcp     LISTEN   0        128          0.0.0.0:22              0.0.0.0:*     
    tcp     LISTEN   0        128             [::]:80                 [::]:*     
    udp     UNCONN   0        0            0.0.0.0:68              0.0.0.0:*     
    ```

### 3. Cấu Hình SSH và Bảo Mật Kết Nối Từ Xa

#### Cài Đặt SSH

Để cài đặt và cấu hình SSH trên Ubuntu, bạn có thể làm theo các bước sau:

1.  **Cài đặt OpenSSH Server:**

    ```bash
    sudo apt update
    sudo apt install openssh-server
    ```

2.  **Kiểm tra trạng thái của SSH:**

    ```bash
    sudo systemctl status ssh
    ```

3.  **Khởi động dịch vụ SSH:**

    ```bash
    sudo systemctl start ssh
    ```

4.  **Kích hoạt SSH để tự động khởi động khi hệ thống khởi động:**

    ```bash
    sudo systemctl enable ssh
    ```

**Ví dụ chi tiết:**

1.  **Cài đặt OpenSSH Server:**

    ```bash
    sudo apt update
    sudo apt install openssh-server
    ```

    Lệnh này sẽ cài đặt OpenSSH Server trên hệ thống của bạn.

2.  **Kiểm tra trạng thái của SSH:**

    ```bash
    sudo systemctl status ssh
    ```

    Kết quả sẽ hiển thị trạng thái hiện tại của dịch vụ SSH, bao gồm thông tin về PID, thời gian chạy và các thông báo lỗi nếu có.

3.  **Khởi động dịch vụ SSH:**

    ```bash
    sudo systemctl start ssh
    ```

    Lệnh này sẽ khởi động dịch vụ SSH nếu nó chưa chạy.

4.  **Kích hoạt SSH để tự động khởi động khi hệ thống khởi động:**

    ```bash
    sudo systemctl enable ssh
    ```

    Lệnh này sẽ thiết lập dịch vụ SSH tự động khởi động khi hệ thống khởi động.

#### Bảo Mật Kết Nối SSH

Để bảo mật kết nối SSH, bạn có thể thực hiện các bước sau:

1.  **Chỉnh sửa tệp cấu hình SSH:**

    ```bash
    sudo nano /etc/ssh/sshd_config
    ```

2.  **Thay đổi cổng mặc định (tùy chọn):**

    Tìm dòng `#Port 22` và thay đổi thành một số cổng khác, ví dụ:

    ```plaintext
    Port 2222
    ```

3.  **Vô hiệu hóa đăng nhập root qua SSH:**

    Tìm dòng `PermitRootLogin` và thay đổi thành:

    ```plaintext
    PermitRootLogin no
    ```

4.  **Giới hạn truy cập theo IP (tùy chọn):**

    Thêm các dòng sau để giới hạn truy cập theo địa chỉ IP:

    ```plaintext
    AllowUsers user@192.168.1.100
    ```

5.  **Khởi động lại dịch vụ SSH để áp dụng các thay đổi:**

    ```bash
    sudo systemctl restart ssh
    ```

6.  **Cấu hình tường lửa để cho phép kết nối SSH:**

    ```bash
    sudo ufw allow 2222/tcp
    sudo ufw enable
    ```

    Thay `2222` bằng số cổng bạn đã chọn nếu bạn thay đổi cổng mặc định.

**Chi tiết hơn:**

-   **Chỉnh sửa tệp cấu hình SSH:**

    ```bash
    sudo nano /etc/ssh/sshd_config
    ```

     Lệnh này sẽ mở tệp cấu hình SSH trong trình soạn thảo văn bản Nano.

-   **Thay đổi cổng mặc định:**

     Tìm dòng `#Port 22` và thay đổi thành một số cổng khác, ví dụ:

    ```plaintext
    Port 2222
    ```

     Điều này sẽ thay đổi cổng mặc định cho kết nối SSH từ 22 thành 2222, giúp tăng cường bảo mật bằng cách tránh các cuộc tấn công tự động vào cổng mặc định.

-   **Vô hiệu hóa đăng nhập root qua SSH:**

     Tìm dòng `PermitRootLogin` và thay đổi thành:

    ```plaintext
    PermitRootLogin no
    ```

     Điều này sẽ vô hiệu hóa khả năng đăng nhập vào hệ thống qua SSH bằng tài khoản root, giúp bảo vệ hệ thống khỏi các cuộc tấn công từ xa.

-   **Giới hạn truy cập theo IP:**

     Thêm các dòng sau để giới hạn truy cập theo địa chỉ IP:

    ```plaintext
    AllowUsers user@192.168.1.100
    ```

     Điều này sẽ chỉ cho phép người dùng `user` từ địa chỉ IP `192.168.1.100` kết nối tới hệ thống qua SSH.

-   **Khởi động lại dịch vụ SSH để áp dụng các thay đổi:**

    ```bash
    sudo systemctl restart ssh
    ```

     Lệnh này sẽ khởi động lại dịch vụ SSH để áp dụng các thay đổi cấu hình.

-   **Cấu hình tường lửa để cho phép kết nối SSH:**

    ```bash
    sudo ufw allow 2222/tcp
    sudo ufw enable
    ```

     Điều này sẽ cho phép kết nối qua cổng 2222 trong tường lửa và kích hoạt tường lửa.

-   **Test kết nối SSH từ máy khác:**

    ```bash
    ssh user@ip -p 2222
    ```

### Kết Luận

Qua bài học này, bạn đã nắm vững cách thiết lập và cấu hình mạng trên hệ thống Linux, kiểm tra kết nối mạng và bảo mật kết nối SSH từ xa. Những kỹ năng này rất quan trọng để đảm bảo hệ thống của bạn luôn kết nối ổn định và an toàn. Chúc bạn thực hành thành công và áp dụng kiến thức này vào công việc hàng ngày của mình.
