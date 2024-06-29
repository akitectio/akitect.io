* * *

categories:

-   linux
-   network
    date: 2024-02-26T10:23:30-09:00
    draft: false
    featuredImage: /labs/linux/memcached.png
    images:
-   /labs/linux/memcached.png
    license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
    tags:
-   cache
-   memcached
    title: Cài đặt và bảo mật Memcached trên Ubuntu 22.04
    description: Learn how to install and secure Memcached on Ubuntu 22.04. Memcached is a high-performance, distributed memory object caching system, generic in nature, but intended for use in speeding up dynamic web applications by alleviating database load.
    description: Cài đặt và bảo mật Memcached trên Ubuntu 22.04. Memcached là một hệ thống caching đối tượng bộ nhớ phân tán hiệu suất cao, tổng quát trong tính chất, nhưng dự định để sử dụng trong việc tăng tốc ứng dụng web động bằng cách giảm tải cơ sở dữ liệu.
    weight: 1

* * *

### Memcached là gì?

Memcached là một hệ thống caching đối tượng bộ nhớ phân tán hiệu suất cao, tổng quát trong tính chất, nhưng dự định để sử dụng trong việc tăng tốc ứng dụng web động bằng cách giảm tải cơ sở dữ liệu. Memcached được phát triển bởi Brad Fitzpatrick vào năm 2003. Memcached là một phần mềm mã nguồn mở và được phân phối theo giấy phép BSD.

### Cài đặt Memcached trên Ubuntu 22.04

Để cài đặt Memcached trên Ubuntu 22.04, chúng ta sẽ sử dụng APT, trình quản lý gói mặc định của Ubuntu.

```bash
sudo apt update
sudo apt install memcached -y
```

{{< figure src="/images/memcached-installation.png" alt="Cài đặt Memcached trên Ubuntu 22.04" caption="Cài đặt Memcached trên Ubuntu 22.04" >}}

Sau khi cài đặt xong, bạn có thể kiểm tra trạng thái của Memcached bằng cách sử dụng lệnh sau:

```bash
sudo systemctl status memcached
```

{{< figure src="/images/memcached-status.png" alt="Kiểm tra trạng thái của Memcached" caption="Kiểm tra trạng thái của Memcached" >}}

### Bảo mật Memcached

Mặc định, Memcached không có bất kỳ cơ chế bảo mật nào. Điều này có nghĩa là bất kỳ ai cũng có thể kết nối và thực hiện các thao tác trên Memcached server. Để bảo mật Memcached, chúng ta sẽ sử dụng một số cách sau:

#### Bật xác thực SASL

SASL (Simple Authentication and Security Layer) là một giao thức xác thực mạnh mẽ và linh hoạt. SASL cho phép Memcached xác thực người dùng bằng cách sử dụng tên người dùng và mật khẩu.

Đầu tiên, chúng ta cần cài đặt gói `libmemcached-tools` để sử dụng lệnh `memcstat`:

```bash
sudo apt install libmemcached-tools -y
```

{{< figure src="/images/libmemcached-tools-installation.png" alt="Cài đặt libmemcached-tools" caption="Cài đặt libmemcached-tools" >}}

Cuối cùng, chúng ta sẽ khởi động lại Memcached để áp dụng các thay đổi:

```bash
sudo systemctl restart memcached
```

#### Kiểm tra trạng thái của Memcached

Sau khi cài đặt xong, bạn có thể kiểm tra trạng thái của Memcached bằng cách sử dụng lệnh sau:

```bash
sudo systemctl status memcached
```

{{< figure src="/images/memcached-status.png" alt="Kiểm tra trạng thái của Memcached" caption="Kiểm tra trạng thái của Memcached" >}}

#### Bật xác thực IP

Một cách khác để bảo mật Memcached là bật xác thực IP. Điều này có nghĩa là chỉ những IP được chỉ định mới có thể kết nối và thực hiện các thao tác trên Memcached server.

Đầu tiên, chúng ta sẽ mở tệp tin cấu hình của Memcached:

```bash
sudo nano /etc/memcached.conf
```

Sau đó, chúng ta sẽ tìm và chỉnh sửa dòng sau:

```bash
# Specify which IP address to listen on. The default is to listen on all IP addresses
-l  127.0.0.1
```

Thay đổi `# Specify which IP address to listen on. The default is to listen on all IP addresses` thành `# Specify which IP address to listen on. The default is to listen on all IP addresses` và thêm IP của bạn vào sau `-l`:

```bash
# Specify which IP address to listen on. The default is to listen on all IP addresses
-l 0.0.0.0
```

{{< figure src="/images/memcached-configuration.png" alt="Chỉnh sửa tệp tin cấu hình của Memcached" caption="Chỉnh sửa tệp tin cấu hình của Memcached" >}}

Cuối cùng, chúng ta sẽ khởi động lại Memcached để áp dụng các thay đổi:

```bash
sudo systemctl restart memcached
```

{{< figure src="/images/memcached-restart.png" alt="Khởi động lại Memcached" caption="Khởi động lại Memcached" >}}

### Kết luận

Trong bài viết này, chúng ta đã tìm hiểu cách cài đặt và bảo mật Memcached trên Ubuntu 22.04. Memcached là một hệ thống caching đối tượng bộ nhớ phân tán hiệu suất cao, tổng quát trong tính chất, nhưng dự định để sử dụng trong việc tăng tốc ứng dụng web động bằng cách giảm tải cơ sở dữ liệu. Bằng cách bảo mật Memcached, chúng ta có thể ngăn chặn các cuộc tấn công từ bên ngoài và giữ cho Memcached server của chúng ta an toàn.
