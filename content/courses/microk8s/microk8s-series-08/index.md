---
categories:
  - microk8s
date: 2023-01-14T08:00:00+08:00
description: Chào các bạn, Tiếp tục series microk8s thì đây là phần mình Reseach lâu nhất và cách deploy một kiến trúc microservices
draft: false
featuredImage: /series/microk8s/lesson-8-using-kong-gateway-to-deploy-api-gateway-for-microservices-system-on-microk8s.webp
images:
  - /series/microk8s/lesson-8-using-kong-gateway-to-deploy-api-gateway-for-microservices-system-on-microk8s.webp
  - /bai-8-dung-kong-gateway-de-trien-khai-api-gateway-cho-he-thong-microservices-tren-microk8s/images/index.png

license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - Kubernetes
  - k8s
  - ubuntu
  - virtualbox
  - kong-api-gateway
title: Bài 8 - Dùng Kong Gateway để triển khai API Gateway cho hệ thống Microservices trên Microk8s
url: /bai-8-dung-kong-gateway-de-trien-khai-api-gateway-cho-he-thong-microservices-tren-microk8s
weight: 8
---

# KONG là gì?

- Kong là bộ Microservice API Gateway. Kong cung cấp một lớp cấu hình linh hoạt quản lý an toàn giao tiếp giữa máy khách và microservice thông qua API. Còn được gọi là cổng API, phần mềm trung gian API hoặc trong một số trường hợp lưới dịch vụ. Nó có sẵn dưới dạng dự án nguồn mở vào năm 2015, các giá trị cốt lõi của nó là hiệu suất cao và khả năng mở rộng.

- Kong là một ứng dụng Lua chạy trong Nginx và được thực hiện nhờ lua-nginx-module

## Tại sao chúng ta lại sử dụng KONG

Nếu bạn đang xây dựng cho Web, Mobile hoặc IoT (Internet of Things), bạn có thể sẽ cần phải có chức năng chung để chạy phần mềm thực tế của bạn. Kong có thể giúp bằng cách đóng vai trò là cổng (hoặc sidecar) cho các yêu cầu microservice trong khi cung cấp cân bằng tải, ghi nhật ký, xác thực, giới hạn tốc độ, biến đổi và nhiều hơn nữa thông qua các plugin.

{{< figure src="images/476634ea-51fa-44c2-835f-ffc6b78efb53.png" >}}

Nhưng chắc chắn, Kong là công cụ mẫu sẽ giúp tăng tốc thời gian phát triển và nó là hỗ trợ các plugin có thể định cấu hình. Và cộng đồng hỗ trợ phát triển và làm cho nó ổn định.

- Rất nhiều plugin xác thực

  1. JWT
  2. LDAP (được sử dụng nhiều nhất)
  3. OAuth2

- Puglin Bảo mật:

  1. CL
  2. CORS
  3. Dynamic SSL
  4. IP Restriction

- Plugin kiểm soát Traffic là một rất hữu ích cho chi phí hạn chế như giới hạn tỷ lệ, giới hạn kích thước yêu cầu, giới hạn tốc độ phản hồi và những người khác.
- Plugin Analytics and monitoring trực quan hóa, kiểm tra và giám sát lưu lượng API như Prometheus, Data Dog và RunScope.
- Transformation plugin that transform request and responses on the fly such as Request Transformer, Response Transformer.
- Logging plugin that log request and response data using the best transport for your infrastructure: TCP, UDP, HTTP, StatsD, Syslog and others.

Mình đã giới thiệu sơ lượt về Kong API Gateway , bạn có thể tham khảo thêm ở trang chủ https://konghq.com/

Bây giờ mình thực hiện cấu hình như sau:

Trong bài trước thì mình đã hướng dẫn các bạn build 2 image service01 và service02.

[Bài 7: Cấu hình Jenkins trên Ubuntu 22.04 và viết Pipeline Build Service](/bai-7-cau-hinh-jenkins-tren-ubuntu-2204-va-viet-pipeline-build-service)

| IP           | Hostname          | vCPU   | RAM | DISK     |
| ------------ | ----------------- | ------ | --- | -------- |
| 127.0.0.1    | Host              | 8 core | 32G | SSD 500G |
| 192.168.56.2 | microk8s-master-1 | 1 core | 2G  | 50G      |
| 192.168.56.3 | microk8s-master-2 | 1 core | 2G  | 50G      |
| 192.168.56.4 | microk8s-master-3 | 1 core | 2G  | 50G      |
| 192.168.56.5 | microk8s-worker-1 | 1 core | 2G  | 50G      |
| 192.168.56.6 | microk8s-worker-2 | 1 core | 2G  | 50G      |
| 192.168.56.7 | microk8s-worker-3 | 1 core | 2G  | 50G      |
| 192.168.56.8 | microk8s-worker-4 | 1 core | 2G  | 50G      |

Hiện tại Kong hỗ trợ cài đặt có dùng databases (DB-mode) hoặc không dùng database (DB-less-mode)

{{< figure src="./images/3e27029b-d779-48ad-b196-07bfe027881f.webp" >}}

Ở đây mình sử dụng DB-less-mode để lưu cấu hình Kong

{{< youtube "hYHbbyWyVTQ" >}}

### Bước 1: Cấu hình Kong Controller Ingress

```
ssh ubuntu@192.168.56.2
```

Chúng ta tiến hình apply kong controller ingress, ở đây mình có nghiên cứu nhiều bài viết và đưa ra một file chuẩn bên dưới, các bạn có thể lấy chạy luôn hoặc là custom theo yêu cầu của các bạn, để thực hiện ta sử dụng lệnh như sau

```
microk8s kubectl apply -f https://gist.githubusercontent.com/tdduydev/2b875c579a2ef53f68053e4e5af5b90b/raw/deef2792d33db7fb62c784421c447731f1d5808d/all-in-one-dbless.yaml
```

{{< figure src="./images/866cebdd-e19c-4b8a-a4a4-89e9908983a9.webp" >}}

Nội dung tiệp **all-in-one-dbless.yaml**

{{< gist tdduydev 2b875c579a2ef53f68053e4e5af5b90b >}}

Tiếp tục apply cấu hình kong ingress

```nginx
microk8s kubectl apply -f https://gist.githubusercontent.com/tdduydev/1d8c8f270e28d022cd6cdfb06b6a4484/raw/02ba2bdc5c5c6ae0996a40340e6f3a0e6bc965c2/kong-ingress.yaml
```

{{< figure src="./images/63fdc794-1f9b-4308-8029-45d2e40f23a5.webp" >}}

Nội dung tiệp **kong-ingress.yaml**

{{< gist tdduydev 1d8c8f270e28d022cd6cdfb06b6a4484 >}}

### Bước 2: Cấu hình service01 và service02

**Service01**

```nginx
microk8s kubectl apply -f  https://gist.githubusercontent.com/tdduydev/e1e50da183eff226f7b2fc5e39781d8c/raw/97eb61e0debbd76a9c42f61e01e47054653334c9/service01-deployment.yaml
```

{{< figure src="./images/bd8a95fc-8a48-4a39-bc12-2f0440091feb.webp" >}}

Nội dung tiệp **service01-deployment.yaml**

{{< gist tdduydev e1e50da183eff226f7b2fc5e39781d8c >}}

**Service02**

```nginx
microk8s kubectl apply -f  https://gist.githubusercontent.com/tdduydev/e3b20b53d33302d654cd1b058a283562/raw/44553ddf0f142431fc1c394a25e89c8a4e513255/service02-deployment.yaml
```

{{< figure src="./images/2f06a6d8-48a5-45bc-8ad3-5b7f36d85bdb.webp" >}}

Nội dung tiệp **service02-deployment.yaml**

{{< gist tdduydev e3b20b53d33302d654cd1b058a283562 >}}

### Bước 3: Cấu hình Kong ingress cho Service01 và Service02

**Service01**

```
 microk8s kubectl apply -f https://gist.githubusercontent.com/tdduydev/0f43b50aeed7d1a2cd51cc6dcf307ace/raw/f70bab1ac04d98280777e21821ca530690403285/kong-ingress-service01.yaml
```

{{< figure src="./images/78eb7dfc-2bc8-4221-ba95-843ae7495c93.webp" >}}

Nội dung tiệp **kong-ingress-service01.yaml**

{{< gist tdduydev 0f43b50aeed7d1a2cd51cc6dcf307ace >}}

**Service02**

```
 microk8s kubectl apply -f https://gist.githubusercontent.com/tdduydev/6e366ab62b4d01e84352f3e616627201/raw/a49523cc901736875e7af33e0b54d633fc2c927c/kong-ingress-service02.yaml
```

{{< figure src="./images/db441b0a-844e-409c-94b5-c6bcab88ea45.webp" >}}

Nội dung tiệp **kong-ingress-service02.yaml**

{{< gist tdduydev 6e366ab62b4d01e84352f3e616627201 >}}

Sau khi apply cấu hình thành công ta check cấu hình có đúng hay không bằng cách vào đường dẫn:

1. http://192.168.56.5/service01

{{< figure src="./images/bbaa70ab-5f01-475d-8603-b5a6cc92f298.webp" >}}

3. http://192.168.56.5/service02

{{< figure src="./images/f31d0d66-d48a-4e24-b256-79689c8a39c7.webp" >}}

### Cấu hình Custom Config Kong ingress

Ở đây mình tăng set connect_timeout, read_timeout, write_timeout các bạn dùng lệnh

```
microk8s kubectl apply -f https://gist.githubusercontent.com/tdduydev/61b2dce63558ccfed18b6095abc06ed6/raw/6cca022d014c919153ef66a8bd8eb5634c4cec28/timeout-kong-ingress.yaml
```

{{< figure src="./images/25b93eaa-60f9-455a-ba7e-792f9e8d18e4.webp" >}}

Nội dung file **timeout-kong-ingress.yaml**

{{< gist tdduydev 61b2dce63558ccfed18b6095abc06ed6 >}}

Như vậy mình đã tăng timeout của ingress thành công

Các bạn có thể tìm hiểu thêm ở đường dẫn : https://docs.konghq.com/kubernetes-ingress-controller/latest/references/custom-resources/

### Cấu hình Kubernetes Ingress Controller annotations

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: service02
  namespace: kong
  annotations: #<- Thêm vào đây
    konghq.com/strip-path: "true" #
    konghq.com/preserve-host: "false
spec:
    ingressClassName: kong
    rules:
      - http:
          paths:
          - path: /service02
            pathType: Prefix
            backend:
              service:
                name: service02
                port:
                  number: 5000
```

Các bạn tìm ở đây các cấu hình tường service của mình cần: https://docs.konghq.com/kubernetes-ingress-controller/latest/references/custom-resources/

### Cấu hình plugin zipkin distributed tracing system

Lưu ý bạn cần cài đặt trước zipkin server, mình có hướng dẫn ở [bài 7].

Chúng ta apply cấu hình Kong bằng lệnh

```nginx
 microk8s kubectl apply -f https://gist.githubusercontent.com/tdduydev/c5af0e748628ca778740451da9e1d783/raw/62ca7641ebe3d939a93c53a91cceb734d8893c3e/kong-plugin-zipkin.yaml
```

{{< figure src="./images/6adc3546-8021-4d63-ba6d-4d033832ec36.webp" >}}

Nội dung tiệp **kong-plugin-zipkin.yaml**

{{< gist tdduydev c5af0e748628ca778740451da9e1d783 >}}

Như vậy bạn đã cấu hình thành công kong plugin zipkin, bây giờ bạn có thể theo dõi tất cả request tới api thông qua Kong

{{< figure src="./images/6adc3546-8021-4d63-ba6d-4d033832ec36.webp" >}}

Như vậy các bạn đã tiến hành cài đặt thành công Kong Api Gateway
