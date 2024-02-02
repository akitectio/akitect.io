---
categories:
  - keycloak
date: 2023-10-01T08:00:00+08:00
description: Keycloak Triển khai phiên bản mới nhất trên Kubernetes
draft: false
featuredImage: /series/keycloak-k8s.png
images:
  - /keycloak-trien-khai-phien-ban-2204-tren-microk8s/images/index.png
  - /series/keycloak-k8s.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
lightgallery: true
tags:
  - keycloak
  - kubernetes
  - Keycloak 22.0.4
  - microk8s
title: Triển khai Keycloak phiên bản 22.0.4 trên Microk8s
url: /keycloak-trien-khai-phien-ban-2204-tren-microk8s
---

Dưới đây là các bước chúng ta cần thực hiện khi triển khai keycloak trên Kubernetes.

### Bước 1: Cài đặt Microk8s

Them khảo ở bài viết: [Microk8s](/categories/microk8s)

### Bước 2: Tạo tạo file `keycloak-service.yaml`

Chúng ta cần tạo tệp dịch vụ để hiển thị ứng dụng Keycloak. Theo mặc định, Keycloak hoạt động trên cổng 8080, với phiên bản trước, chúng tôi có thể sử dụng trực tiếp `ClusterIP` và có thể có bản sao của các nhóm. Nhưng với phiên bản keycloak mới nhất, chúng tôi sẽ cần tạo dịch vụ `headless`, điều đó sẽ giúp chúng tôi có các bản sao, tức là có `HA keycloak` trên kubernetes.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: keycloak
  labels:
    app: keycloak
spec:
  ports:
    - name: http
      port: 8080
      targetPort: 8080
  selector:
    app: keycloak
  type: ClusterIP
  clusterIP: None
```

Sau khi yaml cho dịch vụ được tạo, chúng ta cần áp dụng triển khai bằng lệnh bên dưới.

```bash
microk8s kubectl apply -f service.yaml -n keycloak
```

Để kiểm tra xem dịch vụ đã được tạo hay chưa, hãy chạy lệnh bên dưới.

```bash
microk8s kubectl get svc -n keycloak
```

### Bước 3: Tạo tạo file `keycloak-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: keycloak
  labels:
    app: keycloak
spec:
  replicas: 2
  selector:
    matchLabels:
      app: keycloak
  template:
    metadata:
      labels:
        app: keycloak
    spec:
      containers:
        - name: keycloak
          image: quay.io/keycloak/keycloak:22.0.4
          args: ["start", "--cache-stack=kubernetes"]
          env:
            - name: KEYCLOAK_ADMIN
              value: "admin"
            - name: KEYCLOAK_ADMIN_PASSWORD
              value: "admin"
            - name: KC_PROXY
              value: "edge"
            - name: jgroups.dns.query
              value: "keycloak"
            - name: KC_HOSTNAME_STRICT
              value: "false"
            - name: KC_HEALTH_ENABLED
              value: "true"
            - name: KC_METRICS_ENABLED
              value: "true"
            - name: KC_HTTP_ENABLED
              value: "true"
            - name: KC_DB
              value: "postgres"
            - name: KC_DB_PASSWORD
              value: PassKhongChilaPasss
            - name: KC_DB_USERNAME
              value: postgres
            - name: KC_DB_URL_PORT
              value: "5432"
            - name: KC_DB_URL_HOST
              value: 10.86.224.19
            - name: CACHE_OWNERS_COUNT
              value: "2"
            - name: CACHE_OWNERS_AUTH_SESSIONS_COUNT
              value: "2"
          ports:
            - name: jgroups
              containerPort: 7600
            - name: https
              containerPort: 8443
            - name: http
              containerPort: 8080
          readinessProbe:
            httpGet:
              scheme: HTTP
              path: /health/ready
              port: 8080
            initialDelaySeconds: 60
            periodSeconds: 1
```

Nếu bạn đang có nhiều bản sao, tức là có nhiều hơn `1` nhóm cho keycloak và nếu bạn gặp sự cố khi truy cập ứng dụng, hãy thêm các biến env bên dưới. cập nhật giá trị theo số lượng nhóm, tức là nếu bạn có hai nhóm thì giá trị sẽ là `2`, nếu bạn có bốn nhóm thì giá trị sẽ là `4`.

```yaml
- name: CACHE_OWNERS_COUNT
  value: "2"
- name: CACHE_OWNERS_AUTH_SESSIONS_COUNT
  value: "2"
```

Sau khi yaml để triển khai được tạo, chúng ta cần áp dụng triển khai bằng lệnh bên dưới.

```bash
microk8s kubectl apply -f keycloak-deployment.yaml -n keycloak
```

Để kiểm tra xem việc triển khai đã được tạo hay chưa, hãy chạy lệnh bên dưới.

```bash
microk8s kubectl get deployments -n keycloak
```

### Bước 4: Tạo tạo file `keycloak-ingress.yaml`

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: keycloak-ingress
  annotations:
    nginx.ingress.kubernetes.io/proxy-body-size: 2500m
    nginx.ingress.kubernetes.io/proxy-buffer-size: 12k
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  ingressClassName: nginx
  rules:
    - host: keycloak.localhost
      http:
        paths:
          - backend:
              service:
                name: keycloak
                port:
                  number: 8080
            path: /(.*)
            pathType: Prefix
```

Sau khi yaml cho ingress được tạo, chúng ta cần áp dụng triển khai bằng lệnh bên dưới.

```bash
microk8s kubectl apply -f keycloak-ingress.yaml -n keycloak
```

Để kiểm tra xem lối vào đã được tạo hay chưa, hãy chạy lệnh bên dưới.

```bash
microk8s kubectl get ingress -n keycloak
```

Sau khi triển khai, dịch vụ, ingress được tạo, bạn có thể kiểm tra trạng thái của nhóm xem chúng có chạy hay không. Chạy lệnh bên dưới để kiểm tra trạng thái của nhóm.

```bash
microk8s kubectl get pods -n keycloak
```
