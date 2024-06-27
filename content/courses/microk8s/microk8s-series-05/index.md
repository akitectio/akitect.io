---
categories:
  - microk8s
date: 2023-01-07T08:00:00+08:00
description: Ở bài này thì mình cần thêm 1 VM cài Nginx để loadbalancer ở đây mình sẽ tạo thêm 1 VM ubuntu 22.04
draft: false
featuredImage: /series/microk8s/lesson-5-install-and-use-ingress-in-the-microk8s-integrator.webp
images:
  - /series/microk8s/lesson-5-install-and-use-ingress-in-the-microk8s-integrator.webp
  - /bai-5-cai-dat-va-su-dung-ingress-trong-bo-tich-hop-cua-microk8s/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - kubernetes
  - k8s
  - ubuntu
  - virtualbox
title: Bài 4 - Cài đặt và sử dụng ingress trong bộ tích hợp của Microk8s
url: /bai-5-cai-dat-va-su-dung-ingress-trong-bo-tich-hop-cua-microk8s
weight: 5
---

| Hostname     | vCPU              | RAM    | DISK |
| ------------ | ----------------- | ------ | ---- | -------- |
| 127.0.0.1    | Host              | 8 core | 32G  | SSD 500G |
| 192.168.56.2 | microk8s-master-1 | 1 core | 2G   | 50G      |
| 192.168.56.3 | microk8s-master-2 | 1 core | 2G   | 50G      |
| 192.168.56.4 | microk8s-master-3 | 1 core | 2G   | 50G      |
| 192.168.56.5 | microk8s-worker-1 | 1 core | 2G   | 50G      |
| 192.168.56.6 | microk8s-worker-2 | 1 core | 2G   | 50G      |
| 192.168.56.7 | microk8s-worker-3 | 1 core | 2G   | 50G      |
| 192.168.56.8 | microk8s-worker-4 | 1 core | 2G   | 50G      |

## Cài đặt Nginx trên Ubuntu 22.04

{{< youtube "GBJrgIYU3aI" >}}

### Bước 1 – Cài đặt Nginx

Cập nhật các gói cài đặt apt

```bash
sudo apt update
```

Cài đặt Nginx

```bash
sudo apt install nginx -y
```

{{< figure src="/9483e7a2-849f-4e2d-82ab-c4f6fe4ce3af.webp" >}}

### Bước 2 – Cấp quyền HTTP Firewall

```bash
sudo ufw allow 'Nginx HTTP'
```

### Bước 3 – Kiểm tra Máy chủ Web của bạn

Kiểm tra service nginx có hoạt đông không?

{{< figure src="/a1a7fe65-fc40-4b1b-a890-c5da04c2f000.webp" >}}

## Để tiện cho việc quản trị code trên máy master và test thì mình sẽ cài code-server lên máy microk8s-master-01

{{< youtube "-zkiQWYhRfg" >}}

Để cài đặt các bạn chạy lệnh:

```bash
curl -fsSL https://code-server.dev/install.sh | sh

sudo systemctl enable --now code-server@$USER

```

sau khi cài đặt xong thì mình vào thư mục config

```bash
nano ~/.config/code-server/config.yaml

bind-addr: 0.0.0.0:9999
auth: password
password: 123456
cert: false

sudo service code-server@$USER restart

```

{{< figure src="/0ca3f6fe-0d24-4e75-9747-2276cd33eee1.webp" >}}

## Kích hoạt INGRESS

Để kích hoạt [Inpress Controller](https://github.com/kubernetes/ingress-nginx) của Microk8s chúng ta sử dụng lệnh:

```bash
microk8s enable ingress
```

{{< figure src="/ced428f0-7ad7-4adb-b358-42a8e14265b1.webp" >}}

Sau khi kích hoạt thành công chúng ta sẽ thử tạo tiệp ingress của dashboard

Tiệp: **ingress-dashboard.yaml**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: https-ingress-dashboard
  namespace: kube-system
  annotations:
    kubernetes.io/ingress.class: public
    nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
spec:
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: kubernetes-dashboard
                port:
                  number: 443
```

Chạy lệnh `microk8s kubectl apply -f ingress-dashboard.yaml`

{{< figure src="/ad42bee3-447b-4828-a6b5-00823dc94496.webp" >}}

Khi thành công ta vào đường dẫn https://10.19.2.92/ để kiểm tra

{{< figure src="/a7694569-fa42-4520-b53e-23a4a22e0bc9.webp" >}}

## Cấu hình nginx Reverse Proxy

{{< youtube "qO-O_dK3RfU" >}}

### Bước 1: bạn vào file hosts để thêm 1 dòng ở cuối ở máy bạn đang sử dụng, ở đây mình sử dụng Ubuntu (Máy Host)

Windows Path: **C:\Windows\System32\drivers\etc\hosts**

MacOS & Linux Path: **/etc/hosts**

```bash
127.0.0.1 microk8s-dashboard.localhost
```

{{< figure src="/b3f5deb5-92d2-4d7f-8c07-5ba54fce5356.webp" >}}

### Bước 2: Cấu hình nginx

đi đến thư mục: **/etc/nginx/sites-enabled**

```bash
cd /etc/nginx/sites-enabled
```

tạo file **microk8s-dashboard.localhost**

```bash
sudo nano microk8s-dashboard.localhost
```

copy đoạn config thêm vào file **microk8s-dashboard.localhost.conf**

```bash

upstream host_mircok8s_worker {
    server 192.168.56.5;
    server 192.168.56.6;
    server 192.168.56.7;
    server 192.168.56.8;
}


server {

    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    server_name microk8s-dashboard.localhost;

    ssl_certificate /etc/nginx/ssl/localhost.crt;
    ssl_certificate_key /etc/nginx/ssl/localhost.key;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_pass http://host_mircok8s_worker;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Lưu lại và kiểm tra config xem đã chính xác chưa : **sudo nginx -t**

```nginx
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Khởi động lại service:

```bash
sudo systemctl restart nginx
```

## Cấu hình ingress-dashboard.yaml

Thêm dòng **host: kubernetes-dashboard.localhost** vào

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: https-ingress-dashboard
  namespace: kube-system
  annotations:
    kubernetes.io/ingress.class: public
    nginx.ingress.kubernetes.io/backend-protocol: "HTTPS"
spec:
  rules:
    - host: kubernetes-dashboard.localhost
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: kubernetes-dashboard
                port:
                  number: 443
```

Chạy lại config : `microk8s kubectl apply -f ingress-dashboard.yaml`

{{< figure src="/37ac3dd4-92ec-4cbe-ab88-5bd82835b86c.webp" >}}

Nếu bạn thấy bài chia sẽ này hay xin hãy cho mình một like và đăng ký để ủng hộ mình nhé. Cảm ơn các bạn nhiều ♥️♥️♥️♥️

Các bài tham khảo:

- https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04
- https://microk8s.io/docs/addon-ingress
