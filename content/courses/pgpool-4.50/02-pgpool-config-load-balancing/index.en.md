akitectio: 
> Để kết nối từ mạng ngoài trong các máy chủ trong mạng LAN của các máy chủ trong mạng, chúng ta cần cấu hình VPN Server để truy cập, ở bài này mình sẽ sử dụng Open VPN Server

## Khái niệm
* OpenVPN là SSL VPN đầy đủ tính năng (mạng riêng ảo). Nó thực hiện các tiện ích mở rộng mạng ASI Lớp 2 hoặc 3 bằng giao thức SSL/TLS. Nó là phần mềm nguồn mở và được phân phối theo GNU GPL. VPN cho phép bạn kết nối an toàn với mạng công cộng không an toàn như mạng WiFi tại sân bay hoặc khách sạn. VPN cũng được yêu cầu truy cập tài nguyên công ty, doanh nghiệp hoặc máy chủ gia đình của bạn. Bạn có thể bỏ qua trang web bị chặn địa lý và tăng quyền riêng tư hoặc an toàn trực tuyến. Hướng dẫn này cung cấp các hướng dẫn từng bước để định cấu hình máy chủ OpenVPN trên máy chủ Ubuntu Linux 22.04.

## Hướng dẫn

Để thực hiện cài đặt ta thực hiện các bước sau:

### Bước 1: Cập nhật hệ thống, hệ điều hành của bạn, ở đây mình sử dụng Ubuntu 22.04

Ta thực hiện update các gói bằng lệnh apt

```shell
sudo apt update && sudo apt upgrade -y
```
{{<figure src="./images/7f32aed5-2f3a-4396-8b09-3da3f714073f.webp" >}}

### Bước 2: Tìm và ghi nhận lại ip của bạn 

Để tìm ra ip của bạn ta sử dụng lệnh: 

```html
ip a
ip a show enp4s0
```

{{<figure src="./images/e0f12e13-e3ca-4aae-9333-0720b8e56d2c.webp" >}}


### Bước 3: Tải và chạy file  ubuntu-22.04-lts-vpn-server.sh script

```shell
wget https://raw.githubusercontent.com/Nyr/openvpn-install/master/openvpn-install.sh -O ubuntu-22.04-lts-vpn-server.sh
```

{{<figure src="./images/78e79b5b-c865-4532-8f96-8148504edba2.webp" >}}

sau đó ta set quyền cho tiệp mới tải về bằng lệnh 

```shell
chmod -v +x ubuntu-22.04-lts-vpn-server.sh
```

Chạy **ubuntu-22.04-lts-vpn-server.sh** để cài đặt  OpenVPN Server

```shell
sudo ./ubuntu-22.04-lts-vpn-server.sh
```

{{<figure src="./images/759f014d-721a-4a67-a41f-b0fce57005b3.webp" >}}
{{<figure src="./images/5a573a80-35f2-411c-bbf8-fb1837822fa7.webp" >}}


sau đó ta kiểm tra trạng thái của openVPN

```shell
sudo systemctl status openvpn-server@server
```
{{<figure src="./images/6f81e43a-4a69-41a8-b41c-6f4355e8e4f7.webp" >}}

Sau đó ta tải tiệp **client.ovpn** về máy cần vô VPN 

Chúng ta tìm tiệp .ovpn bằng lệnh 

```sql
sudo find / -iname "*.ovpn" -ls
```

Như vậy ta đã cài đặt thành cong OpenVPN

Tiếp theo ta cài đặt Open VPN Client cho máy cần vào VPN

1. [Apple IOS client ](https://apps.apple.com/us/app/openvpn-connect/id590379981)
2. [ Android client ](https://play.google.com/store/apps/details?id=net.openvpn.openvpn&hl=en)
3. [Apple MacOS client](https://openvpn.net/client-connect-vpn-for-mac-os/)

Ở đây mình sử dụng Mac M1 và kết nối thành công

{{<figure src="./images/ca0bfdb6-495f-4cb6-a333-2dbef13d098e.webp" >}}

### Link tham khảo
* https://www.cyberciti.biz/faq/ubuntu-22-04-lts-set-up-openvpn-server-in-5-minutes/

dịch sang tiếng anh

GitHub Copilot: > To connect from the external network to the servers in the LAN of the servers in the network, we need to configure the VPN Server for access, in this article, I will use Open VPN Server

## Concept
* OpenVPN is a full-featured SSL VPN (Virtual Private Network). It implements Layer 2 or 3 network extensions with the SSL/TLS protocol. It is open-source software and is distributed under the GNU GPL. VPN allows you to securely connect to an insecure public network such as WiFi at airports or hotels. VPN is also required to access your company, business, or home server resources. You can bypass geographically blocked websites and increase online privacy or security. This guide provides step-by-step instructions to configure an OpenVPN server on an Ubuntu Linux 22.04 server.

## Instructions

To perform the installation, follow these steps:

### Step 1: Update your system, your operating system, here I use Ubuntu 22.04

We update the packages with the apt command

```shell
sudo apt update && sudo apt upgrade -y
```
{{<figure src="./images/7f32aed5-2f3a-4396-8b09-3da3f714073f.webp" >}}

### Step 2: Find and record your IP 

To find your IP, we use the command: 

```html
ip a
ip a show enp4s0
```

{{<figure src="./images/e0f12e13-e3ca-4aae-9333-0720b8e56d2c.webp" >}}


### Step 3: Download and run the ubuntu-22.04-lts-vpn-server.sh script

```shell
wget https://raw.githubusercontent.com/Nyr/openvpn-install/master/openvpn-install.sh -O ubuntu-22.04-lts-vpn-server.sh
```

{{<figure src="./images/78e79b5b-c865-4532-8f96-8148504edba2.webp" >}}

then we set permissions for the newly downloaded file with the command 

```shell
chmod -v +x ubuntu-22.04-lts-vpn-server.sh
```

Run **ubuntu-22.04-lts-vpn-server.sh** to install OpenVPN Server

```shell
sudo ./ubuntu-22.04-lts-vpn-server.sh
```

{{<figure src="./images/759f014d-721a-4a67-a41f-b0fce57005b3.webp" >}}
{{<figure src="./images/5a573a80-35f2-411c-bbf8-fb1837822fa7.webp" >}}


then we check the status of openVPN

```shell
sudo systemctl status openvpn-server@server
``}
{{<figure src="./images/6f81e43a-4a69-41a8-b41c-6f4355e8e4f7.webp" >}}

Then we download the **client.ovpn** file to the machine that needs to go into VPN 

We find the .ovpn file with the command 

```sql
sudo find / -iname "*.ovpn" -ls
```

So we have successfully installed OpenVPN

Next, we install the Open VPN Client for the machine that needs to go into VPN

1. [Apple IOS client ](https://apps.apple.com/us/app/openvpn-connect/id590379981)
2. [ Android client ](https://play.google.com/store/apps/details?id=net.openvpn.openvpn&hl=en)
3. [Apple MacOS client](https://openvpn.net/client-connect-vpn-for-mac-os/)

Here I use Mac M1 and successfully connect

{{<figure src="./images/ca0bfdb6-495f-4cb6-a333-2dbef13d098e.webp" >}}

### Reference Links
* https://www.cyberciti.biz/faq/ubuntu-22-04-lts-set-up-openvpn-server-in-5-minutes/

akitectio: 
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

### Cài đặt PGpool-II

#### Bước 1: Cài đặt PGpool-II

Đầu tiên, cài đặt PGpool-II trên máy chủ `pgpool2` bằng cách thực hiện các bước sau:

```bash
sudo apt update
sudo apt install -y pgpool2
```

#### Bước 2: Cấu hình PGpool-II

Sau khi cài đặt xong, chúng ta sẽ cấu hình PGpool-II bằng cách chỉnh sửa tệp cấu hình `/etc/pgpool2/pgpool.conf`:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```

Thêm cấu hình sau vào tệp:

```bash
listen_addresses = '*'
port = 5432 

# only write queries are load balanced
backend_hostname0 = '192.168.56.2' 
backend_port0 = 5432
backend_weight0 = 2  
backend_data_directory0 = '/var/lib/postgresql/16/main' 
backend_application_name0 = 'postgresql-master'

# only read queries are load balanced
backend_hostname1 = '192.168.56.3'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/var/lib/postgresql/16/main' 
backend_application_name0 = 'postgresql-slave-01'

backend_hostname2 = '192.168.56.4'
backend_port2 = 5432
backend_weight2 = 1
backend_data_directory2 = '/var/lib/postgresql/16/main'
backend_application_name0 = 'postgresql-slave-02'

load_balance_mode = on
master_slave_mode = on 
master_slave_sub_mode = 'stream'
```

Trong đó: 

- `listen_addresses`: Địa chỉ IP mà PGpool-II sẽ lắng nghe các kết nối đến.
- `port`: Cổng mà PGpool-II sẽ lắng nghe các kết nối đến.
- `backend_hostname0`: Địa chỉ IP của máy chủ PostgreSQL master.
- `backend_port0`: Cổng của máy chủ PostgreSQL master.
- `backend_weight0`: Trọng số của máy chủ PostgreSQL master.
- `backend_data_directory0`: Đường dẫn đến thư mục dữ liệu của máy chủ PostgreSQL master.
- `backend_hostname1`: Địa chỉ IP của máy chủ PostgreSQL slave 1.
- `backend_port1`: Cổng của máy chủ PostgreSQL slave 1.
- `backend_weight1`: Trọng số của máy chủ PostgreSQL slave 1.
- `backend_data_directory1`: Đường dẫn đến thư mục dữ liệu của máy chủ PostgreSQL slave 1.
- `backend_hostname2`: Địa chỉ IP của máy chủ PostgreSQL slave 2.
- `backend_port2`: Cổng của máy chủ PostgreSQL slave 2.
- `backend_weight2`: Trọng số của máy chủ PostgreSQL slave 2.
- `backend_data_directory2`: Đường dẫn đến thư mục dữ liệu của máy chủ PostgreSQL slave 2.
- `load_balance_mode`: Chế độ phân phối tải đều.
- `master_slave_mode`: Chế độ master/slave.
- `master_slave_sub_mode`: Chế độ sao chép dữ liệu.

#### Bước 3: Cấu hình quản lý kết nối

Tiếp theo, chúng ta sẽ cấu hình quản lý kết nối bằng cách chỉnh sửa tệp cấu hình `/etc/pgpool2/pool_hba.conf`:

```bash
sudo nano /etc/pgpool2/pool_hba.conf
```

Thêm cấu hình sau vào tệp:

```bash
host    all         all         0.0.0.0/0          trust
```

#### Bước 4: Khởi động PGpool-II

Cuối cùng, khởi động lại dịch vụ PGpool-II để áp dụng các thay đổi:

```bash
sudo systemctl restart pgpool2
```

Để đảm bảo PGpool-II tự động khởi động khi khởi động hệ thống, bạn có thể kích hoạt dịch vụ PGpool-II bằng lệnh sau:

```bash
sudo systemctl enable pgpool2
```

#### Bước 5: Test kết nối, ta dùng pgadmin để kết nối đến pgpool2

Thông tin đăng nhập như sau:

`Host`: 192.168.56.5
`Port`: 5432
`Username`: postgres
`Password`: ở bước cài đặt PostgreSQL Replication

{{< figure src="./images/pgpool-pgadmin.jpg" >}}

Như vậy ta đã cài đặt và cấu hình PGpool-II thành công.

### Kết luận
Trong hướng dẫn này, chúng ta đã đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux. Bằng cách thực hiện các bước này, bạn có thể tối ưu hóa và mở rộng khả năng của cơ sở dữ liệu PostgreSQL, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.

dịch sang tiếng anh

GitHub Copilot: # What is Pgpool-II

Pgpool-II is a unique middleware solution, specially designed to optimize and scale the capabilities of the PostgreSQL database management system. It offers numerous benefits such as optimizing connections, load balancing, and performing data replication, making Pgpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure Pgpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.

# Installation Architecture

{{< figure src="./images/postgresql-pgpool.jpeg" >}}


Before starting, we need to prepare 4 servers

| IP           | Hostname             | vCPU   | RAM | DISK | OS           |
| ------------ | -------------------- | ------ | --- | ---- | ------------ |
| 192.168.56.5 | pgpool2              | 2 core | 4G  | 50G  | Ubuntu 22.04 |
| 192.168.56.2 | postgresql-master    | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave-01  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | postgresql-slave-02  | 4 core | 8G  | 50G  | Ubuntu 22.04 |

### Install PostgreSQL Replication 

[Install PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc) on 3 servers `postgresql-master` and `postgresql-slave-01`, `postgresql-slave-02`.

### Install PGpool-II

#### Step 1: Install PGpool-II

First, install PGpool-II on the `pgpool2` server by performing the following steps:

```bash
sudo apt update
sudo apt install -y pgpool2
```

#### Step 2: Configure PGpool-II

After the installation is complete, we will configure PGpool-II by editing the configuration file `/etc/pgpool2/pgpool.conf`:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```

Add the following configuration to the file:

```bash
listen_addresses = '*'
port = 5432 

# only write queries are load balanced
backend_hostname0 = '192.168.56.2' 
backend_port0 = 5432
backend_weight0 = 2  
backend_data_directory0 = '/var/lib/postgresql/16/main' 
backend_application_name0 = 'postgresql-master'

# only read queries are load balanced
backend_hostname1 = '192.168.56.3'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/var/lib/postgresql/16/main' 
backend_application_name0 = 'postgresql-slave-01'

backend_hostname2 = '192.168.56.4'
backend_port2 = 5432
backend_weight2 = 1
backend_data_directory2 = '/var/lib/postgresql/16/main'
backend_application_name0 = 'postgresql-slave-02'

load_balance_mode = on
master_slave_mode = on 
master_slave_sub_mode = 'stream'
```

Where: 

- `listen_addresses`: The IP address that PGpool-II will listen for incoming connections.
- `port`: The port that PGpool-II will listen for incoming connections.
- `backend_hostname0`: The IP address of the PostgreSQL master server.
- `backend_port0`: The port of the PostgreSQL master server.
- `backend_weight0`: The weight of the PostgreSQL master server.
- `backend_data_directory0`: The path to the data directory of the PostgreSQL master server.
- `backend_hostname1`: The IP address of the PostgreSQL slave 1 server.
- `backend_port1`: The port of the PostgreSQL slave 1 server.
- `backend_weight1`: The weight of the PostgreSQL slave 1 server.
- `backend_data_directory1`: The path to the data directory of the PostgreSQL slave 1 server.
- `backend_hostname2`: The IP address of the PostgreSQL slave 2 server.
- `backend_port2`: The port of the PostgreSQL slave 2 server.
- `backend_weight2`: The weight of the PostgreSQL slave 2 server.
- `backend_data_directory2`: The path to the data directory of the PostgreSQL slave 2 server.
- `load_balance_mode`: Load balancing mode.
- `master_slave_mode`: Master/slave mode.
- `master_slave_sub_mode`: Data replication mode.

#### Step 3: Configure Connection Management

Next, we will configure connection management by editing the configuration file `/etc/pgpool2/pool_hba.conf`:

```bash
sudo nano /etc/pgpool2/pool_hba.conf
```

Add the following configuration to the file:

```bash
host    all         all         0.0.0.0/0          trust
```

#### Step 4: Start PGpool-II

Finally, restart the PGpool-II service to apply the changes:

```bash
sudo systemctl restart pgpool2
```

To ensure PGpool-II automatically starts when the system boots, you can enable the PGpool-II service with the following command:

```bash
sudo systemctl enable pgpool2
```

#### Step 5: Test the connection, we use pgadmin to connect to pgpool2

Login information as follows:

`Host`: 192.168.56.5
`Port`: 5432
`Username`: postgres
`Password`: at the PostgreSQL Replication installation step

{{< figure src="./images/pgpool-pgadmin.jpg" >}}

So we have successfully installed and configured PGpool-II.

### Conclusion
In this guide, we went through the steps to install and configure PGpool-II on the Ubuntu Linux operating system. By performing these steps, you can optimize and expand the capabilities of your PostgreSQL database, helping you to maximize the performance and high availability of your database.

akitectio: Lấy 12 bài viết mới nhất,filter theo ngày

GitHub Copilot: Để lấy 12 bài viết mới nhất và sắp xếp theo ngày, bạn có thể sử dụng hàm `first` kết hợp với hàm `where` và `sort` trong Hugo. Dưới đây là cách bạn có thể thực hiện điều này:

```html
{{ range first 12 (where .Site.RegularPages "Type" "post" | sort "Date" "desc") }}
  <div class="col-md-3">
    <div class="card mb-4 shadow-sm">
      <div class="card-body">
        <a href="{{ .Permalink }}">
          <img
            src="{{ .Params.featuredImage }}"
            class="card-img-top-home"
            alt="{{ .Title }}"
          />
        </a>
        <h5 class="card-title mt-1">
          <a class="link-a" href="{{ .Permalink }}">{{ .Title }} </a>
        </h5>
        <p class="card-text limited-text">{{ .Description }}</p>
        <div class="tag">
          {{ range first 2 .Params.tags }} {{ $tagPage := $.Site.GetPage
          (printf "/tags/%s" .) }}
          <a href="{{ $.Page.Site.BaseURL }}tags/{{ . }}">
            <span
              class="badge bg-secondary"
              style="border: 1px solid;color: {{ $tagPage.Params.color }}!important; background-color: {{ $tagPage.Params.background }}!important;"
            >
              #{{ . }}
            </span>
          </a>
          {{ end }}
        </div>
      </div>
    </div>
  </div>
{{ end }}
```



akitectio:  render: failed to render pages: render of "home" failed: execute of template failed at <sort "Date" "desc">: error calling sort: can’t sort string 


GitHub Copilot: The error message indicates that the `sort` function is trying to sort a string, which is not possible. The `sort` function in Hugo is used to sort slices.

In your code, you're trying to sort pages by date in descending order:

```html
{{ range first 12 (where .Site.RegularPages "Type" "post" | sort "Date" "desc") }}
```

The error might be due to the fact that the "Date" field is not correctly recognized as a date or time type, but as a string. 

Ensure that the "Date" field in your content files is in the correct format. In Hugo, dates should be in the format `YYYY-MM-DD` or as a full timestamp `YYYY-MM-DDTHH:MM:SSZ`.

If the "Date" field is correct and the error still persists, try to debug by printing out the "Date" field of your pages to see what it returns:

```html
{{ range first 12 (where .Site.RegularPages "Type" "post") }}
    {{ .Date }}
{{ end }}
```

This will help you identify if there's an issue with the date format in your content files.

akitectio:  render: failed to render pages: render of "home" failed: [1;36m"/Users/akitect.io/Codes/duydev/akitect.io-new/layouts/index.html:76:29"[0m: execute of template failed at <.Site.RegularPages>: can’t give argument to non-function sort "Date" "desc" 


GitHub Copilot: The error message indicates that the `sort` function is not being recognized as a function, which is likely due to incorrect syntax or placement in your code.

In Hugo, the `sort` function is used to sort a slice of pages. The correct syntax to sort pages by date in descending order is:

```html
{{ range sort .Site.RegularPages "Date" "desc" }}
```

However, from the error message, it seems like you're trying to use `sort` in this way:

```html
{{ range .Site.RegularPages | sort "Date" "desc" }}
```

This is incorrect because the pipe operator (`|`) is used to pass the output of one function as the input to another function. In this case, `.Site.RegularPages` is not a function, so it can't be piped into `sort`.

To fix the error, you should change your code to use the correct syntax for `sort`. Here's an example:

```html
{{ range sort .Site.RegularPages "Date" "desc" }}
    <!-- Your code here -->
{{ end }}
```

This will sort all the pages on your site by date in descending order.

akitectio: ---
categories:
  - database
date: 2024-02-25T08:00:00+08:00
draft: false
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images:
  - /labs/postgresql/postgresql-pgpool.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - postgreSQL
  - ubuntu
  - pgpool
title: Lesson 6 - Pgpool Admin Ubuntu 
url: /cai-dat-va-cau-hinh-pgpool-admin-tren-ubuntu
description: Pgpool Admin là một công cụ quản lý cấu hình và giám sát PGpool-II, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu PostgreSQL. Trong hướng dẫn này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool Admin trên hệ điều hành Ubuntu Linux, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu của mình.
weight: 6
---

## Pgpool Admin là gì

Pgpool Admin là một công cụ quản lý cấu hình và giám sát PGpool-II, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu PostgreSQL. Trong hướng dẫn này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool Admin trên hệ điều hành Ubuntu Linux, giúp bạn quản lý và theo dõi hiệu suất của cơ sở dữ liệu của mình.


### Cài đặt PGpool Admin trên Ubuntu

#### Bước 1: Cài đặt apache2 và php

Thêm repository php

```bash
sudo apt-add-repository ppa:ondrej/php
sudo apt update
```

Đầu tiên, cài đặt Apache2 và PHP trên máy chủ `pgpool2` bằng cách thực hiện các bước sau:

```bash
sudo apt update
sudo apt install -y apache2 php sudo apt install php7.4 libapache2-mod-php7.4 php7.4-cli php7.4-common php7.4-pgsql 
```

Sau khi cài đặt xong, kiểm tra phiên bản PHP:

```bash
php -v
```

{{< figure src="./images/php-version.jpg" >}}

#### Bước 2: Tải PGpool Admin 4.2.0

Đầu tiên, tải PGpool Admin từ trang web chính thức của dự án PGpool-II:

```bash
wget https://www.pgpool.net/mediawiki/download.php?f=pgpoolAdmin-4.2.0.tar.gz -O pgpoolAdmin-4.2.0.tar.gz
```

Sau khi tải xong, giải nén tệp tải về:

```bash
tar -xvf pgpoolAdmin-4.2.0.tar.gz
```

Di chuyển tệp giải nén vào thư mục `/var/www/html`:

```bash
sudo mv pgpoolAdmin-4.2.0 /var/www/html/pgpooladmin
```

sau khi giải nén xong, ta cần phải cấu hình lại quyền truy cập cho thư mục `pgpooladmin`:

```bash
sudo chown -R www-data:www-data /var/www/html/pgpooladmin
```

{{< figure src="./images/pgpooladmin-directory.jpg" >}}

#### Bước 3: Cấu hình apache2

Tiếp theo, cấu hình Apache2 để chạy PGpool Admin bằng cách tạo một tệp cấu hình mới:

```bash
sudo nano /etc/apache2/sites-available/pgpooladmin.conf
```

Thêm cấu hình sau vào tệp:

```bash
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/pgpooladmin
    <Directory /var/www/html/pgpooladmin>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

{{< figure src="./images/pgpooladmin-apache2.jpg" >}}

Sau khi cấu hình xong, lưu và đóng tệp cấu hình.

#### Kích hoạt cấu hình

Tiếp theo, kích hoạt tệp cấu hình mới bằng lệnh sau:

```bash
sudo a2ensite pgpooladmin.conf
```

Cuối cùng, khởi động lại dịch vụ Apache2 để áp dụng các thay đổi:

```bash
sudo systemctl restart apache2
```

### Bước 4: Cấu hình PGpool-II

Tạo password pgpooladmin

```bash
pg_md5 akitect@123
```

{{< figure src="./images/pg_md5.jpg" >}}

Tiếp theo, chúng ta sẽ cấu hình PGpool-II bằng cách chỉnh sửa tệp cấu hình `/etc/pgpool2/pgpool.conf`:

```bash
sudo nano /etc/pgpool2/pcp.conf
```

{{< figure src="./images/pgpool-pcp.jpg" >}}

trong đó `admin` là user mà bạn muốn sử dụng để đăng nhập vào pgpooladmin, `md5` là mật khẩu mà bạn đã tạo ở trên.

Tiếp theo tạo và gán quyền cho tệp `.pcppass`:

```bash
# Tạo tệp .pcppass
touch /var/www/.pcppass

# Gán quyền cho tệp .pcppass
chmod -R 0600 /var/www/.pcppass 
```

### Bước 5: Truy cập PGpool Admin và cấu hình ban đầu

Sau khi cài đặt xong, truy cập PGpool Admin bằng cách mở trình duyệt web và truy cập địa chỉ IP của máy chủ `pgpool2` : [http://192.168.56.5/pgpooladmin/install](http://192.168.56.5/pgpooladmin/install)

{{< figure src="./images/pgpooladmin-install.jpg" >}}

chọn ngôn ngữ và nhấn `Next`

{{< figure src="./images/pgpooladmin-install-2.jpg" >}}

Tiếp tục thêm thông tin cấu hình kết nối đến `pgpool2`

{{< figure src="./images/pgpooladmin-install-3.jpg" >}}

- `pgpool.conf` nhập `/etc/pgpool2/pgpool.conf`
- `pcp.conf` nhập `/etc/pgpool2/pcp.conf`
- `pgpool Command` nhập `/usr/sbin/pgpool`
- `PCP directory` nhập `/usr/sbin`

Tiếp tục nhấn `Next`

{{< figure src="./images/pgpooladmin-install-4.jpg" >}}

Như vậy đã setup ban đầu xong, ta xoá thư mục `install` để bảo mật hơn.

```bash
sudo rm -rf /var/www/html/pgpooladmin/install
```

{{< figure src="./images/pgpooladmin-install-5.jpg" >}}

### Bước 6: Đăng nhập vào PGpool Admin

Sau khi cài đặt xong, truy cập PGpool Admin bằng cách mở trình duyệt web và truy cập địa chỉ IP của máy chủ `pgpool2` : [http://192.168.56.5/pgpooladmin](http://192.168.56.5/pgpooladmin)

{{< figure src="./images/pgpooladmin-install-6.jpg" >}}

nhập thông tin đăng nhập đã tạo ở trên và nhấn `Login`

- `Username`: admin
- `Password`: akitect@123 <- mật khẩu đã tạo ở trên

{{< figure src="./images/pgpooladmin-install-7.jpg" >}}

Như vậy ta đã cấu hình và đăng nhập thành công vào PGpool Admin.

### Kết luận
Trong hướng dẫn này, chúng ta đã đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux. Bằng cách thực hiện các bước này, bạn có thể tối ưu hóa và mở rộng khả năng của cơ sở dữ liệu PostgreSQL, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.


dịch sang tiếng anh

GitHub Copilot: ---
categories:
  - database
date: 2024-02-25T08:00:00+08:00
draft: false
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images:
  - /labs/postgresql/postgresql-pgpool.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - postgreSQL
  - ubuntu
  - pgpool
title: Lesson 6 - Pgpool Admin Ubuntu 
url: /install-and-configure-pgpool-admin-on-ubuntu
description: Pgpool Admin is a configuration management and monitoring tool for PGpool-II, helping you manage and monitor the performance of your PostgreSQL database. In this guide, we will go through the steps to install and configure Pgpool Admin on Ubuntu Linux, helping you manage and monitor the performance of your database.
weight: 6
---

## What is Pgpool Admin

Pgpool Admin is a configuration management and monitoring tool for PGpool-II, helping you manage and monitor the performance of your PostgreSQL database. In this guide, we will go through the steps to install and configure Pgpool Admin on Ubuntu Linux, helping you manage and monitor the performance of your database.


### Installing Pgpool Admin on Ubuntu

#### Step 1: Install apache2 and php

Add php repository

```bash
sudo apt-add-repository ppa:ondrej/php
sudo apt update
```

First, install Apache2 and PHP on the `pgpool2` server by performing the following steps:

```bash
sudo apt update
sudo apt install -y apache2 php sudo apt install php7.4 libapache2-mod-php7.4 php7.4-cli php7.4-common php7.4-pgsql 
```

After the installation is complete, check the PHP version:

```bash
php -v
```

{{< figure src="./images/php-version.jpg" >}}

#### Step 2: Download PGpool Admin 4.2.0

First, download PGpool Admin from the official website of the PGpool-II project:

```bash
wget https://www.pgpool.net/mediawiki/download.php?f=pgpoolAdmin-4.2.0.tar.gz -O pgpoolAdmin-4.2.0.tar.gz
```

After the download is complete, extract the downloaded file:

```bash
tar -xvf pgpoolAdmin-4.2.0.tar.gz
```

Move the extracted file to the `/var/www/html` directory:

```bash
sudo mv pgpoolAdmin-4.2.0 /var/www/html/pgpooladmin
```

after extracting, we need to reconfigure the access rights for the `pgpooladmin` directory:

```bash
sudo chown -R www-data:www-data /var/www/html/pgpooladmin
```

{{< figure src="./images/pgpooladmin-directory.jpg" >}}

#### Step 3: Configure apache2

Next, configure Apache2 to run PGpool Admin by creating a new configuration file:

```bash
sudo nano /etc/apache2/sites-available/pgpooladmin.conf
```

Add the following configuration to the file:

```bash
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/pgpooladmin
    <Directory /var/www/html/pgpooladmin>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

{{< figure src="./images/pgpooladmin-apache2.jpg" >}}

After the configuration is complete, save and close the configuration file.

#### Activate the configuration

Next, activate the new configuration file with the following command:

```bash
sudo a2ensite pgpooladmin.conf
```

Finally, restart the Apache2 service to apply the changes:

```bash
sudo systemctl restart apache2
```

### Step 4: Configure PGpool-II

Create pgpooladmin password

```bash
pg_md5 akitect@123
```

{{< figure src="./images/pg_md5.jpg" >}}

Next, we will configure PGpool-II by editing the configuration file `/etc/pgpool2/pgpool.conf`:

```bash
sudo nano /etc/pgpool2/pcp.conf
```

{{< figure src="./images/pgpool-pcp.jpg" >}}

where `admin` is the user you want to use to log in to pgpooladmin, `md5` is the password you created above.

Next, create and assign rights to the `.pcppass` file:

```bash
# Create .pcppass file
touch /var/www/.pcppass

# Assign rights to the .pcppass file
chmod -R 0600 /var/www/.pcppass 
```

### Step 5: Access PGpool Admin and initial configuration

After the installation is complete, access PGpool Admin by opening a web browser and accessing the IP address of the `pgpool2` server: [http://192.168.56.5/pgpooladmin/install](http://192.168.56.5/pgpooladmin/install)

{{< figure src="./images/pgpooladmin-install.jpg" >}}

select the language and press `Next`

{{< figure src="./images/pgpooladmin-install-2.jpg" >}}

Continue to add connection configuration information to `pgpool2`

{{< figure src="./images/pgpooladmin-install-3.jpg" >}}

- `pgpool.conf` enter `/etc/pgpool2/pgpool.conf`
- `pcp.conf` enter `/etc/pgpool2/pcp.conf`
- `pgpool Command` enter `/usr/sbin/pgpool`
- `PCP directory` enter `/usr/sbin`

Continue to press `Next`

{{< figure src="./images/pgpooladmin-install-4.jpg" >}}

So the initial setup is complete, we delete the `install` directory for better security.

```bash
sudo rm -rf /var/www/html/pgpooladmin/install
```

{{< figure src="./images/pgpooladmin-install-5.jpg" >}}

### Step 6: Log in to PGpool Admin

After the installation is complete, access PGpool Admin by opening a web browser and accessing the IP address of the `pgpool2` server: [http://192.168.56.5/pgpooladmin](http://192.168.56.5/pgpooladmin)

{{< figure src="./images/pgpooladmin-install-6.jpg" >}}

enter the login information created above and press `Login`

- `Username`: admin
- `Password`: akitect@123 <- password created above

{{< figure src="./images/pgpooladmin-install-7.jpg" >}}

So we have successfully configured and logged in to PGpool Admin.

### Conclusion
In this guide, we went through the steps to install and configure PGpool-II on Ubuntu Linux. By performing these steps, you can optimize and expand the capabilities of your PostgreSQL database, helping you maximize the performance and high availability of your database.

akitectio: ---
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
title: Lesson 5 -  Postgres Pgpool-II Ubuntu 
url: /pgpool-ii-ubuntu-cau-hinh-tung-buoc
description: PGpool-II là một giải pháp trung gian độc đáo, được thiết kế đặc biệt để tối ưu hóa và mở rộng khả năng của hệ quản trị cơ sở dữ liệu PostgreSQL. Nó mang lại nhiều lợi ích như việc tối ưu hóa kết nối, phân phối tải đều và thực hiện sao chép dữ liệu, biến PGpool-II thành công cụ không thể thiếu trong quản lý các triển khai PostgreSQL. Trong hướng dẫn chi tiết này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.
weight: 5
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

dịch sang tiếng anh

GitHub Copilot: ---
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
title: Lesson 5 -  Postgres Pgpool-II Ubuntu 
url: /pgpool-ii-ubuntu-step-by-step-configuration
description: PGpool-II is a unique middleware solution, specially designed to optimize and scale the capabilities of the PostgreSQL database management system. It brings many benefits such as optimizing connections, load balancing, and performing data replication, making PGpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure PGpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.
weight: 5
---

# What is Pgpool-II 

PGpool-II is a unique middleware solution, specially designed to optimize and scale the capabilities of the PostgreSQL database management system. It brings many benefits such as optimizing connections, load balancing, and performing data replication, making PGpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure PGpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.

# Installation Architecture

{{< figure src="./images/postgresql-pgpool.jpeg" >}}


Before we start, we need to prepare 4 servers

| IP           | Hostname             | vCPU   | RAM | DISK | OS           |
| ------------ | -------------------- | ------ | --- | ---- | ------------ |
| 192.168.56.5 | pgpool2              | 2 core | 4G  | 50G  | Ubuntu 22.04 |
| 192.168.56.2 | postgresql-master    | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave-01  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | postgresql-slave-02  | 4 core | 8G  | 50G  | Ubuntu 22.04 |

### Install PostgreSQL Replication 

[Install PostgreSQL 16 Replication](/setting-up-postgresql-replication-step-by-step-guide) on 3 servers `postgresql-master` and `postgresql-slave-01`, `postgresql-slave-02`.

### Install PGpool-II 4.5.0

#### Step 1: Install PGpool-II 4.5.0

##### Install make and gcc

*** GNU make version 3.80 or newer is required; other make programs or older GNU make versions will not work. (GNU make is sometimes installed under the name gmake.) To check GNU, enter:

```bash
sudo apt update
sudo apt install -y make gcc libpq-dev
```

after installing check the version of `make`

```bash
make --version
```

{{< figure src="./images/make-version.jpg" >}}

The current version of `make` is `4.3`

```bash
gcc --version
```

{{< figure src="./images/gcc-version.jpg" >}}

The current version of `gcc` is `11.4.0`

##### Download the PGpool-II installation package

```bash
wget https://www.pgpool.net/mediawiki/download.php?f=pgpool-II-4.5.0.tar.gz -O pgpool-II-4.5.0.tar.gz
```


##### Extract and install

```bash
# extract
tar -xvf pgpool-II-4.5.0.tar.gz
cd pgpool-II-4.5.0
```

Next, we install [memcached](/install-and-secure-memcached-on-ubuntu-22-04) and libmemcached-dev and libpq-dev

```bash
sudo apt install -y libmemcached-dev libpq-dev
```

Then, we will configure and install PGpool-II by performing the following steps:

```bash
./configure --prefix=/home/pgpool2 --with-memcached=/usr/sbin/memcached 
make && sudo make install
```


You can customize the build and installation process by providing one or more of the following command line options to configure:

| Option | Description | Default |
|---|---|---|
| `--prefix` | PGpool-II installation path | `/usr/local` |
| `--with-pgsql` | Directory of installed PostgreSQL client libraries | Provided by `pg_config` |
| `--with-openssl` | Support for OpenSSL (AES256 password encryption) | Off |
| `--enable-sequence-lock` | Lock rows in the sequence table (compatible with PGpool-II 3.0) | Off |
| `--enable-table-lock` | Lock the target insert table (compatible with PGpool-II 2.2 & 2.3) | Off |
| `--with-memcached=path` | Use memcached for memory query cache | Not used |
| `--with-pam` | Support for PAM authentication | Off |


After configuring, we proceed to create ln -la to create a link to the `/usr/sbin` directory 

```bash
sudo ln -s /home/pgpool2/bin/* /usr/sbin
```

Continue to check the version of `pgpool` after successful installation with the following command:

```bash
pgpool --version
```

{{< figure src="./images/pgpool-version.jpg" >}}

#### Step 2: Install `pgpool_recovery`

`PGpool-II` requires including the functions `pgpool_recovery`, `pgpool_remote_start` and `pgpool_switch_xlog` when using online recovery as explained later. Moreover, the pgpoolAdmin management tool also supports the ability to stop, restart or reload PostgreSQL versions on the screen using `pgpool_pgctl`. Installing these functions in the initial template1 is enough, there is no need to install them in all databases.

First we need to install `postgresql-server-dev-16`: 

##### 2.1: Add key and repo of PostgreSQL 16

```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
apt update
```

##### 2.2: Install `postgresql-server-dev-16`

```bash
sudo apt install -y postgresql-server-dev-16 -y
```

##### 2.3: Install `pgpool_recovery`

Next, we will install `pgpool_recovery` by performing the following steps:

```bash
# move to the pgpool-recovery directory
 cd pgpool-II-4.5.0/src/sql/pgpool-recovery

# install
make && sudo make install
```

After successful build we will have the file `pgpool_recovery.so` in the directory `pgpool-II-4.5.0/src/sql/pgpool-recovery`

```bash
ls -la
```

{{< figure src="./images/pgpool-recovery.jpg" >}}

##### 2.4: Configure `pgpool_recovery` replication from `pgpool2` server to `postgresql-master` server using `scp`: 

Copy the `pgpool_recovery.so` and `pgpool_recovery.sql` files from the `pgpool2` server to the `postgresql-master` server using the `scp` command:

  ```bash
  scp pgpool-recovery.so ubuntu@192.168.56.2:/home/ubuntu 
  scp pgpool-recovery.sql ubuntu@192.168.56.2:/home/ubuntu 
  scp pgpool_recovery.control ubuntu@192.168.56.2:/home/ubuntu 
  ```

Continue to ssh into the `postgresql-master` server:

```bash
ssh ubuntu@192.168.56.2
```

Copy the `pgpool_recovery.so` and `pgpool_recovery.sql` files to the `/usr/lib/postgresql/16/lib` and `/usr/share/postgresql/16/extension` directories:

```bash
sudo mv pgpool-recovery.so /usr/lib/postgresql/16/lib
sudo mv pgpool-recovery.sql /usr/share/postgresql/16/extension
sudo mv pgpool_recovery.control /usr/share/postgresql/16/extension
```

After installation, we will configure `pgpool_recovery` on the `postgresql-master` server by performing the following steps:

```bash
 sudo -u postgres psql -d template1 -f /usr/share/postgresql/16/extension/pgpool-recovery.sql 
 ```

{{< figure src="./images/pgpool-recovery-sql.jpg" >}}

Where `template1` is the sample database where we install `pgpool_recovery`

#### Step 3: Configure PGpool-II

Create a configuration directory for PGpool-II:

```bash
sudo mkdir /etc/pgpool2
```

Copy from the sample config :

````bash
sudo cp /home/pgpool2/etc/pgpool.conf.sample /etc/pgpool2/pgpool.conf 
sudo cp /home/pgpool2/etc/pool_hba.conf.sample /etc/pgpool2/pool_hba.conf 
sudo cp /home/pgpool2/etc/pcp.conf.sample /etc/pgpool2/pcp.conf
````

#### Step 3: Configure connection management

Next, we will configure connection management by editing the configuration file `/etc/pgpool2/pool_hba.conf`:

```bash
sudo nano /etc/pgpool2/pool_hba.conf
```

Add the following configuration to the file:

```bash
host    all         all         0.0.0.0/0          trust
```

{{< figure src="./images/pgpool-pool-hba-config.jpg" >}}

#### Step 4: Start PGpool-II

Finally, restart the PGpool-II service to apply the changes:

```bash
sudo /usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
```

{{< figure src="./images/pgpool-start.jpg" >}}

Thus, we have installed and configured PGpool-II on the Ubuntu Linux operating system. By performing these steps, you can optimize and scale the capabilities of your PostgreSQL database, helping you to maximize the performance and high availability of your database.

akitectio: Error: error building site: render: failed to render pages: render of "home" failed: "/Users/akitect.io/Codes/duydev/akitect.io-new/layouts/index.html:92:23": execute of template failed: template: index.html:92:23: executing "main" at <first 2 .Params.tags>: error calling first: both limit and seq must be provided

GitHub Copilot: The error message indicates that the `first` function in Hugo is not receiving the correct parameters. The `first` function in Hugo requires two parameters: the number of items to return and the sequence from which to return them.

In your case, it seems like `.Params.tags` is not providing a sequence. This could be because `.Params.tags` is `nil` or not an array/slice. 

You can add a condition to check if `.Params.tags` exists and is not empty before using the `first` function. Here's how you can do it:

```html
{{ if and (.Params.tags) (gt (len .Params.tags) 0) }}
    {{ range first 2 .Params.tags }}
        {{ $tagPage := $.Site.GetPage (printf "/tags/%s" .) }}
        <a href="{{ $.Page.Site.BaseURL }}tags/{{ . }}">
            <span class="badge bg-secondary" style="border: 1px solid;color: {{ $tagPage.Params.color }}!important; background-color: {{ $tagPage.Params.background }}!important;">
                #{{ . }}
            </span>
        </a>
    {{ end }}
{{ end }}
```

This code will only execute the `range` loop if `.Params.tags` exists and is not empty.

akitectio: * `/usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf -m smart` trong đó : 

- `-n` : Không chạy dưới dạng daemon
- `-f` : Đường dẫn đến tệp cấu hình `pgpool.conf`
- `-F` : Đường dẫn đến tệp cấu hình quản lý `pcp.conf`
- `-m` : Chế độ hoạt động của Pgpool-II. Có 3 chế độ hoạt động:
  - `fast` : Chế độ hoạt động nhanh
  - `smart` : Chế độ hoạt động thông minh, mặc định

dịch sang tiếng anh

GitHub Copilot: * `/usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf -m smart` where:

- `-n` : Do not run as a daemon
- `-f` : Path to the `pgpool.conf` configuration file
- `-F` : Path to the `pcp.conf` management configuration file
- `-m` : Operating mode of Pgpool-II. There are 3 operating modes:
  - `fast` : Fast operating mode
  - `smart` : Smart operating mode, default

akitectio: - `backend_flag` là cờ cho phép `FAILOVER` khi máy chủ `postgresql-master` bị lỗi, `ALLOW_TO_FAILOVER` là cờ cho phép `FAILOVER` khi máy chủ `postgresql-slave-01` và `postgresql-slave-02` bị lỗi. Các cờ khác: 
 - `DISALLOW_TO_FAILOVER` : không cho phép `FAILOVER`
 - `ALWAYS_MASTER` : luôn là máy chủ `postgresql-master`

 dịch sang tiếng anh

GitHub Copilot: - `backend_flag` is a flag that allows `FAILOVER` when the `postgresql-master` server fails, `ALLOW_TO_FAILOVER` is a flag that allows `FAILOVER` when the `postgresql-slave-01` and `postgresql-slave-02` servers fail. Other flags include:
 - `DISALLOW_TO_FAILOVER` : does not allow `FAILOVER`
 - `ALWAYS_MASTER` : always the `postgresql-master` server

akitectio: ---
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
title: Lesson 2 - Load balancing và Replication
url: /lesson-2-pgpool-ii-configuration-load-balancing-and-replication
description: PGpool-II là một giải pháp trung gian độc đáo, được thiết kế đặc biệt để tối ưu hóa và mở rộng khả năng của hệ quản trị cơ sở dữ liệu PostgreSQL. Nó mang lại nhiều lợi ích như việc tối ưu hóa kết nối, phân phối tải đều và thực hiện sao chép dữ liệu, biến PGpool-II thành công cụ không thể thiếu trong quản lý các triển khai PostgreSQL. Trong hướng dẫn chi tiết này, chúng ta sẽ đi qua các bước để cài đặt và cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.
weight: 2
---

Cấu hình PGpool-II là một bước quan trọng trong quá trình triển khai cụm cơ sở dữ liệu PostgreSQL. Trong hướng dẫn này, chúng ta sẽ đi qua các bước để cấu hình PGpool-II trên hệ điều hành Ubuntu Linux, giúp bạn khai thác tối đa hiệu suất và tính sẵn sàng cao của cơ sở dữ liệu của mình.


# Kiến trúc cài đặt

{{< figure src="./images/postgresql-pgpool.jpeg" >}}


Trước khi bắt đầu ta cần chuẩn bị 4 máy chủ

| IP           | Hostname             | vCPU   | RAM | DISK | OS           |
| ------------ | -------------------- | ------ | --- | ---- | ------------ |
| 192.168.56.5 | pgpool2              | 2 core | 4G  | 50G  | Ubuntu 22.04 |
| 192.168.56.2 | postgresql-master    | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave-01  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | postgresql-slave-02  | 4 core | 8G  | 50G  | Ubuntu 22.04 |


#### Bước 3: Cấu hình Load Balancing và Replication

Bạn cần phải cấu hình Replication trước, nếu bạn chưa cấu hình thì tham khảo [Cài đặt PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc)

Thêm user `replicator` vào 

Tạo tệp `pcp.conf`:

```bash
password_hash=$(pg_md5 PassKhongChilaPasss) && echo "postgre:$password_hash" >> /etc/pgpool2/pcp.conf
```

Tiếp theo, cấu hình tệp `pgpool.conf` trên máy chủ `pgpool2`:

```bash
sudo chown pgpool:pgpool /etc/pgpool2/pgpool.conf
sudo chmod 0600 /etc/pgpool2/pgpool.conf
```

Sau đó, mở tệp `pgpool.conf` bằng trình soạn thảo văn bản `nano`:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```

Thêm cấu hình sau vào tệp `pgpool.conf`:

```bash
listen_addresses = '*' 
port = 9999 

backend_hostname0 = '192.168.56.2'
backend_port0 = 5432
backend_weight0 = 0
backend_data_directory0 = '/home/ubuntu/postgresql/master'
backend_flag0 = 'ALLOW_TO_FAILOVER'
backend_application_name0 = 'postgresql-master'

backend_hostname1 = '192.168.56.3'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/home/ubuntu/postgresql/slave-01'
backend_flag1 = 'ALLOW_TO_FAILOVER'
backend_application_name1 = 'postgresql-slave-01'

backend_hostname2 = '192.168.56.4'
backend_port2 = 5432
backend_weight2 = 2
backend_data_directory2 = '/home/ubuntu/postgresql/slave-02'
backend_flag2 = 'ALLOW_TO_FAILOVER'
backend_application_name2 = 'postgresql-slave-02'

log_statement = on
log_per_node_statement = on

sr_check_user = 'replicator'
health_check_user = 'replicator'
health_check_period = 10

```

Trong đó `listen_addresses` là địa chỉ IP mà PGpool-II sẽ lắng nghe, `port` là cổng mà PGpool-II sẽ lắng nghe.

- `backend_hostname0`, `backend_port0`, `backend_weight0`, `backend_data_directory0`, `backend_flag0`, `backend_application_name0` là thông tin kết nối đến máy chủ `postgresql-master`.

- `backend_hostname1`, `backend_port1`, `backend_weight1`, `backend_data_directory1`, `backend_flag1`, `backend_application_name1` là thông tin kết nối đến máy chủ `postgresql-slave-01`.

- `backend_hostname2`, `backend_port2`, `backend_weight2`, `backend_data_directory2`, `backend_flag2`, `backend_application_name2` là thông tin kết nối đến máy chủ `postgresql-slave-02`.

- `log_statement` và `log_per_node_statement` là cấu hình ghi log truy vấn.

- `sr_check_user` và `health_check_user` là tên người dùng kiểm tra sức khỏe của máy chủ cơ sở dữ liệu. `replicator` là user mà chúng ta đã tạo ở bước [Cài đặt PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc)

- `backend_flag` là cờ cho phép `FAILOVER` khi máy chủ `postgresql-master` bị lỗi, `ALLOW_TO_FAILOVER` là cờ cho phép `FAILOVER` khi máy chủ `postgresql-slave-01` và `postgresql-slave-02` bị lỗi. Các cờ khác: 
 - `DISALLOW_TO_FAILOVER` : không cho phép `FAILOVER`
 - `ALWAYS_MASTER` : luôn là máy chủ `postgresql-master`

#### Bước 4: Chạy kiểm tra cấu hình PGpool-II

Cuối cùng, chạy kiểm tra cấu hình PGpool-II:

```bash
sudo /usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
```

{{< figure src="./images/run-pgpool-configuration.jpg" >}}

Sau khi cấu hình xong, bạn có thể kiểm tra trạng thái của PGpool-II bằng các truy cập vào bằng pg4admin : 

{{< figure src="./images/pg4admin-pgpool.jpg" >}}

- `ip_address` là địa chỉ IP (192.168.56.5) của máy chủ `pgpool2`.
- `username`  mặc định là `postgresql`.
- `password` là mật khẩu của tài khoản `postgresql`.

#### Bước 5: Tạo dữ liệu test 

Ở đây mình sẽ tạo một cơ sở dữ liệu `student` và `10000` dữ liệu test :

```bash

# Tạo cơ sở dữ liệu student
CREATE TABLE student (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100),
    date_of_birth DATE,
    grade_level INT CHECK (grade_level BETWEEN 1 AND 12)
);



WITH generated_data AS (
       SELECT 
           substring(md5(random()::text) from 1 for 8) AS first_name,
           substring(md5(random()::text) from 1 for 8) AS last_name,
           concat(substring(md5(random()::text) from 1 for 6), '@example.com') AS email,
           '1990-01-01'::date + random() * (age(now(), '1990-01-01'::date)) AS date_of_birth,
           (random() * 11 + 1)::int AS grade_level
       FROM generate_series(1,1000000) -- Generate 100 rows
   )
   INSERT INTO student (first_name, last_name, email, date_of_birth, grade_level)
   SELECT * FROM generated_data; 
  
```

{{< figure src="./images/pg4admin-pgpool-student.jpg" >}}

Như ảnh ta thấy dữ liệu `INSERT` đã được thực hiện trên  máy chủ `postgresql-master`.

Tiếp tục ta Query bằng lệnh `SELECT` trên PG4Admin:

```bash
SELECT * FROM student;
```

{{< figure src="./images/pg4admin-pgpool-student-select.jpg" >}}

Sau đó ta quay lại xem log của máy chủ `pgpool2` 

{{< figure src="./images/pgpool-log.jpg" >}}

Như ảnh ta thấy `SELECT` đã được thực hiện trên máy chủ `postgresql-slave-01` thông qua máy chủ `pgpool2`.


dịch sang tiếng anh

GitHub Copilot: ---
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
title: Lesson 2 - Load Balancing and Replication
url: /lesson-2-pgpool-ii-configuration-load-balancing-and-replication
description: PGpool-II is a unique middleware solution, specially designed to optimize and scale the capabilities of the PostgreSQL database management system. It offers numerous benefits such as connection optimization, load balancing, and data replication, making PGpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure PGpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.
weight: 2
---

Configuring PGpool-II is an important step in the process of deploying a PostgreSQL database cluster. In this guide, we will go through the steps to configure PGpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.


# Installation Architecture

{{< figure src="./images/postgresql-pgpool.jpeg" >}}


Before we start, we need to prepare 4 servers

| IP           | Hostname             | vCPU   | RAM | DISK | OS           |
| ------------ | -------------------- | ------ | --- | ---- | ------------ |
| 192.168.56.5 | pgpool2              | 2 core | 4G  | 50G  | Ubuntu 22.04 |
| 192.168.56.2 | postgresql-master    | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave-01  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | postgresql-slave-02  | 4 core | 8G  | 50G  | Ubuntu 22.04 |


#### Step 3: Configure Load Balancing and Replication

You need to configure Replication first, if you haven't configured it yet, refer to [Installing PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc)

Add `replicator` user 

Create `pcp.conf` file:

```bash
password_hash=$(pg_md5 PassKhongChilaPasss) && echo "postgre:$password_hash" >> /etc/pgpool2/pcp.conf
```

Next, configure the `pgpool.conf` file on the `pgpool2` server:

```bash
sudo chown pgpool:pgpool /etc/pgpool2/pgpool.conf
sudo chmod 0600 /etc/pgpool2/pgpool.conf
```

Then, open the `pgpool.conf` file with the `nano` text editor:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```

Add the following configuration to the `pgpool.conf` file:

```bash
listen_addresses = '*' 
port = 9999 

backend_hostname0 = '192.168.56.2'
backend_port0 = 5432
backend_weight0 = 0
backend_data_directory0 = '/home/ubuntu/postgresql/master'
backend_flag0 = 'ALLOW_TO_FAILOVER'
backend_application_name0 = 'postgresql-master'

backend_hostname1 = '192.168.56.3'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/home/ubuntu/postgresql/slave-01'
backend_flag1 = 'ALLOW_TO_FAILOVER'
backend_application_name1 = 'postgresql-slave-01'

backend_hostname2 = '192.168.56.4'
backend_port2 = 5432
backend_weight2 = 2
backend_data_directory2 = '/home/ubuntu/postgresql/slave-02'
backend_flag2 = 'ALLOW_TO_FAILOVER'
backend_application_name2 = 'postgresql-slave-02'

log_statement = on
log_per_node_statement = on



sr

_check_user = 'replicator'
health_check_user = 'replicator'
health_check_period = 10

```

Where `listen_addresses` is the IP address that PGpool-II will listen to, `port` is the port that PGpool-II will listen to.

- `backend_hostname0`, `backend_port0`, `backend_weight0`, `backend_data_directory0`, `backend_flag0`, `backend_application_name0` are the connection information to the `postgresql-master` server.

- `backend_hostname1`, `backend_port1`, `backend_weight1`, `backend_data_directory1`, `backend_flag1`, `backend_application_name1` are the connection information to the `postgresql-slave-01` server.

- `backend_hostname2`, `backend_port2`, `backend_weight2`, `backend_data_directory2`, `backend_flag2`, `backend_application_name2` are the connection information to the `postgresql-slave-02` server.

- `log_statement` and `log_per_node_statement` are query logging configurations.

- `sr_check_user` and `health_check_user` are the usernames for checking the health of the database server. `replicator` is the user that we created in the step [Installing PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc)

- `backend_flag` is a flag that allows `FAILOVER` when the `postgresql-master` server fails, `ALLOW_TO_FAILOVER` is a flag that allows `FAILOVER` when the `postgresql-slave-01` and `postgresql-slave-02` servers fail. Other flags include: 
 - `DISALLOW_TO_FAILOVER` : does not allow `FAILOVER`
 - `ALWAYS_MASTER` : always the `postgresql-master` server

#### Step 4: Run PGpool-II Configuration Check

Finally, run the PGpool-II configuration check:

```bash
sudo /usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
```

{{< figure src="./images/run-pgpool-configuration.jpg" >}}

After the configuration is complete, you can check the status of PGpool-II by accessing it via pg4admin : 

{{< figure src="./images/pg4admin-pgpool.jpg" >}}

- `ip_address` is the IP address (192.168.56.5) of the `pgpool2` server.
- `username`  default is `postgresql`.
- `password` is the password of the `postgresql` account.

#### Step 5: Create Test Data 

Here I will create a `student` database and `10000` test data :

```bash

# Create student database
CREATE TABLE student (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100),
    date_of_birth DATE,
    grade_level INT CHECK (grade_level BETWEEN 1 AND 12)
);



WITH generated_data AS (
       SELECT 
           substring(md5(random()::text) from 1 for 8) AS first_name,
           substring(md5(random()::text) from 1 for 8) AS last_name,
           concat(substring(md5(random()::text) from 1 for 6), '@example.com') AS email,
           '1990-01-01'::date + random() * (age(now(), '1990-01-01'::date)) AS date_of_birth,
           (random() * 11 + 1)::int AS grade_level
       FROM generate_series(1,1000000) -- Generate 100 rows
   )
   INSERT INTO student (first_name, last_name, email, date_of_birth, grade_level)
   SELECT * FROM generated_data; 
  
```

{{< figure src="./images/pg4admin-pgpool-student.jpg" >}}

As the image shows, the `INSERT` data has been executed on the `postgresql-master` server.

Continue to Query with the `SELECT` command on PG4Admin:

```bash
SELECT * FROM student;
```

{{< figure src="./images/pg4admin-pgpool-student-select.jpg" >}}

Then we go back to see the log of the `pgpool2` server 

{{< figure src="./images/pgpool-log.jpg" >}}

As seen in the image, the `SELECT` operation was performed on the `postgresql-slave-01` server through the `pgpool2` server.

Thus, we have successfully configured PGpool-II to perform Load Balancing and Replication.