---
categories:
  - microk8s
date: 2023-01-06T08:00:00+08:00
description: Nếu bạn một Lập trình viên, nhà phát triển phần miềm, kiểm thử phần miềm, designer, một ngày đẹp trời đội dự án kiêu bạn vào và kiêu bạn chạy thử phần miềm trên máy bạn, bạn phải xây dựng 1 hệ thông y chang nhưng máy bạn, để cho tiện việt phát trển và kiểm thử, và hạn chế các câu hỏi "Tại sao máy tôi chạy được, mà máy bạn chạy không được", thì chúng ta có thể dùng Vagrant để đồng bộ cho môi trường phát triển
draft: false
featuredImage: /series/microk8s/lesson-3-install-and-run-the-oracle-vm-virtualbox-7-virtualization-environment-using-vagrant.webp
images:
  - /series/microk8s/lesson-3-install-and-run-the-oracle-vm-virtualbox-7-virtualization-environment-using-vagrant.webp
  - /bai-3-cai-dat-va-chay-moi-truong-ao-hoa-oracle-vm-virtualbox-7-bang-vagrant/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - kubernetes
  - k8s
  - ubuntu
  - virtualbox
  - virtualbox 7
  - virtualbox 7 on ubuntu 22.04
title: Bài 3 - Cài đặt và chạy môi trường ảo hoá Oracle VM VirtualBox 7 bằng Vagrant
url: bai-3-cai-dat-va-chay-moi-truong-ao-hoa-oracle-vm-virtualbox-7-bang-vagrant
weight: 3
---

Vagrant là gì?

Vagrant là một công cụ để làm việc với các môi trường ảo và trong hầu hết các trường hợp, điều này có nghĩa là làm việc với các máy ảo. Vagrant cung cấp một ứng dụng khách lệnh đơn giản và dễ sử dụng để quản lý các môi trường này và một trình thông dịch cho các định nghĩa dựa trên văn bản về mỗi môi trường trông như thế nào, được gọi là vagrantfiles. Vagrant là nguồn mở, có nghĩa là bất cứ ai cũng có thể tải xuống, sửa đổi nó và chia sẻ nó một cách tự do.

Mặc dù nhiều trình lập trình máy ảo cung cấp giao diện dòng lệnh của riêng họ và về mặt kỹ thuật việc cung cấp các máy ảo thông qua các chương trình này có thể được thực hiện trực tiếp hoặc thông qua các tập lệnh shell, lợi thế khác cung cấp bằng cách thêm một lớp bổ sung là đơn giản, khả năng tương tác trên nhiều hệ thống và Một cách tiếp cận nhất quán hơn về mặt lý thuyết có thể được sử dụng với bất kỳ môi trường ảo nào chạy trên bất kỳ hệ thống nào khác.

Bằng cách cung cấp một định dạng dựa trên văn bản phổ biến để làm việc với các máy ảo, môi trường của bạn có thể được xác định trong mã, giúp bạn dễ dàng sao lưu, sửa đổi, chia sẻ và quản lý với kiểm soát sửa đổi. Điều đó cũng có nghĩa là thay vì chia sẻ toàn bộ hình ảnh máy ảo, có thể là nhiều gigabyte, mỗi khi thay đổi được thực hiện với cấu hình, một tệp văn bản đơn giản chỉ có thể chia sẻ một vài kilobyte.

| IP           | Hostname          | vCPU   | RAM | DISK |
| ------------ | ----------------- | ------ | --- | ---- |
| 192.168.56.2 | microk8s-master-1 | 1 core | 2G  | 50G  |
| 192.168.56.3 | microk8s-master-2 | 1 core | 2G  | 50G  |
| 192.168.56.4 | microk8s-master-3 | 1 core | 2G  | 50G  |
| 192.168.56.5 | microk8s-worker-1 | 1 core | 2G  | 50G  |
| 192.168.56.6 | microk8s-worker-2 | 1 core | 2G  | 50G  |
| 192.168.56.7 | microk8s-worker-3 | 1 core | 2G  | 50G  |
| 192.168.56.8 | microk8s-worker-4 | 1 core | 2G  | 50G  |

# Cài đặt Vagrant

### Bước 1: Ta cài đặt Oracle VM VirtualBox 7

Tham khảo lại bài trước

### Bước 2: Cài đặt Vagrant

Chúng ta vào trang chủ của vagrant để tải tiệp cài đặt về https://developer.hashicorp.com/vagrant/downloads tuỳ theo hệ điều hành thì chúng ta sẽ chọn tiệp cài đặt đó

{{< figure src="./edbe3216-d88f-4982-8607-486cc1958c1f.png" >}}

### Bước 3: Sau khi cài đặt thành công ta tạo file Vagriantfile

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.winrm.timeout = 1800 # 30 minutes
  config.vm.boot_timeout = 1800 # 30 minutes
  config.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
  config.vm.provision "shell", inline: <<-EOF
    apt install net-tools
    ifconfig
    snap install microk8s --classic
    microk8s.status --wait-ready
    microk8s.enable dns
    usermod -a -G microk8s vagrant
    echo "alias kubectl='microk8s.kubectl'" > /home/vagrant/.bash_aliases
    chown vagrant:vagrant /home/vagrant/.bash_aliases
    echo "alias kubectl='microk8s.kubectl'" > /root/.bash_aliases
    chown root:root /root/.bash_aliases
    echo "

    #master
    192.168.56.2  microk8s-master-01
    192.168.56.3  microk8s-master-02
    192.168.56.4  microk8s-master-03

    #worker
    192.168.56.5  microk8s-worker-01
    192.168.56.6  microk8s-worker-02
    192.168.56.7  microk8s-worker-03
    192.168.56.8  microk8s-worker-04

    " > /etc/hosts
    cat /etc/hosts
  EOF

  config.vm.define "microk8s_master_01" do |microk8s_master_01|
    microk8s_master_01.vm.hostname = "microk8s-master-01"
    microk8s_master_01.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_master_01.vm.network "private_network", ip: "192.168.56.2"
    microk8s_master_01.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-master-01"
    end
  end
  config.vm.define "microk8s_master_02" do |microk8s_master_02|
    microk8s_master_02.vm.hostname = "microk8s-master-02"
    microk8s_master_02.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_master_02.vm.network "private_network", ip: "192.168.56.3"
    microk8s_master_02.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-master-02"
    end
  end
  config.vm.define "microk8s_master_03" do |microk8s_master_03|
    microk8s_master_03.vm.hostname = "microk8s-master-03"
    microk8s_master_03.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_master_03.vm.network "private_network", ip: "192.168.56.4"
    microk8s_master_03.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-master-03"
    end
  end
  config.vm.define "microk8s_worker_01" do |microk8s_worker_01|
    microk8s_worker_01.vm.hostname = "microk8s-worker-01"
    microk8s_worker_01.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_worker_01.vm.network "private_network",  ip: "192.168.56.5"
    microk8s_worker_01.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-woker-01"
    end
  end
  config.vm.define "microk8s_worker_02" do |microk8s_worker_02|
    microk8s_worker_02.vm.hostname = "microk8s-worker-02"
    microk8s_worker_02.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_worker_02.vm.network "private_network", ip: "192.168.56.6"
    microk8s_worker_02.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-worker-02"
    end
  end
  config.vm.define "microk8s_worker_03" do |microk8s_worker_03|
    microk8s_worker_03.vm.hostname = "microk8s-worker-03"
    microk8s_worker_03.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_worker_03.vm.network "private_network", ip: "192.168.56.7"
    microk8s_worker_03.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-worker-03"
    end

  end
  config.vm.define "microk8s_worker_04" do |microk8s_worker_04|
    microk8s_worker_04.vm.hostname = "microk8s-worker-04"
    microk8s_worker_04.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_worker_04.vm.network "private_network",  ip: "192.168.56.8"
    microk8s_worker_04.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-worker-04"
    end
  end
end
```

{{< figure src="./46a04e77-c66f-4b8a-94cf-ba43d01c7fc4.png" >}}

### Bước 4: Cấu hình network

Ở đây mình tạo 2 network **public_network** và **private_network**

- public_network: Kết nối trược Internet
- private_network: Không kết nối trược Internet chỉ kết nối được mạng LAN nội bộ của VirtualBox

### Bước 5: Vagrant up

Sau khi tạo xong thì chúng ta sử dụng lệnh **Vagrant up** để chạy môi trường ảo hoá lên

```
Vagrant up
```

> Tuỳ theo mạng và cấu hình máy của bạn, bước này tầm 30 -> 60p

{{< figure src="./6a303659-3afa-4291-a22d-4fd4d5cb1be9.png" >}}

Sau đó bạn mở **Oracle VM VirtualBox** và xem các máy ảo đã tạo và start lên chưa

{{< figure src="./388b6633-8ff5-4d47-8552-c7ec30a86f72.png" >}}

### Bước 5: Vagrant ssh và join các node vào master

Như vậy đã start thành công tiếp tục ta ssh vào máy microk8s-master-01 bằng lệnh

```
vagrant ssh microk8s_master_01
```

{{< figure src="./4923dfb0-1710-4068-b958-f6db06d5a823.png" >}}

Tiếp tục ta tạo token để kết nối vào cụm microk8s

```
microk8s add-node
```

{{< figure src="./cd2e5200-4905-4ca7-82c1-6f4736f5be64.png" >}}

sau đó ta ssh vào các máy **microk8s-master-02** và **microk8s-master-03** để join vào các máy master

{{< figure src="./5f13ea88-a045-488f-8d5f-d07fc1bc1421.png" >}}

Chunng ta sử dụng lệnh **microk8s join** để kết nối 2 máy vào cum master

```
microk8s join 192.168.56.2:25000/f9b92e3f904dd17cba2332a88a3092da/a25a7c633d5c
```

{{< figure src="./fe7179c0-9934-4b35-bece-f3761a956f4d.png" >}}

{{< figure src="./3945f334-da82-4782-9b36-8c59eac4a499.png" >}}

Ta dùng lệnh **microk8s kubectl get no** để check xem 2 node master 01 vs master 02 đã join chưa

```
microk8s kubectl get no
```

{{< figure src="./8007393e-cbb0-4565-a9d4-9095f295683d.png" >}}

Như vậy cụm master đã join thành công, bây giờ ta thực hiện với cụm worker, sau đó ta ssh vào các máy **microk8s-worker-01**, **microk8s-worker-02**, **microk8s-worker-03**, **microk8s-worker-04** để join vào các máy worker

```
vagrant ssh microk8s_worker_01
```

{{< figure src="./d2b2744c-4192-456a-bbf4-070175392a4c.png" >}}

```
vagrant ssh microk8s_worker_02
```

{{< figure src="./c01e6c0e-e040-48c9-8d19-57ba610815eb.png" >}}

```
vagrant ssh microk8s_worker_03
```

{{< figure src="./d827a85c-afa0-4532-90e3-74c9ecc08250.png" >}}

```
vagrant ssh microk8s_worker_04
```

{{< figure src="./35623fc0-db5b-4c47-9266-812125831d00.png" >}}

Sau khi login vào các worker thành công ta vào con **microk8s-master-01** gõ lệnh **microk8s add-node **

```
microk8s add-node
```

{{< figure src="./e4d809a4-7660-4b7c-8919-64f695894dbe.png" >}}

Ta sử dụng token có ip là **192.168.56.2** và thêm **--worker** ở cuối, và áp dụng cho tất cả các máy **worker**

```
microk8s join 192.168.56.2:25000/{toke} --worker
```

{{< figure src="./844ec79a-ac53-4d78-8d59-517f08ffad37.png" >}}

Ta dùng lệnh **microk8s kubectl get no** trên máy **master 01** để check xem các **node woker** đã kết nối được chưa

{{< figure src="./8e830ded-3564-42b0-b457-25ce61fa4b99.png" >}}

Như vậy ta đã tạo máy ảo thành công và kết nối được **3 master** với **4 worker**

**Cảm ơn các bạn đã theo dõi bài viết của mình, nếu các bạn thấy hay thì cho mình 1 upvote để có thêm động lực chia sẽ nhiều bài viết hữu ích hơn**
