---
categories: [database]
date: 2024-02-24T00:00:00.000Z
draft: false
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images: [/labs/postgresql/postgresql-pgpool.jpeg]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [database, postgresql, ubuntu, pgpool]
title: Lesson 1 -  Cài đặt Pgpool-II  trên Ubuntu 22.04
description: PGpool-II là một giải pháp trung gian độc đáo, được thiết kế đặc biệt để tối ưu hóa và mở rộng khả năng của hệ quản trị cơ sở dữ liệu PostgreSQL. Nó mang lại nhiều lợi ích như việc tối ưu hóa kết nối, phân phối tải đều và thực hiện sao chép dữ liệu, biến PGpool-II thành công cụ không thể thiếu trong quản lý các triển khai PostgreSQL. Trong hướng dẫn chi tiết này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.
weight: 1
---

# Pgpool-II là gì

PGpool-II là một giải pháp trung gian độc đáo, được thiết kế đặc biệt để tối ưu hóa và mở rộng khả năng của hệ quản trị cơ sở dữ liệu PostgreSQL. Nó mang lại nhiều lợi ích như việc tối ưu hóa kết nối, phân phối tải đều và thực hiện sao chép dữ liệu, biến PGpool-II thành công cụ không thể thiếu trong quản lý các triển khai PostgreSQL. Trong hướng dẫn chi tiết này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.

# Kiến trúc cài đặt

{{< figure src="./images/postgresql-pgpool.jpeg" >}}

Trước khi bắt đầu ta cần chuẩn bị 4 máy chủ

| IP            | Hostname    | vCPU   | RAM | DISK | OS           |
| ------------- | ----------- | ------ | --- | ---- | ------------ |
| 192.168.50.10 | pgpool2     | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.11 | pg-master   | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.12 | pg-slave-01 | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.13 | pg-slave-02 | 2 core | 4G  | 60G  | Ubuntu 22.04 |

### Cài đặt PostgreSQL Replication

[Cài đặt PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc) trên 3 máy chủ `pg-master` và `pg-slave-01`, `pg-slave-02`.

### Cài đặt PGpool-II 4.5.0

#### Bước 1: Cài đặt PGpool-II 4.5.0

##### Cài đặt make và gcc

\*\*\* Cần có GNU tạo phiên bản 3.80 hoặc mới hơn; các chương trình tạo khác hoặc các phiên bản tạo GNU cũ hơn sẽ không hoạt động. (GNU make đôi khi được cài đặt dưới tên gmake.) Để kiểm tra GNU, hãy nhập:

```bash
sudo apt update
sudo apt install -y make gcc libpq-dev
```

sau khi cài đặt kiểm tra phiên bản của `make`

```bash
make --version
```

{{< figure src="./images/make-version.jpg" >}}

Phiên bản `make` hiện tại đang là `4.3`

```bash
gcc --version
```

{{< figure src="./images/gcc-version.jpg" >}}

Phiên bản `gcc` hiện tại đang là `11.4.0`

##### Tải gói cài đặt PGpool-II

```bash
wget https://www.pgpool.net/mediawiki/download.php?f=pgpool-II-4.5.0.tar.gz -O pgpool-II-4.5.0.tar.gz
```

##### Giải nén và cài đặt

```bash
# giải nén
tar -xvf pgpool-II-4.5.0.tar.gz
cd pgpool-II-4.5.0
```

Tiếp tục ta cài đặt [memcached](/cai-dat-va-bao-mat-memcached-tren-ubuntu-22-04) và libmemcached-dev và libpq-dev

```bash
sudo apt install -y libmemcached-dev libpq-dev
```

Sau đó, chúng ta sẽ cấu hình và cài đặt PGpool-II bằng cách thực hiện các bước sau:

```bash
./configure --prefix=/home/pgpool2 --with-memcached=/usr/sbin/memcached 
make && sudo make install
```

Bạn có thể tùy chỉnh quá trình xây dựng và cài đặt bằng cách cung cấp một hoặc nhiều tùy chọn dòng lệnh sau để định cấu hình:

| Tùy chọn                 | Mô tả                                                     | Mặc định                 |
| ------------------------ | --------------------------------------------------------- | ------------------------ |
| `--prefix`               | Đường dẫn cài đặt PGpool-II                               | `/usr/local`             |
| `--with-pgsql`           | Thư mục cài đặt thư viện máy khách PostgreSQL             | Cung cấp bởi `pg_config` |
| `--with-openssl`         | Hỗ trợ OpenSSL (mã hóa mật khẩu AES256)                   | Tắt                      |
| `--enable-sequence-lock` | Khóa hàng trong bảng tuần tự (tương thích PGpool-II 3.0)  | Tắt                      |
| `--enable-table-lock`    | Khóa bảng mục tiêu chèn (tương thích PGpool-II 2.2 & 2.3) | Tắt                      |
| `--with-memcached=path`  | Sử dụng memcached cho bộ đệm truy vấn bộ nhớ              | Không sử dụng            |
| `--with-pam`             | Hỗ trợ xác thực PAM                                       | Tắt                      |

Sau khi cấu hình xong, chúng ta tiền hành tạo ln -la để tạo liên kết đến thư `/usr/sbin` 

```bash
sudo ln -s /home/pgpool2/bin/* /usr/sbin
```

Tiếp tục kiểm tra phiên bản của `pgpool` sau khi cài đặt thành công bằng lệnh sau:

```bash
pgpool --version
```

{{< figure src="./images/pgpool-version.jpg" >}}

#### Bước 2: Cấu hình PGpool-II

Tạo thư mục cấu hình cho PGpool-II:

```bash
sudo mkdir /etc/pgpool2
```

Sao chép từ config mẫu :

```bash
sudo cp /home/pgpool2/etc/pgpool.conf.sample /etc/pgpool2/pgpool.conf 
sudo cp /home/pgpool2/etc/pool_hba.conf.sample /etc/pgpool2/pool_hba.conf 
sudo cp /home/pgpool2/etc/pcp.conf.sample /etc/pgpool2/pcp.conf
```

#### Bước 3: Cấu hình quản lý kết nối

Tiếp theo, chúng ta sẽ cấu hình quản lý kết nối bằng cách chỉnh sửa tệp cấu hình `/etc/pgpool2/pool_hba.conf`:

```bash
sudo nano /etc/pgpool2/pool_hba.conf
```

Thêm cấu hình sau vào tệp:

```bash
host    all         all         0.0.0.0/0          trust
```

{{< figure src="./images/pgpool-pool-hba-config.jpg" >}}

#### Bước 4: Khởi chạy PGpool-II

Tạo thư mục chứa file pid:

```bash
sudo mkdir /var/run/pgpool
```

Cuối cùng, khởi chạy lại dịch vụ PGpool-II để áp dụng các thay đổi:

```bash
sudo /usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
```

{{< figure src="./images/pgpool-start.jpg" >}}

#### Bước 5: Cấu hình pcp.conf

\*\*Pgpool-II cung cấp một giao diện để người quản trị thực hiện các thao tác quản lý, chẳng hạn như xem trạng thái Pgpool-II hoặc tắt các tiến trình Pgpool-II từ xa. `pcp.conf` là tệp chứa người dùng/mật khẩu được sử dụng để xác thực cho giao diện này. Tất cả các chế độ hoạt động đều yêu cầu cài đặt tệp `pcp.conf`.

Tạo tệp `pcp.conf`:

```bash
password_hash=$(pg_md5 Ehi@123)
echo "pgpool:$password_hash" >> /etc/pgpool2/pcp.conf
```

Trong đó `akitect@123` là mật khẩu của tài khoản `pgpool`, bạn có thể thay đổi mật khẩu theo ý muốn.

Sau đó, thêm quyền truy cập vào tệp `pcp.conf`:

```bash
sudo chown pgpool:pgpool /etc/pgpool2/pcp.conf
sudo chmod 0600 /etc/pgpool2/pcp.conf
```

#### Bước 6: Tạo Service PGpool-II

Giống như bất kỳ tiến trình máy chủ (server daemon) nào có thể truy cập từ bên ngoài, bạn nên chạy Pgpool-II bằng một tài khoản người dùng riêng biệt. Tài khoản người dùng này chỉ nên sở hữu dữ liệu được quản lý bởi máy chủ và không được dùng chung với tiến trình khác. (Ví dụ, sử dụng tài khoảng 'nobody' là không tốt.) Cũng không nên cài đặt các tệp thực thi thuộc sở hữu của người dùng này bởi vì nếu hệ thống bị xâm phạm, kẻ tấn công có thể sửa đổi các tệp nhị phân đó.

Để thêm tài khoản người dùng Unix vào hệ thống, hãy tìm lệnh `useradd` hoặc `adduser`. Tên người dùng pgpool thường được sử dụng, và nó được mặc định sử dụng trong tài liệu, nhưng bạn có thể dùng tên khác nếu thích.

Tạo người dùng `pgpool`:

```bash
sudo useradd -s /bin/bash -m -d /home/pgpool -c "pgpool user" pgpool
```

Set quyền cho người dùng `pgpool`:

```bash
sudo chown -R pgpool:pgpool /etc/pgpool2
sudo chown -R pgpool:pgpool /var/run/pgpool
```

Tạo một tệp systemd service `pgpool2.service` trên máy chủ `pgpool2`:

```bash
sudo nano /etc/systemd/system/pgpool2.service
```

Thêm nội dung sau vào tệp `pgpool2.service`:

```bash
Description=pgpool-II
Documentation=man:pgpool(8)
Wants=postgresql.service
After=network.target

[Service]
User=pgpool
ExecStart=/usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf -m smart
ExecReload=/bin/kill -HUP $MAINPID
KillSignal=SIGINT
StandardOutput=syslog
SyslogFacility=local0

[Install]
WantedBy=multi-user.target
```

> `/usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf -m smart` trong đó : 

-   `-n` : Không chạy dưới dạng daemon
-   `-f` : Đường dẫn đến tệp cấu hình `pgpool.conf`
-   `-F` : Đường dẫn đến tệp cấu hình quản lý `pcp.conf`
-   `-m` : Chế độ hoạt động của Pgpool-II. Có 3 chế độ hoạt động:
    -   `fast` : Chế độ hoạt động nhanh
    -   `smart` : Chế độ hoạt động thông minh, mặc định

Tự động bật khi khởi động hệ thống:

```bash
sudo systemctl enable pgpool2
```

Khởi động lại dịch vụ PGpool-II:

```bash
sudo systemctl start pgpool2
```

Như vậy, chúng ta đã cài đặt và cấu hình PGpool-II trên hệ điều hành `Ubuntu Linux`. Bằng cách thực hiện các bước này, bạn có thể tối ưu hóa và mở rộng khả năng của cơ sở dữ liệu `PostgreSQL`, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.
