---
categories:
  - database
date: 2024-02-24T08:00:00+08:00
draft: false
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images:
  - /labs/postgresql/postgresql-pgpool.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - postgresql
  - ubuntu
  - pgpool
title: Lesson 1 -  Postgres Pgpool-II Ubuntu 
url: /pgpool-ii-ubuntu-cau-hinh-tung-buoc
description: PGpool-II là một giải pháp trung gian độc đáo, được thiết kế đặc biệt để tối ưu hóa và mở rộng khả năng của hệ quản trị cơ sở dữ liệu PostgreSQL. Nó mang lại nhiều lợi ích như việc tối ưu hóa kết nối, phân phối tải đều và thực hiện sao chép dữ liệu, biến PGpool-II thành công cụ không thể thiếu trong quản lý các triển khai PostgreSQL. Trong hướng dẫn chi tiết này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.
weight: 1
---

# Pgpool-II là gì 

PGpool-II là một giải pháp trung gian độc đáo, được thiết kế đặc biệt để tối ưu hóa và mở rộng khả năng của hệ quản trị cơ sở dữ liệu PostgreSQL. Nó mang lại nhiều lợi ích như việc tối ưu hóa kết nối, phân phối tải đều và thực hiện sao chép dữ liệu, biến PGpool-II thành công cụ không thể thiếu trong quản lý các triển khai PostgreSQL. Trong hướng dẫn chi tiết này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.

# Kiến trúc cài đặt

{{< figure src="./images/postgresql-pgpool.jpeg" >}}


Trước khi bắt đầu ta cần chuẩn bị 4 máy chủ

| IP           | Hostname             | vCPU   | RAM | DISK | OS           |
| ------------ | -------------------- | ------ | --- | ---- | ------------ |
| 192.168.56.5 | pgpool2              | 2 core | 4G  | 50G  | Ubuntu 22.04 |
| 192.168.56.2 | postgresql-master    | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave-01  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | postgresql-slave-02  | 4 core | 8G  | 50G  | Ubuntu 22.04 |

### Cài đặt PostgreSQL Replication 

[Cài đặt PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc) trên 3 máy chủ `postgresql-master` và `postgresql-slave-01`, `postgresql-slave-02`.

### Cài đặt PGpool-II 4.5.0

#### Bước 1: Cài đặt PGpool-II 4.5.0

##### Cài đặt make và gcc

*** Cần có GNU tạo phiên bản 3.80 hoặc mới hơn; các chương trình tạo khác hoặc các phiên bản tạo GNU cũ hơn sẽ không hoạt động. (GNU make đôi khi được cài đặt dưới tên gmake.) Để kiểm tra GNU, hãy nhập:

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

| Tùy chọn | Mô tả | Mặc định |
|---|---|---|
| `--prefix` | Đường dẫn cài đặt PGpool-II | `/usr/local` |
| `--with-pgsql` | Thư mục cài đặt thư viện máy khách PostgreSQL | Cung cấp bởi `pg_config` |
| `--with-openssl` | Hỗ trợ OpenSSL (mã hóa mật khẩu AES256) | Tắt |
| `--enable-sequence-lock` | Khóa hàng trong bảng tuần tự (tương thích PGpool-II 3.0) | Tắt |
| `--enable-table-lock` | Khóa bảng mục tiêu chèn (tương thích PGpool-II 2.2 & 2.3) | Tắt |
| `--with-memcached=path` | Sử dụng memcached cho bộ đệm truy vấn bộ nhớ | Không sử dụng |
| `--with-pam` | Hỗ trợ xác thực PAM | Tắt |


Sau khi cấu hình xong, chúng ta tiền hành tạo ln -la để tạo liên kết đến thư `/usr/sbin` 

```bash
sudo ln -s /home/pgpool2/bin/* /usr/sbin
```

Tiếp tục kiểm tra phiên bản của `pgpool` sau khi cài đặt thành công bằng lệnh sau:

```bash
pgpool --version
```

{{< figure src="./images/pgpool-version.jpg" >}}

#### Bước 2: Cài đặt `pgpool_recovery`

`PGpool-II` yêu cầu bao gồm các chức năng `pgpool_recovery`, `pgpool_remote_start` và `pgpool_switch_xlog` khi sử dụng khôi phục trực tuyến như được giải thích sau. Hơn nữa, công cụ quản lý pgpoolAdmin còn hỗ trợ khả năng dừng, khởi động lại hoặc tải lại phiên bản PostgreSQL trên màn hình bằng cách sử dụng `pgpool_pgctl`. Việc cài đặt các chức năng này trong template1 ban đầu là đủ, không cần thiết phải cài đặt chúng trong tất cả các cơ sở dữ liệu.

Trước tiên ta cần cài đặt `postgresql-server-dev-16`: 

##### 2.1: Thêm key và repo của PostgreSQL 16

```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
apt update
```

##### 2.2: Cài đặt `postgresql-server-dev-16`

```bash
sudo apt install -y postgresql-server-dev-16 -y
```

##### 2.3: Cài đặt `pgpool_recovery`

Tiếp theo, chúng ta sẽ cài đặt `pgpool_recovery` bằng cách thực hiện các bước sau:

```bash
# di chuyển đến thư mục pgpool-recovery
 cd pgpool-II-4.5.0/src/sql/pgpool-recovery

# cài đặt
make && sudo make install
```

Sau khi build thành công ta sẽ có file `pgpool_recovery.so` trong thư mục `pgpool-II-4.5.0/src/sql/pgpool-recovery`

```bash
ls -la
```

{{< figure src="./images/pgpool-recovery.jpg" >}}

##### 2.4: Cấu hình sao chép `pgpool_recovery` từ máy chủ `pgpool2` tới máy chủ `postgresql-master` bằng lệnh `scp`: 

Sao chép file `pgpool_recovery.so` và `pgpool_recovery.sql` từ máy chủ `pgpool2` tới máy chủ `postgresql-master` bằng lệnh `scp`:

  ```bash
  scp pgpool-recovery.so ubuntu@192.168.56.2:/home/ubuntu 
  scp pgpool-recovery.sql ubuntu@192.168.56.2:/home/ubuntu 
  scp pgpool_recovery.control ubuntu@192.168.56.2:/home/ubuntu 
  ```

Tiếp tục ssh vào máy chủ `postgresql-master`:

```bash
ssh ubuntu@192.168.56.2
```

Sao chép file `pgpool_recovery.so` và `pgpool_recovery.sql` vào thư mục `/usr/lib/postgresql/16/lib` và `/usr/share/postgresql/16/extension`:

```bash
sudo mv pgpool-recovery.so /usr/lib/postgresql/16/lib
sudo mv pgpool-recovery.sql /usr/share/postgresql/16/extension
sudo mv pgpool_recovery.control /usr/share/postgresql/16/extension
```

Sau khi cài đặt, chúng ta sẽ cấu hình `pgpool_recovery` ở máy chủ `postgresql-master` bằng cách thực hiện các bước sau:

```bash
 sudo -u postgres psql -d template1 -f /usr/share/postgresql/16/extension/pgpool-recovery.sql 
 ```

{{< figure src="./images/pgpool-recovery-sql.jpg" >}}

Trong đó `template1` là cơ sở dữ liệu mẫu mà chúng ta cài đặt `pgpool_recovery`

#### Bước 3: Cấu hình PGpool-II

Tạo thư mục cấu hình cho PGpool-II:

```bash
sudo mkdir /etc/pgpool2
```

Sao chép từ config mẫu :

````bash
sudo cp /home/pgpool2/etc/pgpool.conf.sample /etc/pgpool2/pgpool.conf 
sudo cp /home/pgpool2/etc/pool_hba.conf.sample /etc/pgpool2/pool_hba.conf 
sudo cp /home/pgpool2/etc/pcp.conf.sample /etc/pgpool2/pcp.conf
````

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

Cuối cùng, khởi chạy lại dịch vụ PGpool-II để áp dụng các thay đổi:

```bash
sudo /usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
```

{{< figure src="./images/pgpool-start.jpg" >}}

Như vậy, chúng ta đã cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux. Bằng cách thực hiện các bước này, bạn có thể tối ưu hóa và mở rộng khả năng của cơ sở dữ liệu PostgreSQL, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.