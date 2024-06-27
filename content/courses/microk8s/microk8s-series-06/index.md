---
categories:
  - microk8s
date: 2023-01-12T08:00:00+08:00
description: Trước tiên thì hệ thống mình hướng dẫn trong series sẽ triển khai trong moi trường VM và các image sẽ không public ra Internet nên mình sử dụng private registry
draft: false
featuredImage: /series/microk8s/lesson-6-installing-and-using-private-registry-in-the-microk8s-integrator.webp

images:
  - /series/microk8s/lesson-6-installing-and-using-private-registry-in-the-microk8s-integrator.webp
  - /bai-6-cai-dat-va-su-dung-private-registry-trong-bo-tich-hop-cua-microk8s/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - kubernetes
  - k8s
  - ubuntu
  - virtualbox
title: Bài 5 - Cài đặt và sử dụng private registry trong bộ tích hợp của Microk8s
url: /bai-6-cai-dat-va-su-dung-private-registry-trong-bo-tich-hop-cua-microk8s
weight: 6
---

## Kích hoạt registry

Mình ssh vào VM **microk8s-master-01 (192.168.56.2)** để thực hện, vai trò con microk8s-master-01 hiện giờ đang đóng vai trò Master

Sau khi login vào microk8s-master-01 thành công ta tiến hành kích hoạt registry bằng lệnh

```bash
microk8s enable registry
```

Sau khi kích hoạt registry thì persistent volume mặt định là 20G dùng để lưu trữu images, tuy nhiên bạn có thể cấp thêm để phù hợp với ứng dụng của bạn

```bash
microk8s enable registry:size=40Gi
```

{{< figure src="/adadf521-dc7c-4a3b-9ced-3b7ba4dc5902.png" >}}

## Cách sử dụng Private Registry

### 1. Cài đặt **Docker Engine** trên trên **Server Host**

- Các bạn tham khảo thêm ở nguồn https://docs.docker.com/engine/install/ubuntu/

- Gỡ cài đặt phiên bản cũ

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

{{< figure src="/90625428-54b1-4f12-a0af-275518928cf9.png" >}}

### Thiết lập repository

- Cập nhật chỉ mục gói apt và cài đặt các gói để cho phép apt sử dụng kho lưu trữ qua HTTPS:

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release
```

- Thêm khóa GPG chính thức của Docker:

```bash
 sudo mkdir -p /etc/apt/keyrings
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

- Sử dụng lệnh để thiết lập repository

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

{{< figure src="/7c16258e-9349-45cd-8c43-b750dd4cce2b.webp" >}}

### Cài đặt Docker Engine

1. Cập nhât các gói apt

```bash
 sudo apt-get update
```

2. Cài đặt Docker Engine, containerd và Docker Compose.

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

3. thêm quyền docker

```bash
sudo gpasswd -a $USER docker
newgrp docker
sudo su $USER
```

{{< figure src="/7ea474c3-8db9-463a-9a6c-cc5f42dabb01.png" >}}

### Cấu hình Insecure registry (Cho phép push image không cần SSL)

Ở máy build và máy push từ Docker chúng ta cần thêm cấu hình ở file **/etc/docker/daemon.json**

```bash
sudo nano /etc/docker/daemon.json
```

thêm đoạn cấu hình vào

```json
{
  "insecure-registries" : ["192.168.56.2:32000"]
}
```

{{< figure src="/dd099e8f-6d2d-4537-9b8c-daf4a0949819.webp" >}}

Lưu lại và khởi động lại service docker

```bash
sudo systemctl restart docker
```

Bây giờ ta test cấu hình theo các bước:

1. Pull image Nginx

```bash
docker pull nginx
```

{{< figure src="/8d58395e-78ad-4337-ac00-b3b428e65d1f.png" >}}

2. tag file image nginx -> 192.168.56.2:32000/duy-tran-nginx

```bash
 docker tag nginx 192.168.56.2:32000/duy-tran-nginx
```

{{< figure src="/e8de8112-11a3-46b7-8ef2-dab56a8dfa72.webp" >}}

4. Push image 10.19.2.92:32000/duy-tran-nginx lên registry

```bash
 docker push 192.168.56.2:32000/duy-tran-nginx
```

{{< figure src="/0c4d3201-daf1-4172-ad79-ba724f543d9a.webp" >}}

### Cấu hình microk8s

Microk8s 1.23 và các phiên bản mới hơn sử dụng các tệp máy chủ riêng biệt cho mỗi đăng ký hình ảnh. Đối với Cơ quan đăng ký http://192.168.56.2:32000, tiệp cấu hình sẽ ở **/var/snap/microk8s/current/args/certs.d/192.168.56.2:32000**

Đầu tiên, hãy tạo thư mục và tiệp nếu nó không tồn tại:

```bash
sudo mkdir -p /var/snap/microk8s/current/args/certs.d/192.168.56.2:32000
sudo touch /var/snap/microk8s/current/args/certs.d/192.168.56.2:32000/hosts.toml
```

Sau đó, chỉnh sửa tệp bạn vừa tạo và đảm bảo đúng nội dung như sau:

```toml
# /var/snap/microk8s/current/args/certs.d/192.168.56.2:32000/hosts.toml
server = "http://192.168.56.2:32000"

[host."http://192.168.56.2:32000"]
capabilities = ["pull", "resolve"]
```

{{< figure src="/a8d81df1-f2d5-4c51-a2e8-680e3b487cbe.png" >}}

Lưu lại và khởi động lại dịch vụ microk8s

```bash
microk8s stop
microk8s start
```

### Deploy và test image mới push

1. Tạo tiệp **duy-tran-nginx-all.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: duy-tran-nginx-deployment
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      app: duy-tran-nginx
  template:
    metadata:
      labels:
        app: duy-tran-nginx
    spec:
      containers:
        - name: duy-tran-nginx
          image: 192.168.56.2:32000/duy-tran-nginx
          ports:
            - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: duy-tran-nginx-svc
  labels:
    app: duy-tran-nginx-svc
spec:
  type: NodePort
  selector:
    app: duy-tran-nginx
  ports:
    - name: http
      port: 80
      targetPort: 80
      nodePort: 30039
      protocol: TCP
```

2. Run file **duy-tran-nginx-all.yaml**

```bash
 microk8s kubectl apply -f duy-tran-nginx-all.yaml
```

{{< figure src="/92dfade4-1da8-4033-8233-bb6463997ba5.webp" >}}

Có 2 cách để test:

1. Vào port 30039 để kiểm tra : http://192.168.56.2:30039

{{< figure src="/6fda0347-2055-4c17-aa4d-7ca063cd074c.webp" >}}

2. run dashboard để kiểm tra

```bash
microk8s dashboard-proxy
```

{{< figure src="/0c878dc2-ba78-4a92-9cbd-488fcd79e1fc.webp" >}}

{{< figure src="/516a3f6a-8f07-420a-a4c6-c466d3e54e23.webp" >}}


Link [github](https://github.com/akitectio/microk8s-series/) để các bạn copy cho nhanh các tiệp
