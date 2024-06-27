---
categories:
  - microk8s
date: 2023-01-15T08:00:00+08:00
description: Để phát hiện và ngăn ngừa lỗi, sẽ rất thuận tiện nếu có một công cụ giám sát tốt . Các hệ thống giám sát chịu trách nhiệm giám sát công nghệ mà một công ty sử dụng (phần cứng, mạng và thông tin liên lạc, hệ điều hành hoặc ứng dụng, trong số những thứ khác) để phân tích hiệu suất của nó, đồng thời phát hiện và cảnh báo về các lỗi có thể xảy ra. Một hệ thống giám sát tốt có khả năng giám sát các thiết bị, cơ sở hạ tầng, ứng dụng, dịch vụ và thậm chí cả quy trình kinh doanh .
draft: false
featuredImage: /series/microk8s/lesson-9-using-the-monitoring-system-integration-elasticsearch-fluentd-and-kibana-grafana-zipkin-of-microk8s.webp
images:
  - /series/microk8s/lesson-9-using-the-monitoring-system-integration-elasticsearch-fluentd-and-kibana-grafana-zipkin-of-microk8s.webp
  - /bai-9-su-dung-bo-tich-hop-monitoring-system-elasticsearch-fluentd-and-kibana-grafana-zipkin-cua-microk8s/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - kubernetes
  - k8s
  - ubuntu
  - virtualbox
  - grafana
  - zabbix-agent
  - prometheus
  - alertmanager
title: Bài 8 - Sử dụng bộ tích hợp Monitoring System (Elasticsearch, Fluentd and Kibana, Grafana, Zipkin) của Microk8s
url: /bai-9-su-dung-bo-tich-hop-monitoring-system-elasticsearch-fluentd-and-kibana-grafana-zipkin-cua-microk8s
weight: 9
---

# Tầm quan trọng của việc có một hệ thống giám sát tốt

Ngày nay, thật khó để tìm thấy một công ty không sử dụng công nghệ.

Trên thực tế, hiệu suất của thiết bị, mạng và hệ thống phù hợp là điều cần thiết để các doanh nghiệp tiếp tục hoạt động. Ở những nơi khác, những gì sẽ được cung cấp cho khách hàng sẽ là các dịch vụ công nghệ ngay lập tức.

Mặc dù công nghệ là cần thiết cho công việc của bất kỳ công ty nào, nhưng điều đó không có nghĩa là nó không thể sai lầm.

Bất cứ lúc nào thất bại có thể xảy ra làm phát sinh các tình huống đôi khi quan trọng.

Do đó, trong bất kỳ công ty nào có cơ sở hạ tầng CNTT là quan trọng, cần phải giám sát hoạt động chính xác của nó để một lỗi có thể xảy ra không ảnh hưởng đến dịch vụ được cung cấp cho người dùng.

### Tại sao phải sử dụng hệ thống giám sát?

Để phát hiện và ngăn ngừa lỗi, sẽ rất thuận tiện nếu có một công cụ giám sát tốt . Các hệ thống giám sát chịu trách nhiệm giám sát công nghệ mà một công ty sử dụng (phần cứng, mạng và thông tin liên lạc, hệ điều hành hoặc ứng dụng, trong số những thứ khác) để phân tích hiệu suất của nó, đồng thời phát hiện và cảnh báo về các lỗi có thể xảy ra. Một hệ thống giám sát tốt có khả năng giám sát các thiết bị, cơ sở hạ tầng, ứng dụng, dịch vụ và thậm chí cả quy trình kinh doanh .

Về cơ bản, được dịch sang ngôn ngữ của công ty, một hệ thống giám sát tốt sẽ giúp tăng năng suất . Điều này được thể hiện qua nhiều khía cạnh:

**1. Cải thiện việc sử dụng phần cứng**

Bằng cách giám sát hoạt động tốt của nó, bạn sẽ quản lý để sử dụng thiết bị của mình tốt hơn. Ví dụ, nếu một thiết bị không hoạt động bình thường, hệ thống giám sát sẽ phát hiện ra nó, đưa ra thông báo về nó và có thể đưa ra quyết định sửa chữa hoặc thay thế nó.

**2. Ngăn ngừa sự cố và giúp phát hiện sớm hơn**

Và điều đó tiết kiệm thời gian và tiền bạc!, Hãy tưởng tượng rằng doanh nghiệp của bạn là một cửa hàng điện tử và trang web không hoạt động tốt hoặc bị lag rất nhiều. Nếu không có hệ thống giám sát, bạn có thể mất hàng giờ để nhận ra vấn đề (có thể bạn sẽ làm điều đó vì khiếu nại của người dùng), điều này có thể gây ra tổn thất tiền bạc đáng kể. Một hệ thống giám sát tốt có thể cảnh báo bạn về các vấn đề ngay khi chúng phát sinh, cho phép bạn giải quyết vấn đề ngay lập tức và giảm thiểu thời gian trang của bạn ngừng hoạt động hoặc chạy chậm.

**3. Hình ảnh công ty được cải thiện**

Bằng cách tránh các lỗi dịch vụ hoặc giảm thiểu thời gian giải quyết, hình ảnh của công ty sẽ được cải thiện đối với người dùng .

Hãy quay lại ví dụ trước.

Nếu cửa hàng trực tuyến của bạn ngừng hoạt động trong một thời gian dài, bạn sẽ không chỉ mất doanh số bán hàng đáng lẽ sẽ kiếm được vào ngày hôm đó mà rất có thể người dùng sẽ nghĩ rằng công việc kinh doanh của bạn không suôn sẻ. Vì vậy, họ sẽ quên bạn như một lựa chọn nghiêm túc và đáng tin cậy để mua hàng trực tuyến.

Ngoài ra, họ có thể thảo luận về vấn đề trục trặc đó với bạn bè, người quen, gia đình hoặc thậm chí trên mạng xã hội, điều này có nghĩa là quảng cáo tiêu cực có thể gây tổn hại thêm cho doanh nghiệp của bạn.

**4. Tốn ít thời gian giám sát hệ thống hơn**

Mất ít thời gian hơn để theo dõi hoạt động của hệ thống thích hợp , chính xác là vì hệ thống giám sát sẽ đảm nhận việc đó.

Bằng cách đó, nhân viên có trình độ của bạn sẽ có thể dành nhiều thời gian hơn cho các nhiệm vụ khác, biết rằng, nếu có vấn đề phát sinh, họ sẽ nhận được các cảnh báo tương ứng. Điều này cũng sẽ dẫn đến tăng năng suất!

# Kích hoạt bộ công cụ Monitoring System

## Kích hoạt addons observability

### Bước 1: Đầu tiên chúng ta kết nối vào vm **microk8s-master-01**

```bash
ssh ubuntu@192.168.56.2
```

Sau khi ssh thành công ta kích hoạt **addons observability** bằng lệnh

```bash
microk8s enable observability
```

{{< figure src="/c23304c3-1579-4e8e-a7e2-41aaf7484a45.webp" >}}

Khi apply thanh công dưới cùng thì sẽ có username vs password mặt định

### Bước 2: port-forwar port 3000

Để truy cập được service thì mình dùng port-forward bằng lệnh:

```bash
microk8s kubectl port-forward -n observability service/kube-prom-stack-grafana --address 0.0.0.0 3000:80
```

{{< figure src="/8160b5a3-5d57-4e09-a5ec-fb3228897909.webp" >}}

### Bước 3: Login vào admin của Grafana

Truy cập vào đường dẫn : http://192.168.56.2:3000

{{< figure src="/bb154f4c-473b-4814-87b1-7bd7244e72ea.webp" >}}

username: **admin**

password: **prom-operator**

Khi đăng nhập thành công thì ta vào phần dashboard: http://192.168.56.2:3000/dashboards

{{< figure src="/849f7781-13ce-4729-8a03-ebe841a3f5a9.webp" >}}

Ở đây có danh sách dashboard template đã được thiết kế sẳng, mình có thể sài luôn, hoặc có thể viết thêm theo nhu cầu của mình

{{< figure src="/974abdf2-3bbf-4b30-b95f-5f7e0b3179b7.webp" >}}

### Monitoring System bằng Zabbix 6.2

Bạn vui lòng tham khảo theo Series mình đã chia sẽ từ trước

https://akitect.io/courses/zabbix/

### Kích hoạt addons community fluentd (Elasticsearch, Fluentd and Kibana)

Đầu tiên ta enable community bằng lệnh

```
 microk8s enable community
```

{{< figure src="/8797a148-841f-4f82-9c8c-1886ccd25e2e.webp" >}}

sau khi enable community thành công thì ta kích hoạt **addons fluentd** bằng lệnh

```
microk8s enable fluentd
```

{{< figure src="/580d8f45-21d8-4d47-99f8-7f1532d5524a.webp" >}}

khi apply thành công thì ta đợi tầm 1p để up các service lên, tiếp tục ta dùng port forward để vào service kibana

```bash
microk8s kubectl port-forward -n kube-system service/kibana-logging --address 0.0.0.0 8181:5601
```

{{< figure src="/1c216157-8a40-40fa-9f12-c8fa399173fb.webp" >}}

Ta vào đường dẫn : http://192.168.56.2:8181 để vào kibana

{{< figure src="/0317007a-870a-4911-9ecb-9229ba2574da.webp" >}}

Như vậy ta đã kích hoạt thành công **addons fluentd**

## Cài đặt Zipkin

Lưu ý phải kích hoạt trước **addons community fluentd** , chúng ta bắt đầu cài đặt **zipkin** bằng lệnh:

```bash
microk8s kubectl apply -f https://gist.githubusercontent.com/akitectio/e979e6d36c7b03d6b41160b470ec70fa/raw/e8e71b14cbb013204262409aaa46a899a29ab64e/zipkin-all-in-one.yaml
```

{{< figure src="/36336ccc-8c97-4790-b5fe-4c976b8a6fb3.webp" >}}

Nội dung tiệp **zipkin-all-in-one.yaml**

{@embed: https://gist.github.com/tdduydev/e979e6d36c7b03d6b41160b470ec70fa}

Sau khi kích hoạt thành công ta dùng port forward để vào service:

```bash
 microk8s kubectl port-forward -n default service/zipkin --address 0.0.0.0 9411:9411
```

Ta vào đường dẫn : http://192.168.56.2:9411 để vào zipkin

{{< figure src="/70fcd036-8b16-4149-9f42-5e3fa6396668.webp" >}}

Như vậy bạn đã cài đặt thành công **Zipkin**

Để sử dụng Zipkin và Kong bạn vui lòng tham khảo [Bài 8]

## Kết Luận

Như vậy mình đã cài đặt thành công bộ Monitoring System chưa đầy 15p, ưu điểm khi mình có thể control toàn bộ hệ thống và chủ động khi xẩy ra lỗi, không bị khách hàng phàn nàn
