---
draft: false
featuredImage: /labs/keycloak/helm-charts-to-deploy-keycloak-in-kubernetes.webp
images:
  - /labs/keycloak/helm-charts-to-deploy-keycloak-in-kubernetes.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
- keycloak
- helm-chart
title: Triển khai Keycloak bằng Helm Chart trên Kubernetes
description: Triển khai Keycloak bằng Helm Chart trên Kubernetes cho phép bạn quản lý Keycloak một cách hiệu quả. Dưới đây là hướng dẫn chi tiết từng bước để triển khai Keycloak với Helm Chart.
lastmod: 2024-07-11T10:23:30-09:00
---

Triển khai Keycloak bằng Helm Chart trên Kubernetes cho phép bạn quản lý Keycloak một cách hiệu quả. Dưới đây là hướng dẫn chi tiết từng bước để triển khai Keycloak với Helm Chart.

### Bước 1: Cài đặt Helm

Nếu chưa cài đặt Helm, bạn có thể cài đặt theo hướng dẫn [tại đây](https://helm.sh/docs/intro/install/).

### Bước 2: Thêm Repository Bitnami

```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

### Bước 3: Tạo file `values.yaml`

File `values.yaml` chứa các cấu hình tuỳ chỉnh cho Keycloak. Tạo file này với nội dung:

```yaml
auth:
  adminUser: custom-admin
  adminPassword: custom-password

externalDatabase:
  host: your-postgresql-host
  port: 5432
  user: your-postgresql-user
  password: your-postgresql-password
  database: your-postgresql-database

postgresql:
  enabled: false
```

### Bước 4: Cài đặt Keycloak

Sử dụng Helm để cài đặt Keycloak với các cấu hình tuỳ chỉnh từ `values.yaml`.

```sh
helm install keycloak bitnami/keycloak -f values.yaml
```

### Bước 5: Kiểm tra trạng thái triển khai

Đảm bảo rằng các Pod của Keycloak đang chạy.

```sh
kubectl get pods
```

### Bước 6: Truy cập Keycloak

Để truy cập Keycloak, bạn có thể dùng lệnh port-forward:

```sh
kubectl port-forward svc/keycloak 8080:80
```

Truy cập `http://localhost:8080` để vào giao diện quản trị Keycloak.

### Bước 7: Cấu hình Ingress (Tùy chọn)

Để truy cập Keycloak thông qua một tên miền, bạn cần cấu hình Ingress. Tạo một file `ingress.yaml` với nội dung:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: keycloak-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  rules:
  - host: keycloak.yourdomain.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: keycloak
            port:
              number: 80
  tls:
  - hosts:
    - keycloak.yourdomain.com
    secretName: keycloak-tls
```

Áp dụng Ingress:

```sh
kubectl apply -f ingress.yaml
```

### Chi tiết cấu hình `values.yaml`

- `auth.adminUser` và `auth.adminPassword`: Thiết lập tên người dùng và mật khẩu cho tài khoản quản trị Keycloak.
- `externalDatabase`: Cấu hình để sử dụng cơ sở dữ liệu PostgreSQL bên ngoài.
  - `host`: Địa chỉ máy chủ PostgreSQL.
  - `port`: Cổng kết nối PostgreSQL (mặc định là 5432).
  - `user`: Tên người dùng PostgreSQL.
  - `password`: Mật khẩu PostgreSQL.
  - `database`: Tên cơ sở dữ liệu PostgreSQL.
- `postgresql.enabled`: Thiết lập giá trị `false` để không sử dụng PostgreSQL đi kèm Helm Chart của Bitnami.

### Kết luận

Bằng cách sử dụng Helm Chart từ Bitnami, bạn có thể triển khai Keycloak nhanh chóng và dễ dàng trên Kubernetes. Việc tuỳ chỉnh cấu hình thông qua `values.yaml` giúp bạn linh hoạt trong việc quản lý và triển khai Keycloak theo nhu cầu cụ thể của mình. Sử dụng Ingress để cấu hình truy cập từ bên ngoài cũng giúp bạn quản lý dịch vụ Keycloak một cách thuận tiện hơn.