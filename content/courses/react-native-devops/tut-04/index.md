---
categories:
  - devops
  - react-native
date: 2023-02-04T08:00:00+08:00
description: Ở bài này mình sẽ hướng dẫn các bạn xơi trái táo cắn vỡ 🍎 🫢
draft: false
featuredImage: /series/react-native-devops/lesson-4-install-fastlane-for-ios-to-push-ipa-to-firebase-distribution-and-testflight.webp
images:
  - /bai-4-cai-dat-fastlane-cho-ios-day-ipa-len-firebase-distribution-va-testflight/images/index.png
  - /series/react-native-devops/lesson-4-install-fastlane-for-ios-to-push-ipa-to-firebase-distribution-and-testflight.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - rn-devops
tags:
  - react-native
  - jenkins-agent
title: Bài 4 - Cài đặt Fastlane cho IOS đẩy IPA lên Firebase Distribution và Testflight
url: /bai-4-cai-dat-fastlane-cho-ios-day-ipa-len-firebase-distribution-va-testflight
weight: 4
---

# Tóm tắt

## Các khái niệm

- **Tệp .IPA** là tệp lưu trữ ứng dụng iOS và iPados lưu trữ ứng dụng iOS/iPados. Mỗi tệp .IPA bao gồm một nhị phân và chỉ có thể được cài đặt trên thiết bị iOS, iPados hoặc MacOS dựa trên ARM. Các tệp có phần mở rộng .IPA có thể không bị nén bằng cách thay đổi tiện ích mở rộng thành .zip và giải nén. Hầu hết các tệp .ipa không thể được cài đặt trên trình giả lập iPhone vì chúng không chứa nhị phân cho kiến x86, ARM của điện thoại và máy tính bảng di động. Để chạy các ứng dụng trên trình giả lập, các tệp dự án gốc có thể được mở bằng cách sử dụng Xcode SDK. Tuy nhiên, một số tệp .ipa có thể được mở trên trình giả lập bằng cách trích xuất và sao chép qua tệp .APP được tìm thấy trong thư mục tải trọng. Một số ứng dụng đơn giản có thể chạy trên trình giả lập thông qua phương pháp này.
- **Tài khoản Apple Developer** là một trong các quy trình quan trọng nhất trong phát triển ứng dụng IOS, khi run, debug, xuất ra tiệp cà đặt .ipa hay upload app lên Testflight. có 2 loại tài khoản

1.  Apple Developer Program for Individuals (99$/ Năm) : Đành cho cá nhân.
2.  Membership Program for Organizations (299$/ Năm) : Dành cho tổ chức, công ty.

- **Apple Distribution Certificates** là giấy chứng nhận định danh tài khoản có quyền hạn để tải app lên appstore của sản phẩm hay không, ở tài khoản 99$ chỉ tạo được max là 3 certificates.
- **Apple Development Certificates** là giấy chứng nhận chỉ được sử dụng cho mục đích phát triển. Hổ trợ tất cả thiết bị, và chỉ tạo được tối da 12 certificates cho tài khoản $99.

## Hướng dẫn

1. Cấu hình Fastlane và đẩy IPA lên Firebase Distribution
2. Cấu hình Fastlane và đẩy Testflight

### Cấu hình Fastlane và đẩy IPA lên Firebase Distribution

#### Tạo App Name cho project android bằng cách nhấp vào icon Android trên trang dashboard của Firebase

{{< figure src="./images/09ee3dc3-6d2d-4463-a73e-876c44dbb431.png" >}}

{{< figure src="./images/f91c3223-74a8-41c3-b2da-806238fb2537.png" >}}

- Nhập Apple bundle ID và nhấp vào "Register app"

{{< figure src="./images/58389774-3587-40d6-ab91-fccd10c12da7.png" >}}

Tải file **GoogleService-Info.plist** về và nhấp vào "Next

Sau khi tải về chúng ta mở xcode bằng lệnh

```
open ios/ReactNativeDevOps.xcworkspace
```

{{< figure src="./images/deb54c5b-a90d-43e6-92d4-b91ffc16592c.png" >}}

Tiếp theo chúng ta kéo tiệp đã tải vào xcode

{{< figure src="./images/8e951631-2a3d-42bd-819f-391a279a74b3.png" >}}

Tiếp theo chọn theo hình và chọn **OK**

{{< figure src="./images/823ccdb4-14d0-4b46-a132-07d4b82143b2.png" >}}

Tiếp chục chọn **Next**

{{< figure src="./images/ed7468be-1bda-4814-8011-81eb5d94bfe0.png" >}}

Tiếp chục chọn **Next**

{{< figure src="./images/242b89f4-6729-4de6-8b74-a881bd0d9589.png" >}}

Tiếp tục chọn **" Continue to console "**

{{< figure src="./images/82fd2b24-f2be-4e52-8a57-af3dade3256b.png" >}}

Sau khi đăng ký thành công thì mình sẽ có phần ios trên trang dashboard

{{< figure src="./images/ba16a2fe-c936-4d02-b307-2f051938c930.png" >}}

Như vậy mình đã đăng ký thành công ios trên Firebase

#### Tiếp theo thì sẽ kích hoạt Firebase Distribution

Ta chọn Firebase Distribution ở menu bên phải, sau đó chon IOS

{{< figure src="./images/7b5e2cd7-d453-42d0-9da2-37b00243d911.png" >}}

Tiếp theo chọn **Get started**

{{< figure src="./images/257a17c9-32ce-4841-af99-6ff53dad2adc.png" >}}

Như vậy ta đã kích hoạt thành công Firebase Distribution

{{< figure src="./images/d57b9358-5acd-4238-bbd1-5e52d6d8628d.png" >}}

#### Build .IPA

Bạn cần một Account Apple Developer để thực hiện bước này

1. Tạo Certificates Distribution:

Ta truy cập vào đường dẫn để thực hiện tạo: https://developer.apple.com/account sau đó chọn **Certificates**

{{< figure src="./images/ffcddf8a-d551-4dc9-8400-44a9c4442d18.png" >}}

Ta mở Keychain Access lên và chọn **Certificate Assistant -> Request a Certificate From a Certificate Authority..**

{{< figure src="./images/77310c5d-5ab0-4a3c-845b-1b04ff0e62fb.png" >}}

Tiếp theo ta nhập email và chọn **Saved to disk**

{{< figure src="./images/e4400680-3fa9-4182-9493-7c5346e976a4.png" >}}

Chọn vào icon tạo mới

{{< figure src="./images/a970ae8d-78e8-407f-a44e-25ca254bdfc1.png" >}}

Tiếp theo chọn **Apple Distribution** và nhấp vào **Continue**

{{< figure src="./images/7e27bf85-ca6d-4adc-a187-bfbe7f9bc629.png" >}}

{{< figure src="./images/12d932f7-13f9-4092-b9e4-0a08863575cc.png" >}}

Chọn tiệp **CertificateSigningRequest.certSigningRequest** vừa tạo ở trên

{{< figure src="./images/64223d18-9636-4198-9338-8f5e94186efe.png" >}}

Nhấp vào tiếp tục và tải tiệp đã tạo về

{{< figure src="./images/abbb38db-0f23-4699-aad7-ea696d22c17c.png" >}}

Nhấp double click vào tiệp mới tải về và import vào keychain acceses và kiểm tra xem tiệp đã được import chưa

{{< figure src="./images/f520c374-9797-4556-bee1-ae8a1c2310d9.png" >}}

Như vậy tiệp đã được import rồi, tiếp tục ta tạo **Provisioning Profile Ad Hoc**

Ta chọn menu bên trái **Profiles**

{{< figure src="./images/70d6ad8e-ff7d-4011-a08d-6bb410ff6540.png" >}}

Tiếp tục ta chọn **Continue**

{{< figure src="./images/9a8bdce1-4009-4cba-8a9b-59c3ca99570f.png" >}}

Tiếp tục ta chọn **Continue**

{{< figure src="./images/22e64aaf-60c5-4094-8f16-c5a9d8021fb2.png" >}}

Ta chọn Certificate vừa tạo và nhấp vào **Continue**

{{< figure src="./images/813e60d8-eba1-497b-b7a1-93eb317fba3a.png" >}}

Chọn thiết bị được sử dụng, ở phần này mình build Ad Hoc nên cần add thêm thiết bị vào bản build

{{< figure src="./images/7d170dfa-1f84-491e-80f8-7569b7230615.png" >}}

Tiếp chọn đặt tên cho tiệp và chọn **Generate** sau đó tải tiệp đó về

{{< figure src="./images/5980a74e-43a0-4699-94e0-a49d335d6de3.png" >}}

Nhấp double click vào tiệp mới tải về và import vào keychain acceses

Tiếp theo ta dùng lệnh fastlane init để add bộ build của ios

```
fastlane init
```

{{< figure src="./images/d3b2205f-6bce-42aa-a931-fc32cac6beb0.png" >}}

Tiếp theo ta chọn số 4

{{< figure src="./images/838d40c7-68d6-4883-a660-588572e5fd04.png" >}}

Tiếp theo ta add thên plugin firebase_app_distribution

```
fastlane add_plugin firebase_app_distribution
```

{{< figure src="./images/7dbc58c6-9748-47ce-89d8-e6f141ab2257.png" >}}

Tiếp tục cập nhật file ios->fastlane->Fastfile

```
# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#
# For a list of all available plugins, check out
#
#     https://docs.fastlane.tools/plugins/available-plugins
#

# Uncomment the line if you want fastlane to automatically update itself
# update_fastlane

ENV["FASTLANE_XCODEBUILD_SETTINGS_TIMEOUT"] = "180"
ENV["FASTLANE_XCODE_LIST_TIMEOUT"] = "180"

default_platform(:ios)

platform :ios do
  desc "Push a new beta build to firebase"
  lane :firebase do |options|

    clear_derived_data
    clean_build_artifacts

    build_app(
      export_options: {
        method: "ad-hoc",
        provisioningProfiles: {
          "com.reactnativedevops" => "Provisioning Profile react native depops"
        }
      }
    )

    puts "+---------------------------------+".bold.blue
    puts "|-- FIREBASE_CLI_TOKEN: #{ENV['FIREBASE_CLI_TOKEN']} 🚀 --|".bold.blue
    puts "|-- FIREBASE_APP_ID: #{ENV['FIREBASE_APP_ID']} 🚀 --|".bold.blue
    puts "+---------------------------------+".bold.blue


     firebase_app_distribution(
       app: "1:320882145170:ios:56b45da6b925693e65b34b",
       firebase_cli_token: "1//0gP2yENNqCT1oCgYIARAAGBASNwF-L9IrqQI9gih5h73FSmaozzyn0HSQXxS7Vyyav2h4zEk4ClMbgmz6CwbEShz_qJVMmTxiGas"
     )
    end
end
```

firebase_app_distribution phần app vs firebase_cli_token các bạn xem ở bài trước nhé, mình có hướng dẫn ở bài trước rồi, tiếp tục ta dùng lệnh **fastlane firebase** để chạy lệnh

```
fastlane firebase
```

{{< figure src="./images/bbf7780a-c0d3-4e28-a8c9-6be3c60b3544.png" >}}

Như vậy bạn đã build thành công và đẩy lên firebase, tiếp tục mình login vào firebase console để xem tiệp đã được đẩy lên chưa

{{< figure src="./images/756e709a-bd22-4f5e-a496-cfa1bc29db01.png" >}}

Như vậy mình đã build thành công, và upload bản build version 1 lên Firebase Distribution

### Cấu hình Fastlane và đẩy Testflight

#### Bước 1: Tạo identifiers

{{< figure src="./images/f351579a-c252-4aba-a7d5-dda07aeeec1c.png" >}}

Tiếp tục chọn **Continue**

{{< figure src="./images/9cab9629-9323-4f89-ae94-ebeff9d8e24a.png" >}}

Chọn App và nhấp vào **Continue**

{{< figure src="./images/0f471687-6cff-4220-b939-5c8443f0bbb1.png" >}}

tiếp tục nhập **Description** và **Bundle ID **

{{< figure src="./images/9ed769fa-583e-4cc4-9328-f360d69365a5.png" >}}

Như vậy ta đã tạo thành công identifiers cho app có packages name là com.reactnativedevops

{{< figure src="./images/db945c18-f958-4e9c-82fc-2dc32073ddea.png" >}}

Tiếp tục ta vào phần trang chủ appconnect tạo New App

{{< figure src="./images/c53e5954-b425-4996-bac9-1fdbf0189cd9.png" >}}

Như vậy ta vùa thành công tạo **New App** có app name là **React Native Devops**

{{< figure src="./images/62054b25-8755-47b1-8f44-55de2c07233f.png" >}}

Tiếp tục cập nhật file ios->fastlane->Fastfile thêm đoạn config build mới vào

```
 desc "Push a new beta build to TestFlight"
    lane :beta do |options|

      clear_derived_data
      clean_build_artifacts

      build_app(
        scheme: "Release",
        export_options: {
          method: "app-store",
          provisioningProfiles: {
            "com.reactnativedevops" => "Provisioningdevopsstore"
          }
        }
      )

      upload_to_testflight(
        notify_external_testers:true,
        skip_waiting_for_build_processing:true

      )

    end
```

Tiếp tục cập nhật file ios->fastlane->Appfile thêm đoạn config build mới vào

```
# app_identifier("[[APP_IDENTIFIER]]") # The bundle identifier of your app
# apple_id("[[APPLE_ID]]") # Your Apple Developer Portal username
app_identifier("com.reactnativedevops") # The bundle identifier of your app

apple_id("tdduy.dev@gmail.com") # Your Apple email address

itc_team_id("") # App Store Connect Team ID
team_id("") # Developer Portal Team ID


# For more information about the Appfile, see:
#     https://docs.fastlane.tools/advanced/#appfile
```

sau khi update ta dùng lệnh **fastlane beta -verbose** để build

```
fastlane beta -verbose
```

{{< figure src="./images/06c1ab78-c282-4557-9994-8c075e0159b2.png" >}}

Sau khi build thành công thì app của mình đã lên được **testflight**

{{< figure src="./images/97f084fe-9339-4ec2-8c68-6dc4e1a1ee52.png" >}}

Như vậy mình đã chia sẽ có các bạn cách build cho ios thông qua fastlane, cảm ơn các bạn đã xem qua bài viết của mình.
