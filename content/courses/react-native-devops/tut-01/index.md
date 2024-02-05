---
categories:
  - devops
  - react-native
date: 2023-02-01T08:00:00+08:00
description: Hướng dẫn setup jenkins agent để bắt đầu build mobile bằng jenkins cho developer an tâm phát triển sản phẩm
draft: false
featuredImage: /series/react-native-devops/lesson-1-react-native-devops-basic-concepts-and-settings.webp
images:
  - /series/react-native-devops/lesson-1-react-native-devops-basic-concepts-and-settings.webp
  - /bai-1-react-native-devops-cac-khai-niem-va-cac-cai-dat-can-ban/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - rn-devops
tags:
  - react-native
  - jenkins-agent
title: Bài 1 - React Native DevOps các khái niệm và các cài đặt căn bản
url: /bai-1-react-native-devops-cac-khai-niem-va-cac-cai-dat-can-ban
weight: 1
---

# Tóm tắt

## Các khái niệm

- Yêu cầu phần cứng
- Kiểm sót source code
- Provisioning profiles ios
- Môi trường build (environment)

## Hướng dẫn

- Thiết lập máy tính ban đầu
- Cài đặt các ứng dụng
- Thêm New node vào jenkins master
- Chạy test job

# Các khái niệm

## Tổng quan hệ thống

{{< figure src="./images/c13c1882-325a-4228-9c5b-948f24cf1766.png" >}}

## Yêu cầu phần cứng

- Ở đây mình sử dụng phần cứng là Mac Mini M1 2020 (3.2Ghz Apple M1 Chip With 8-CPU, 16G RAM, SSD 512), còn các bạn muốn sử dụng trên cloud thì có thể tham khảo máy chủ ở đây [macincloud.com](https://macincloud.com)
- Máy Mac Mini mình sẽ để ở công ty vào chỉ build được ở mạng nội bộ của công ty qua địa chỉ IP của máy, còn các bạn có thể đưa ra network bằng cách NAT ip ra mạng hoặc sử dụng dịch vụ trên Mac Cloud

## Kiểm sót source code

Hướng dẫn này mình sẽ sử dụng Github để quản lý soucre code, Jenkins và GitHub sử dụng cùng nhau mang lại sự nhất quán và tốc độ cho đội develop. Jenkins chạy tất cả các loại luồng DevOps tự động, như xác minh xây dựng hàng giờ, xây dựng hàng đêm, xây dựng yêut theo yêu cầu của bạn, và nhiều cách khác.

## Provisioning profiles ios

- Phần yêu thích nhất của tôi trong phát triển iOS là Provisioning profiles. Tôi thích phương pháp Android vì sự đơn giản của nó, đặc biệt là khi quản lý các loại build. Cách tiếp cận của Apple để xây dựng các điểm đến là hạn chế và yêu cầu thêm công việc từ nhà phát triển để kiểm tra tất cả các trường hợp.
- Để sử được được Provisioning profiles thì ta dùng [Match](https://docs.fastlane.tools/actions/match/) , lưu trữ và cập nhật hồ sơ cung cấp iOS và chứng chỉ ký kết. Tất cả các tệp được tạo bởi khớp sau đó được lưu trữ trong kho lưu trữ Github được mã hóa, với khóa giải mã được chia sẻ bởi nhóm develop.

## Môi trường build (environment)

- Phát hành một dự án React Native (RN) để sản xuất đòi hỏi rất nhiều công cụ. Ở mức tối thiểu, một bản phát hành thành công sẽ sử dụng Node.js, NPM / Yarn, Xcode và Android Studio. Các dự án kết hợp SDK của bên thứ 3 thêm độ phức tạp và công cụ bổ sung vào các bản phát hành. Quản lý sự phức tạp này là rất quan trọng để tạo ra một quy trình build.

- Điều rất quan trọng đối với tất cả các developer làm việc trong dự án là có cùng một môi trường. Tự động tăng version trong môi trường cho RN không đơn giản so với các nền tảng khác - có rất nhiều công cụ và rất nhiều nơi để phạm sai lầm nhỏ. Sự khác biệt nhỏ giữa máy phát triển và máy xây dựng có thể dẫn đến thời gian lãng phí thời gian của developer, và có khả năng ngồi bù đầu để tìm ra lỗi khi build.

- Việc thiết lập và cài đặt Jenkins buộc phải chuẩn hóa môi trường xây dựng cho chính bạn và nhóm develope. Phát triển trong cùng một môi trường chạy các bản build của bạn.

# Hướng dẫn

## Thiết lập máy tính ban đầu

### 1. Cập nhật hệ điều hành mới nhất

Để bộ build chạy mượt và đồng bộ môi trường với developer thì chúng ta cần update hệ thống bằng cách update hệ điều hành:

1. Chọn biểu tượng trái táo {{< figure src="./images/ab0708e4-0038-42ac-9c7b-aa4b7d2a3755.png" >}}
2. Chọn ‘Software Update’
3. Cập nhật macOS

### 2. Cài đặt Xcode

1. Mở ứng dụng Appstore
2. Tìm kiếm xcode
   {{< figure src="./images/f8faa7ab-26ce-450d-830e-09617aec19e3.png" >}}
3. Bấm vào tải về và cài đặt
   {{< figure src="./images/7875a55d-538a-4f2b-8494-1e53c579811c.png" >}}
4. Mở xcode hoàn thành các thiết lập ban đầu
   {{< figure src="./images/c8e3de99-87df-438e-af14-da5c793a88d5.png" >}}

Nhấp vào **_Agree_**

{{< figure src="./images/fc7199ac-63bc-45b6-8555-dd6cae7479a9.png" >}}

Nhấp vào **_Install_**

{{< figure src="./images/33aec20d-970a-4592-9798-b74032292751.png" >}}

Nhấp vào **_Continue_**

{{< figure src="./images/a1ebdd26-93b5-4f20-97a1-e74d5ab6c6d0.png" >}}

Hoàn thành thiết lập

{{< figure src="./images/2bd14f42-4bff-4c55-9c50-edaeba4338ec.png" >}}

### 3. Cài đặt xcode cli

Mở termianl và gõ lệnh

```
xcode-select --install
```

{{< figure src="./images/7736cf30-961f-47e8-8791-1f3ce0a559a6.png" >}}

Nhấp vào **_Install_**

{{< figure src="./images/61730892-9410-49e9-a602-23f769c0e96d.png" >}}

Nhấp vào **_Agree_**

{{< figure src="./images/bdd6ba93-2580-4c19-9d53-cd27204753fd.png" >}}

Cài đặt thành công

{{< figure src="./images/ab8fdb3e-c39c-4110-a660-015513c5be37.png" >}}

Ta tiếp tục dùng lệnh

```
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -license
```

{{< figure src="./images/1a985e57-59c3-4d88-a780-8a9c02559ff5.png" >}}

Bật chế độ nhà phát triển bằng lệnh

```
DevToolsSecurity -enable
```

{{< figure src="./images/d8f424bc-3305-448b-9b53-01c51ef61075.png" >}}

### 4. Cài đặt Android Studio

Vào trang chủ Android Studio để tải https://developer.android.com/studio

{{< figure src="./images/a86c6597-11a7-4423-991d-d3476fdc08fe.png" >}}

Nhấp vào **Download Android Studio Electric Eel**

{{< figure src="./images/66662f19-0d33-4bf9-819f-931cfac969d6.png" >}} vì mình đang sử dụng chi M1 của Apple

Sau khi tải xong mở tiệp vừa tải về, kéo sang thư mục Application

{{< figure src="./images/232f475d-1b74-4800-a388-c92f8908bf7e.png" >}}

sau đó mở Android sutudio trong thư mục Application

{{< figure src="./images/008de8a5-bb8e-4c83-b0da-d85e1becb5f4.png" >}}

Nhấp vào **Open**

{{< figure src="./images/b1f7f83c-2bd5-48c5-a59c-8589a0ad8628.png" >}}

Nhấp vào **Ok**

{{< figure src="./images/e090ea88-693a-4450-929f-3e6813cd8ee5.png" >}}

Nhấp vào **Next**

{{< figure src="./images/088bf7b7-2957-47ce-894c-3e0cb7fe9a82.png" >}}

Chọn **Standard**, sau đó tiếp tục nhấp vào **Next**

{{< figure src="./images/94af1101-3806-4639-abb2-ab4e0d093280.png" >}}

Nhấp vào **Next**

{{< figure src="./images/9ae4ba9d-e5be-4456-b9cb-136fe4b7f84a.png" >}}

Nhấp vào **Next**

{{< figure src="./images/7af747d3-1fa1-4c08-8afe-891a51a5613c.png" >}}

Chọn **Accept**, sau đó tiếp tục nhấp vào **Finish**

{{< figure src="./images/2a176bf1-f04b-4429-b34d-a21d5e681297.png" >}}

Nhấp vào **Finish**

{{< figure src="./images/9cf851c7-afde-400e-aa6d-ea2e8fa2b47c.png" >}}

### 5. Cài đặt Android SDK

Android Studio hiện đã được thiết lập. Thiết lập các thành phần SDK cần thiết trên màn hình chào mừng.

{{< figure src="./images/25979e9e-3052-4781-974f-b1354c1509ab.png" >}}

Nhấp vào **More Actions**, sau đó chọn **SDK Manager**, Tick theo trên hình và sau đó nhấp vào **OK**

{{< figure src="./images/d4196ebf-5592-430c-9b16-186ef008869e.png" >}}

{{< figure src="./images/207cbdbc-1ac1-422c-967d-cd39dd1415eb.png" >}}

### 6. Cài đặt phần miềm CLI

1. Homebrew

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. JDK 11

```
brew tap homebrew/cask-versions
brew install --cask zulu11
```

{{< figure src="./images/fc93bd72-0df0-4ec4-9488-920e7172770e.png" >}}

### 7. Cài đặt .zshrc

Mở tiệp .zshrc và thêm vào dòng bên dưới

```
# Android
export ANDROID_SDK="$HOME/Library/Android/sdk"
export ANDROID_SDK_TOOLS="$ANDROID_SDK/tools"
export ANDROID_SDK_TOOLS_BIN="$ANDROID_SDK_TOOLS/bin"
export ANDROID_PLATFORM_TOOLS="$ANDROID_SDK/platform-tools"
export PATH="$PATH:$ANDROID_SDK:$ANDROID_SDK_TOOLS"
export PATH="$PATH:$ANDROID_SDK_TOOLS_BIN:$ANDROID_PLATFORM_TOOLS"
# Fastlane
export LANG=en_US.UTF-8
export LANGUAGE=en_US.UTF-8
export LC_ALL=en_US.UTF-8
```

{{< figure src="./images/d1d15826-5420-4580-9620-abe3e24f6d54.png" >}}

dùng lệnh để apply cấu hình

```
source $HOME/.zshrc
```

{{< figure src="./images/0c8289c3-7536-44cb-847b-c11f485eb6e2.png" >}}

### 8. Cài đặt Jenkins

Ta cài đặt jenkins bằng lệnh

```
brew install jenkins-lts
```

{{< figure src="./images/633dd259-3919-4748-946c-52e4952fa471.png" >}}

{{< figure src="./images/9195be15-f7af-4399-ab03-677a76227ee2.png" >}}

Sau khi cài đặt thành cộng ta start bằng lệnh

```
brew services start jenkins-lt
```

{{< figure src="./images/74e1ab55-c220-4f85-bdb3-c11a85dddf1a.png" >}}

Sau đó ta vào đường dẫn http://127.0.0.1:8080/login?from=%2F để login vào hệ thống

{{< figure src="./images/0c1d06e6-f8b4-4631-b854-a1f97d3f2485.png" >}}

ta lấy password mặt định bằng lệnh

```nginx
cat /Users/duytran/.jenkins/secrets/initialAdminPassword
```

{{< figure src="./images/2fbda5d8-8a5f-4c73-ad57-d4cc542e95e7.png" >}}

Cấu hình public ip kết nối vào bằng cách mở tất cả ip kết nối đến jenkins

```shell
#nano /usr/local/opt/jenkins-lts/homebrew.mxcl.jenkins-lts.plist

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>homebrew.mxcl.jenkins-lts</string>
	<key>LimitLoadToSessionType</key>
	<array>
		<string>Aqua</string>
		<string>Background</string>
		<string>LoginWindow</string>
		<string>StandardIO</string>
		<string>System</string>
	</array>
	<key>ProgramArguments</key>
	<array>
		<string>/usr/local/opt/openjdk@17/bin/java</string>
		<string>-Dmail.smtp.starttls.enable=true</string>
		<string>-jar</string>
		<string>/usr/local/opt/jenkins-lts/libexec/jenkins.war</string>
		<string>--httpListenAddress=0.0.0.0</string> # <= Thêm ở đây
		<string>--httpPort=8080</string>
	</array>
	<key>RunAtLoad</key>
	<true/>
</dict>
</plist>
```

Sau đó restart service và kết nối bình thường bằng ip của mạng

```shell
brew services restart jenkins-lt
```

### 9. Cài đặt fastlane

Ta cài đăt bằng lệnh fastlane

```shell
brew install fastlane
```

{{< figure src="./images/7330aeff-0a73-4ec5-b78a-f4b74e5b1264.png" >}}

{{< figure src="./images/b3aadf4a-ccc0-4f76-91e0-1f495e19de89.png" >}}

Như vậy bạn đã hiểu và cài đặt được cơ bản server build jenkins cho React native, bài tiếp theo mình sẽ hướng dẫn build đẩy lên firebase distribution
