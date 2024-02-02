---
categories:
  - microk8s
date: 2023-01-13T08:00:00+08:00
description: Jenkins là một máy chủ tự động hóa nguồn mở, tự động hóa các tác vụ kỹ thuật lặp đi lặp lại liên quan đến việc tích hợp và phân phối phần mềm liên tục. Jenkins dựa trên Java, được cài đặt từ các gói Ubuntu hoặc bằng cách tải xuống và chạy tệp lưu trữ ứng dụng web (WAR) của mình-một bộ sưu tập các tệp tạo nên một ứng dụng web hoàn chỉnh để chạy trên máy chủ.
draft: false
featuredImage: /series/microk8s/lesson-7-configuring-jenkins-on-ubuntu-2204-and-writing-pipeline-build-service.webp
images:
  - /series/microk8s/lesson-7-configuring-jenkins-on-ubuntu-2204-and-writing-pipeline-build-service.webp
  - /bai-7-cau-hinh-jenkins-tren-ubuntu-2204-va-viet-pipeline-build-service/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - Kubernetes
  - k8s
  - ubuntu
  - virtualbox
  - virtualbox 7
  - virtualbox 7 on ubuntu 22.04
  - Jenkins Agent
  - Pipeline
title: Bài 7 - Cấu hình Jenkins trên Ubuntu 22.04 và viết Pipeline Build Service
url: /bai-7-cau-hinh-jenkins-tren-ubuntu-2204-va-viet-pipeline-build-service
weight: 7
---

rong hướng dẫn này, bạn sẽ cài đặt Jenkins trên Ubuntu 22.04, bắt đầu máy chủ phát triển và tạo người dùng quản trị để bắt đầu khám phá tự động hóa Jenkins. Vào cuối hướng dẫn này, bạn sẽ có một máy chủ Jenkins không có bảo đảm sẵn sàng để triển khai phát triển.

## Yêu Cầu hệ thống

- Một máy chủ Ubuntu 22.04 được cấu hình với người dùng sudo không root và tường lửa bằng cách làm theo hướng dẫn thiết lập máy chủ ban đầu của Ubuntu 22.04.Ở đây thì mình yêu cầu bạn nên bắt đầu với ít nhất 1 GB RAM. Bạn có thể tham khảo yêu cầu phần cứng ở đưỡng dẫn: https://www.jenkins.io/doc/book/scaling/hardware-recommendations/.
- Đã cài đặt Oracle JDK 11.

{{< youtube "fDpdOJF_BlY" >}}

### Bước 1: Cài đặt Oracle JDK 11

Để cài đặt phiên bản OpenJDK của Java, trước tiên hãy cập nhật chỉ mục gói **apt** của bạn:

```nginx
sudo apt update
```

Tiếp theo, kiểm tra xem Java đã được cài đặt chưa:

```nginx
java -version
```

Nếu Java hiện chưa được cài đặt, bạn sẽ nhận được kết quả sau:

```nginx
Output
Command 'java' not found, but can be installed with:

sudo apt install default-jre              # version 2:1.11-72build1, or
sudo apt install openjdk-11-jre-headless  # version 11.0.14+9-0ubuntu2
sudo apt install openjdk-17-jre-headless  # version 17.0.2+8-1
sudo apt install openjdk-18-jre-headless  # version 18~36ea-1
sudo apt install openjdk-8-jre-headless   # version 8u312-b07-0ubuntu1
```

Thực hiện lệnh sau để cài đặt JRE từ OpenJDK 11:

```nginx
sudo apt install openjdk-11-jre-headless
```

JRE sẽ cho phép bạn chạy hầu hết các phần mềm Java.

Xác minh cài đặt với:

```nginx
java -version
```

```nginx
Output
openjdk version "11.0.14" 2022-01-18
OpenJDK Runtime Environment (build 11.0.14+9-Ubuntu-0ubuntu2)
OpenJDK 64-Bit Server VM (build 11.0.14+9-Ubuntu-0ubuntu2, mixed mode, sharing)
```

Như vậy thì bạn đã cài đặt OpenJDK 11 thành công.

### Bước 2: cài đặt Jenkins

Phiên bản Jenkins đi kèm với các gói Ubuntu mặc định thường là phiên bản mới nhất.

Đầu tiên, thêm khóa kho lưu trữ vào hệ thống của bạn:

```nginx
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key |sudo gpg --dearmor -o /usr/share/keyrings/jenkins.gpg
```

Tiếp theo, hãy thêm địa chỉ kho lưu trữ gói Debian vào **sources.list** của máy chủ:

```nginx
sudo sh -c 'echo deb [signed-by=/usr/share/keyrings/jenkins.gpg] http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```

Phần **[signed-by=/usr/share/keyrings/jenkins.gpg]** của dòng đảm bảo rằng **apt** sẽ xác minh các tệp trong kho lưu trữ bằng khóa GPG mà bạn vừa tải xuống.

Sau khi cả hai lệnh đã được nhập, hãy chạy cập nhật **APT** để **APT** sẽ sử dụng kho lưu trữ mới.

```nginx
sudo apt update
```

Cuối cùng, cài đặt Jenkins bằng lệnh :

```nginx
sudo apt install jenkins
```

Bây giờ Jenkins và các phần phụ thuộc của nó đã sẵn sàng, chúng tôi sẽ khởi động máy chủ Jenkins

### Bước 3: Start Jenkins

Bây giờ Jenkins đã được cài đặt, hãy bắt đầu bằng cách sử dụng **systemctl**:

```nginx
sudo systemctl start jenkins.service
```

Vì **systemctl** không hiển thị đầu ra trạng thái, nên chúng tôi sẽ sử dụng lệnh status để xác minh rằng Jenkins đã bắt đầu thành công:

```nginx
sudo systemctl status jenkins
```

Nếu mọi thứ suôn sẻ, phần đầu của đầu ra trạng thái cho thấy dịch vụ đang hoạt động và được định cấu hình để bắt đầu khi khởi động:

```nginx
Output
● jenkins.service - Jenkins Continuous Integration Server
     Loaded: loaded (/lib/systemd/system/jenkins.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2023-01-19 16:07:28 UTC; 2min 3s ago
   Main PID: 88180 (java)
      Tasks: 42 (limit: 4665)
     Memory: 1.1G
        CPU: 46.997s
     CGroup: /system.slice/jenkins.service
             └─88180 /usr/bin/java -Djava.awt.headless=true -jar /usr/share/java/jenkins.war --webroot=/var/cache/jenkins/war --httpPort=8080
```

Bây giờ Jenkins đã hoạt động và chạy, bay giờ bạn cần phải bật tường lửa (Firewall) và sử dụng.

### Bước 4: Cấu hình tường lửa (Firewall)

Theo mặc định, Jenkins chạy trên cổng **8080**. Mở cổng đó bằng **ufw**:

```nginx
sudo ufw allow 8080
```

Kiểm tra trạng thái của **UFW** để xác nhận các quy tắc mới:

```nginx
sudo ufw status
```

Bạn sẽ thấy rằng các truy cập được phép chuyển đến cổng 8080 từ mọi nơi:

```nginx
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
8080                       ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
8080 (v6)                  ALLOW       Anywhere (v6)
```

Với việc cài đặt Jenkins và cấu hình tường lửa, bạn đã hoàn tất giai đoạn cài đặt và có thể tiếp tục cấu hình Jenkins.

### Bước 5: Thiết lập ban đầu cho Jenkins

Để thiết lập cài đặt của bạn, hãy truy cập Jenkins trên cổng mặc định của nó, 8080, sử dụng tên miền hoặc địa chỉ IP máy chủ của bạn: http://host:8080 (Host là ip của máy host)

Bạn sẽ nhận được màn hình Mở khóa Jenkins, hiển thị vị trí của mật khẩu ban đầu:

{{< figure src="./de4f0dc9-e268-483d-ae7e-f51b3ddcd132.webp" >}}

Trong cửa sổ terminal, sử dụng lệnh **cat** để hiển thị mật khẩu:

```nginx
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Sao chép mật khẩu chữ và số 32 ký tự từ terminal và dán nó vào trường mật khẩu , sau đó nhấp vào Tiếp tục.

Màn hình tiếp theo hiển thị tùy chọn cài đặt các plugin được đề xuất hoặc chọn các plugin cụ thể:

{{< figure src="./8a198c4d-8b5e-4185-acc1-4e29b28eff05.webp" >}}

Ở đây mình khuyên các bạn sử dụng **_Install suggested plugins_** để tiếp tục tiến trình cài đặt

{{< figure src="./e96dd023-2710-4916-a1e9-bc3d921e846f.webp" >}}

Tiếp tục chờ đợi tiến trình cài đặt

Sau khi quá trình cài đặt hoàn tất, sẽ đến bước tạo user admin

{{< figure src="./9a838a23-5d03-4058-a44a-c5083705f311.webp" >}}

Tiếp tục nhập username với password

{{< figure src="./f00d8021-76eb-4d96-8aed-2895828271bb.webp" >}}

sau đó nhấp vào "**Save and Finish**" để hoàn thành cài đặt

{{< figure src="./7e5f7ff4-359e-4b0d-a28b-96e75ab6c303.webp" >}}

Như vậy mình đã cài đặt và thiết lập ban đầu cho jenkins xong rồi, tiếp tục thì mình sẽ viết bộ build image service01 và service02

### Bước 6: Viết Code Pipeline Build Service

{{< youtube "v3cWZGKiR-s" >}}

Ở đây mình có viết trước code jenkins build và push lên git các bạn lấy code về theo github

https://github.com/akitectio/microk8s-series/tree/main/microservices

trong thư mục build sẽ có 1 file Jenkinsfile mình viết trước có cấu trúc như sau

```nginx
// Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any
    stages {
        stage('Build jar') {
            steps {
                script {
                        sh 'sh ./microservices/service01/build-image.sh' // Trỏ đến thư mục build
                }
            }
        }
    }
    post {
        always {
            deleteDir() /* clean up our workspace */
        }
    }
}
```

build-image .sh

```bash
#!/bin/bash

#remove images
docker rmi $(docker images '192.168.56.2:32000/service01' -a -q) //Xoá image cũ

docker build --no-cache=true -t 192.168.56.2:32000/service01 .  -f ./microservices/service01/Dockerfile // Build lại image mới
echo "Finish build docker images  "

echo "Begin Push Images to Local Docker Repository"

docker push "192.168.56.2:32000/service01:latest" // Đẩy image lên repository

echo "Finish Push Images to Local Docker Repository"
docker image ls

exit 0
```

Cảm ơn các bạn đã xem, bài sau mình sẽ hướng dẫn các bạn sử dụng Kong Gateway để triển khai API Gateway cho hệ thống Microservices trên 2 Image của bạn vừa build thành công
