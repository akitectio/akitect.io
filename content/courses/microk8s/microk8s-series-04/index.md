---
categories:
  - microk8s
date: 2023-01-07T08:00:00+08:00
description: Một người bạn của tôi đã từng hỏi, tại sao tôi lại lại thích microk8s hơn minikube?… Kể từ đó, chúng tôi không bao giờ nói chuyện nữa. Đó là một câu hỏi khó, đặc biệt là đối với một kỹ sư. Câu trả lời không quá rõ ràng, vì nó phải tự trải nghiệm và sở thích cá nhân. Để tôi chỉ cho bạn hiểu vì sao.
draft: false
featuredImage: /series/microk8s/lesson-4-installing-kubernetes-with-microk8s-1-26-on-ubuntu-22-04.webp
images:
  - /series/microk8s/lesson-4-installing-kubernetes-with-microk8s-1-26-on-ubuntu-22-04.webp
  - /bai-4-cai-dat-kubernetes-voi-microk8s-126-tren-ubuntu-2204/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - kubernetes
  - k8s
  - ubuntu
  - virtualbox
title: Bài 3 - Cài đặt Kubernetes với Microk8s 1.26 trên Ubuntu 22.04
url: /bai-4-cai-dat-kubernetes-voi-microk8s-126-tren-ubuntu-2204
weight: 4
---

MicrroK8s là gì?

MicroK8s là một nền tảng Kubernetes nhẹ và độc lập, được phát triển bởi Canonical, nhà cung cấp của hệ điều hành Ubuntu. Nó được thiết kế để giúp các nhà phát triển và nhà quản trị hệ thống triển khai và quản lý các ứng dụng và dịch vụ đám mây trên Kubernetes một cách nhanh chóng và dễ dàng.

MicroK8s cung cấp một cách đơn giản để triển khai một cụm Kubernetes trên một máy tính cá nhân, một môi trường phát triển hoặc một trung tâm dữ liệu. Nó cung cấp cho người dùng một số tính năng quan trọng như:

- Cài đặt nhanh chóng và dễ dàng: MicroK8s có thể được cài đặt chỉ bằng một lệnh duy nhất và không cần đòi hỏi kiến thức chuyên môn về Kubernetes.
- Độc lập và linh hoạt: MicroK8s hoạt động như một nền tảng độc lập và có thể được triển khai trên bất kỳ hệ điều hành nào.
- Tích hợp tốt với các công cụ phát triển và triển khai: MicroK8s tích hợp với các công cụ phát triển và triển khai phổ biến như Docker, Helm, kubectl và các công cụ DevOps khác.
- An toàn và bảo mật: MicroK8s cung cấp các tính năng bảo mật và an toàn như tường lửa mạng, TLS và RBAC để giữ cho cụm Kubernetes của bạn an toàn và bảo mật.

> Tóm lại, MicroK8s là một giải pháp Kubernetes nhẹ và độc lập, giúp người dùng triển khai và quản lý các ứng dụng đám mây một cách nhanh chóng và dễ dàng trên các môi trường khác nhau.

## **Lợi ích khi sử dụng Microk8s**

1. Dễ cài dặt, chỉ cần máy của bạn hổ trợ [snap](https://snapcraft.io/)
2. Hỗ trợ cho hơn 42 HĐH Linux, Windows,Mac OS
3. Hỗ trợ nhanh các cài đặt nhanh các addon Kubernetes cốt lõi như dns và dashboard
4. Kiểm soát cụm của bạn bằng công cụ kubectl CLI
5. Triển khai nhanh các container trong cụm

> Trong bài viết này mình sẽ hướng dẫn các bạn cài đặt cụm 11 node theo mô hình HA (High Availability ) gồm có 3 con master và 4 worker có tính khả dụng cao

Bạn cần chuẩn bị 7 máy chủ cấu hình như sau:

**Danh sách máy chủ**

| IP           | Hostname          | vCPU   | RAM | DISK |
| ------------ | ----------------- | ------ | --- | ---- |
| 192.168.56.2 | microk8s-master-1 | 1 core | 2G  | 50G  |
| 192.168.56.3 | microk8s-master-2 | 1 core | 2G  | 50G  |
| 192.168.56.4 | microk8s-master-3 | 1 core | 2G  | 50G  |
| 192.168.56.5 | microk8s-worker-1 | 1 core | 2G  | 50G  |
| 192.168.56.6 | microk8s-worker-2 | 1 core | 2G  | 50G  |
| 192.168.56.7 | microk8s-worker-3 | 1 core | 2G  | 50G  |
| 192.168.56.8 | microk8s-worker-4 | 1 core | 2G  | 50G  |

{{< youtube "0EAkiuABZwM" >}}

\*_Mình sẽ triển khi cài đặt theo mô hình như sau_

{{< figure src="./4306a5e6-a54d-4d9b-8cd5-fcace72b60e8.jpg" >}}

Do lần đầu vẽ tay nên vẽ cũng hơi khó xem, mình sẽ cố đẹp hơn trong lần update sau <3

**Giờ chúng ta bắt đầu thôi :**

### Bước 1 : ssh vào từng node và đổi lại hostname giống như trên

- Vào thư mục **/etc/hostname**
- Đổi tên **hostname** theo từng máy chủ ở **Danh sách máy chủ**

* Các máy chủ Master

{{< figure src="./7d487a2e-cd78-472e-888e-7a8521183cf7.png" >}}

- Các máy chủ Worker

{{< figure src="./ff654f99-1af8-412d-b843-c8e80ecd6f60.png" >}}

### Bước 2: Update và upgrade tất cả các node

```
sudo apt update && apt upgrade -y
```

### Bước 3: Thêm các trường vào file hosts

- Mở file: /etc/hosts

```
nano /etc/hosts
```

- Thêm vào cuối file :

```
#master
192.168.56.2  microk8s-master-01
192.168.56.3  microk8s-master-02
192.168.56.4  microk8s-master-03

#worker
192.168.56.5  microk8s-worker-1
192.168.56.6  microk8s-worker-2
192.168.56.7  microk8s-worker-3
192.168.56.8  microk8s-worker-4
```

- Các máy chủ Master

  {{< figure src="./a35e4a4a-d9c3-4ed8-832d-b687a1e3d7ce.png" >}}

- Các máy chủ Worker

  {{< figure src="./5548e489-d1b9-4656-b956-a96bd506665a.png" >}}

### Bước 4: Cài đặt microk8s

```
sudo snap install microk8s --classic --channel=1.26
```

Microk8s tạo ra một nhóm để kích hoạt sử dụng các lệnh liền mạch yêu cầu đặc quyền root. Để thêm người dùng hiện tại của bạn vào nhóm và có quyền truy cập vào thư mục bộ đệm Kube, hãy chạy hai lệnh sau:

```
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
su - $USER
```

Kiểm tra trạng thái microk8s bằng lệnh `microk8s status --wait-ready`

- Các máy chủ Master
  {{< figure src="./f5a0cde6-bec6-4cb1-ae58-d247e1e037db.png" >}}

### Bước 5: Thêm các node vào cụm

Do hệ thống mình chạy luôn trên máy host bằng Virtuabox nên mình mở tường lửa, các bạn cần allow port của microk8s để thực hiện lệnh ko bị lỗi chận port, các bạn tham khảo qua đường dẫn https://microk8s.io/docs/services-and-ports

Các bạn vui lòng thực hiện lệnh sau trên tất cả các node:

```
sudo ufw allow 16443/tcp
sudo ufw allow 10250/tcp
sudo ufw allow 10255/tcp
sudo ufw allow 25000/tcp
sudo ufw allow 12379/tcp
sudo ufw allow 10257/tcp
sudo ufw allow 10259/tcp
sudo ufw allow 19001/tcp
sudo ufw allow 4789/udp

 sudo ufw allow 10248/tcp
sudo ufw allow 10249/tcp
sudo ufw allow 10251/tcp
sudo ufw allow 10252/tcp
sudo ufw allow 2380/tcp
sudo ufw allow 1338/tcp
```

- Tạo token add-node trên con **microk8s-master-01** :

```
microk8s add-node --token-ttl 3600
```

{{< figure src="./4a7b8e80-85bd-4a81-9862-947fed46027b.png" >}}

Mình có 2 đường mạng:

1. Internet
2. LAN (Mạng dùng để connect các node)

- join các máy chủ master, ở đây mình thực hiện lệnh add ở máy **microk8s-master-01** thì mình cần add 2 máy **microk8s-master-02** và **microk8s-master-03**

```
microk8s join 192.168.56.2:25000/e523c2d3aef2e3679c3e5ccf605d97c2/dbc9df54be3b
```

- Tiếp tục join worker vào các node **microk8s-worker-1, microk8s-worker-2, microk8s-worker-3, microk8s-worker-4** cũng là đoạn token trên, và chỉ cần thêm **_--worker_** sau cùng đoạn join

```
microk8s join 192.168.56.2:25000/e523c2d3aef2e3679c3e5ccf605d97c2/dbc9df54be3b --worker
```

Khi join thành công ta dùng lệnh `microk8s kubectl get no` ở VM **microk8s-master-01** để kiểm tra xem các node đã join vào làm master và worker chưa

{{< figure src="./5b7291da-efb3-4e4f-ba93-f9be3646cc1d.png" >}}

Còn để kiể tra các master node chúng ta dùng lệnh `microk8s status` ở VM **microk8s-master-01**

{{< figure src="./abfe6089-285c-4111-91a2-658fa74f9bbc.png" >}}

### Bước 6: Kích hoạt addon dashboard dns storage

```
microk8s enable dns dashboard hostpath-storage
```

sau khi thành công bạn dùng lệnh **microk8s dashboard-proxy** ở VM **microk8s-master-01** để mở dashboard

```
microk8s dashboard-proxy
```

{{< figure src="./959033c4-3f29-4284-a076-d6da2e730f95.png" >}}

Truy cập đường dẫn https://192.168.56.2:10443 để vào **Kubernetes Dashboard**

{{< figure src="./9773ef9c-0ada-4643-8e91-5988d4df4ba3.png" >}}

Các bạn có thể tham khảo thêm ở trang chủ : https://microk8s.io/docs/high-availability

Cảm ơn mọi người đã theo dõi bài viết!
