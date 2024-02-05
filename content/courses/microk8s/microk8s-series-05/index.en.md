---
categories:
  - microk8s
date: 2023-01-07T08:00:00+08:00
description: In this lesson, I need to add 1 VM to install Nginx to loadbalancer here I will create 1 more VM ubuntu 22.04
draft: false
featuredImage: /series/microk8s/lesson-5-install-and-use-ingress-in-the-microk8s-integrator.webp
images:
  - lesson-5-install-and-use-ingress-in-the-microk8s-integrator.webp
  - /lesson-5-install-and-use-ingress-in-the-microk8s-integrator/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - kubernetes
  - k8s
  - ubuntu
  - virtualbox
title: Lesson 5 - Install and use ingress in the Microk8s integrator
url: /lesson-5-install-and-use-ingress-in-the-microk8s-integrator
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

## Install Nginx on Ubuntu 22.04

{{< youtube "GBJrgIYU3aI" >}}

### Bước 1 – Cài đặt Nginx

### Step 1 - Install Nginx

Update the apt installation packages

```nginx
sudo apt update
```

Install Nginx

```nginx
sudo apt install nginx -y
```

{{< figure src="./9483e7a2-849f-4e2d-82ab-c4f6fe4ce3af.webp" >}}

### Step 2 – Authorize HTTP Firewall

```shell
sudo ufw allow 'Nginx HTTP'
```

### Step 3 – Test your Web Server

Kiểm tra service nginx có hoạt đông không?

{{< figure src="./a1a7fe65-fc40-4b1b-a890-c5da04c2f000.webp" >}}

## To facilitate code administration on the master machine and testing, I will install the code-server on the microk8s-master-01 machine

{{< youtube "-zkiQWYhRfg" >}}

To install, run the command:

```nginx
curl -fsSL https://code-server.dev/install.sh | sh

sudo systemctl enable --now code-server@$USER

```

After installation is complete, go to the config folder

```nginx
nano ~/.config/code-server/config.yaml

bind-addr: 0.0.0.0:9999
auth: password
password: 123456
cert: false

sudo service code-server@$USER restart

```

{{< figure src="./0ca3f6fe-0d24-4e75-9747-2276cd33eee1.webp" >}}

## Kích hoạt INGRESS

To enable [Inpress Controller](https://github.com/kubernetes/ingress-nginx) of Microk8s we use the command:

```nginx
microk8s enable ingress
```

{{< figure src="./ced428f0-7ad7-4adb-b358-42a8e14265b1.webp" >}}

After successful activation, we will try to create a dashboard ingress file: **ingress-dashboard.yaml**

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

Run command `microk8s kubectl apply -f ingress-dashboard.yaml`

{{< figure src="./ad42bee3-447b-4828-a6b5-00823dc94496.webp" >}}

When successful, go to https://10.19.2.92/ to check

{{< figure src="./a7694569-fa42-4520-b53e-23a4a22e0bc9.webp" >}}

## Configure nginx Reverse Proxy

{{< youtube "qO-O_dK3RfU" >}}

### Step 1: go to the hosts file to add a line at the end of the machine you are using, here I use Ubuntu (Host Machine)

Windows Path: **C:\Windows\System32\drivers\etc\hosts**

MacOS & Linux Path: **/etc/hosts**

```nginx
127.0.0.1 microk8s-dashboard.localhost
```

{{< figure src="./b3f5deb5-92d2-4d7f-8c07-5ba54fce5356.webp" >}}

### Step 2: Configure nginx

đi đến thư mục: **/etc/nginx/sites-enabled**

```nginx
cd /etc/nginx/sites-enabled
```

create file **microk8s-dashboard.localhost**

```nginx
sudo nano microk8s-dashboard.localhost
```

Copy the config and add it to the file **microk8s-dashboard.localhost.conf**

```nginx

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

Save and check the config to see if it is correct: **sudo nginx -t**

```nginx
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Restart the service:

```nginx
sudo systemctl restart nginx
```

## Cấu hình ingress-dashboard.yaml

Add the line **host: kubernetes-dashboard.localhost**

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

Run the config again: `microk8s kubectl apply -f ingress-dashboard.yaml`

{{< figure src="./37ac3dd4-92ec-4cbe-ab88-5bd82835b86c.webp" >}}

If you find this sharing article interesting, please give me a like and subscribe to support me. Thank you so much ♥️♥️♥️♥️

Reference articles:

- https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04
- https://microk8s.io/docs/addon-ingress
