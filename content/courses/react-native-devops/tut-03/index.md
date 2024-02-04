---
categories:
  - devops
  - react-native
date: 2023-02-03T08:00:00+08:00
description: Để tiện cho đội tester test sản phẩm và tự động hơn nên chúng ta cần làm CI - CD
draft: false
featuredImage: /series/react-native-devops/lesson-3-register-firebase-and-configure-fastlane-to-push-apk-to-firebase-distribution-test-version.webp
images:
  - /series/react-native-devops/lesson-3-register-firebase-and-configure-fastlane-to-push-apk-to-firebase-distribution-test-version.webp
  - /bai-3-dang-ky-firebase-va-cau-hinh-fastlane-day-apk-len-firebase-distribution-phien-ban-thu-nghiem/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - rn-devops
tags:
  - react-native
  - Jenkins Agent
  - Build React Native app
  - build reactjs
  - build react native
title: Bài 3 - Đăng ký Firebase và cấu hình Fastlane đẩy APK lên Firebase Distribution (Phiên bản thử nghiệm)
url: /bai-3-dang-ky-firebase-va-cau-hinh-fastlane-day-apk-len-firebase-distribution-phien-ban-thu-nghiem
weight: 3
---

# Tóm tắt

## Các khái niệm

- Firebase là một phần mềm phát triển ứng dụng do Google hỗ trợ cho phép các nhà phát triển phát triển các ứng dụng iOS, Android và web. Firebase cung cấp các công cụ để theo dõi các phân tích, báo cáo và sửa chữa các sự cố ứng dụng, tạo thử nghiệm tiếp thị và sản phẩm.

- APK là viết tắt của **Android Package** (đôi khi là **Android Package Kit** hoặc **Android Application Package** ). Đó là định dạng tệp mà Android sử dụng để phân phối và cài đặt ứng dụng. Do đó, APK chứa tất cả các yếu tố mà một ứng dụng cần cài đặt chính xác trên thiết bị của bạn.

- IPA là một tiện ích mở rộng cho tệp gói App Store iOS là tệp lưu trữ ứng dụng được sử dụng để phân phối các ứng dụng trên iOS. IPA chứa các tệp ở dạng không nén, chỉ có thể được cài đặt trên iOS.

# Hướng dẫn

## Đăng ký Firebase

- Chúng ta vào trang chủ của Firebase để đăng ký: https://console.firebase.google.com/u/0/

{{< figure src="./images/3da88c64-df98-4a25-89a9-3eff4b318e04.png" >}}

sau khi login thành công chúng ta cọn "**Add Project**"

{{< figure src="./images/2228fac0-e13e-451d-b22a-ce8f10b0b88a.png" >}}

- Nhập tên project, ở đây mình sẽ đặt tên là "**React Native DevOps**"

{{< figure src="./images/6a242cbc-ef9e-4bda-bab7-12d3e4b6e7b1.png" >}}

- Tiếp tục nhấp vào **Continue**

{{< figure src="./images/2c5ad938-c378-4836-b269-11c809ca075a.png" >}}

- Tiếp tục nhấp vào **Continue**

{{< figure src="./images/6fb20016-0e6e-457d-b13c-761cbd6b5eb6.png" >}}

- Ở bước này chúng ta chọn account của GA or là tạo account mới cho GA, sau dó nhấp vào "Create project"

{{< figure src="./images/daf6d2e6-4037-48e5-86e9-9278d0d84ca5.png" >}}

- Tiếp tục nhấp vào **Continue**, Như vậy bạn đã tạo thành công project trên Firebase

{{< figure src="./images/1ee79b99-1865-4450-bf1b-dcafc542bd5e.png" >}}

## Cấu hình Firebase cho Android

- Tạo App Name cho project android bằng cách nhấp vào icon Android trên trang dashboard của Firebase

{{< figure src="./images/d7893857-82c0-4f58-b649-f59925f50119.png" >}}

- Nhập **Android ackage name** và nhấp vào "**Register app**"

{{< figure src="./images/dd9fc80f-997b-428e-bca1-7d6b4d90be15.png" >}}

- Tải file **google-services.json** về và nhấp vào "**Next**"

{{< figure src="./images/467af0ce-2b39-480f-bde4-ad829e2dd5f2.png" >}}

- Copy file vừa tải về vào thư mục **android/app**

{{< figure src="./images/864cba07-2105-4305-b0b7-b7eb84fbbdc9.png" >}}

- Sau đó mở **Android studio** và chọn project của mình

{{< figure src="./images/37743f0e-fce6-4472-bce2-367a1c932459.png" >}}

- Để cho các giá trị cấu hình google-services.json có thể truy cập được vào SDK Firebase, bạn cần có plugin Gradle dịch vụ của Google.

{{< figure src="./images/b252478f-18a7-44e4-9b15-968035c2875c.png" >}}

- Sau đó, trong tệp module (app-level) build.gradle , hãy thêm cả plugin dịch vụ google và bất kỳ SDK Firebase nào bạn muốn sử dụng trong ứng dụng của mình:

{{< figure src="./images/0094549c-2839-4ef8-a12e-0a3cf89779b9.png" >}}

- Tiếp tục quay lại trang web và nhấp vào "**Next**"

{{< figure src="./images/5b950f29-49a3-4330-ae39-9f1b4dde3900.png" >}}

- Tiếp tục quay lại trang web và nhấp vào "**Continue console**"

{{< figure src="./images/01283676-e67e-4c84-a5e6-3dc15861c854.png" >}}

{{< figure src="./images/8e111288-058f-4d62-9252-cea150e6ff4a.png" >}}

- Như vậy mình đã tạo Firebase cho android thành công

## Cấu hình Fastlane cho Android

Đầu tiền vào thư mục **android** :

```
cd android
```

{{< figure src="./images/4a354dcb-3312-43e2-95aa-0aa39892b0d1.png" >}}

sau đó init fastlane bằng lệnh

```
fastlane init
```

{{< figure src="./images/fe829f2d-fcce-44a1-b7e4-00689bf4748f.png" >}}

Sau khi init thành công thì sẽ có thư mục **fastlane** và 2 file "**Appfile**" , "**Fastfile**"

Cài đặt firebase-tools dùng để lấy token login

```
 npm i firebase-tools -g
```

{{< figure src="./images/7c3251b4-74aa-416c-8d71-c4575a8c57b6.png" >}}

sau khi mình cài đặt thành công và sử dụng lệnh để lấy token login

```
firebase login:ci
```

{{< figure src="./images/e5e68563-3969-41a6-acb8-9e5e207ccdbb.png" >}}

như vậy mình đã có token login

```
1//0gP2yENNqCT1oCgYIARAAGBASNwF-L9IrqQI9gih5h73FSmaozzyn0HSQXxS7Vyyav2h4zEk4ClMbgmz6CwbEShz_qJVMmTxiGas
```

Tiếp tục mình sẽ lấy app-id

{{< figure src="./images/9b64793d-9975-456a-9253-49b5273b84db.png" >}}

```
1:320882145170:android:21837959d5ffc9fe65b34b
```

Tiếp tục ta cập nhật "**Appfile**" , "**Fastfile**"

- Appfile

```
json_key_file("") # Path to the json secret file - Follow https://docs.fastlane.tools/actions/supply/#setup to get one
package_name("com.reactnativedevops") # e.g. com.krausefx.app
```

- Fastfile

```
default_platform(:android)
  desc "Submit a new Beta Build to Crashlytics Beta"
  lane :beta do

    gradle(
      task: "clean"
    )

    gradle(
      task: 'assemble', #assemble #bundle
      build_type: 'Release'
    )

     firebase_app_distribution(
       app: "1:320882145170:android:21837959d5ffc9fe65b34b",
       firebase_cli_token: "1//0gP2yENNqCT1oCgYIARAAGBASNwF-L9IrqQI9gih5h73FSmaozzyn0HSQXxS7Vyyav2h4zEk4ClMbgmz6CwbEShz_qJVMmTxiGas",
     )
  end
end
```

Tiếp tục add plugin cho fastlane

```
fastlane add_plugin firebase_app_distribution
```

{{< figure src="./images/7defe04d-d771-4b9c-b499-f53409bf26fb.png" >}}

Kích hoạt Firebase Distribution trên Firebase console:

{{< figure src="./images/8f37b6b1-6284-458d-8eb6-b315eddedbc0.png" >}}

{{< figure src="./images/b9549722-c18f-4d2b-b9fd-dee877e1b4e2.png" >}}

Sau đó ta dùng lệnh để build

```
cd android && fastlane beta
```

{{< figure src="./images/b717010c-f474-4d84-ad26-c8047ddc971c.png" >}}

Khi build thành công ta vào trang Firebase console để kiểm tra xem bản build đã lên chưa

{{< figure src="./images/b416fa7d-314f-45d0-b440-b740fa73b680.png" >}}

Như vậy bản build đã build thành công, bây giờ mình chỉ cần thêm mail của tester vào để test bản build Android thôi

Các bạn có thể tham khảo qua [github](https://github.com/akitectio/ReactNativeDevOps)
