---
categories:
  - devops
  - react-native
date: 2023-02-02T08:00:00+08:00
description: Giảm thiểu lỗi khi đẩy lên môi trường production
draft: false
featuredImage: /series/react-native-devops/lesson-2-building-a-development-staging-production-environment.webp
images:
  - /series/react-native-devops/lesson-2-building-a-development-staging-production-environment.webp
  - /bai-2-xay-dung-moi-truong-development-staging-production/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - rn-devops
tags:
  - react-native
  - Jenkins Agent
  - Build React Native app
  - build reactjs
  - build react native
title: Bài 2 - Xây dựng môi trường DEVELOPMENT - STAGING - PRODUCTION
url: /bai-2-xay-dung-moi-truong-development-staging-production
weight: 2
---

# Tóm tắt

## Khái niệm Environment files

- Các Key,secrets và config thường được lưu trữ trong các tệp **.env** , với các tham số khác nhau tương ứng với các loại xây dựng cụ thể. Bạn có thể quen thuộc với các tệp .env tạo thành phương pháp [ứng dụng 12 yếu tố](https://12factor.net/). Các nguyên tắc 12 yếu tố được dành cho các ứng dụng phần mềm như một dịch vụ-phương pháp này không áp dụng hoàn hảo cho sự phát triển RN. Tuy nhiên, việc tiếp cận cấu hình xây dựng theo mô hình 12 yếu tố là một ý tưởng tốt.

- Để sử dụng các tiệp cấu hình .env trong các ứng dụng React Native (RN), ta phải thêm [react-native-config](https://www.npmjs.com/package/react-native-config) vào project của bạn. ở đây mình sẽ tạo ra 3 môi trường **DEVELOPMENT - STAGING - PRODUCTION**

## Hướng dẫn

## Tạo project base

Để tạo Project ta dùng lệnh:

```
npx react-native init ReactNativeDevOps
```

{{< figure src="./images/0ae1addc-06f4-469c-9c78-72beb0e6fc00.png" >}}

## Cài đặt và cấu hình react-native-config

Cài đặt **react-native-config**

```
npm i react-native-config --save
```

{{< figure src="./images/52905fe3-1fb1-45c8-857e-11972f7f2c70.png" >}}

### Cấu hình IOS

1. Chạy lện

```
npx pod-install
```

{{< figure src="./images/5d25c95e-2c45-41a4-bca1-9bf63faac813.png" >}}

Ta mở xcode bằng lệnh

```
open ios/ReactNativeDevOps.xcworkspace
```

sau đó mở file **AppDelegate.mm**

{{< figure src="./images/b9c5e25a-4b2f-4c1a-84f3-ccbb0a75dc16.png" >}}

### Cấu hình Android

Ta vào file **android/app/build.gradle** thêm vào dòng số 2 đoạn code

```
apply from: project(':react-native-config').projectDir.getPath() + "/dotenv.gradle"
```

{{< figure src="./images/ed317a4f-8085-40ec-955e-602a0926f3c0.png" >}}

trong file **android/app/proguard-rules.pro** ta thêm dòng

```
-keep class com.reactnativedevops.BuildConfig { *; }
```

{{< figure src="./images/c804a7d5-0c7a-4566-b8a5-bc4ba305d0b9.png" >}}

Tiếp theo ta tạo thêm 3 file ở thư mục root:

1. DEVELOPMENT (.env)

```
API_KEY=devKey
API_URI=https://dev.com/api
```

2. STAGING (.env.stg)

```
API_KEY=stagingKey
API_URI=https://staging.com/api
```

3. PRODUCTION (.env.prod)

```
API_KEY=productKey
API_URI=https://prod.com/api
```

{{< figure src="./images/e9bec63a-b759-4801-9518-d6a87d040a49.png" >}}

Sau đó ta sử dụng config bằng cách thêm vào scripts trong package.json

```
{
  "name": "ReactNativeDevOps",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "dev-android": "ENVFILE=.env react-native run-android", # <- Thêm ở đây
    "stg-android": "ENVFILE=.env.stg react-native run-android", # <- Thêm ở đây
    "prod-android": "ENVFILE=.env.prod react-native run-android",  # <- Thêm ở đây
    "stg-ios": "ENVFILE=.env.stg react-native run-ios", # <- Thêm ở đây
    "dev-ios": "ENVFILE=.env react-native run-ios", # <- Thêm ở đây
    "prod-ios": "ENVFILE=.env.prod react-native run-ios", # <- Thêm ở đây
    "lint": "eslint .",
    "start": "react-native start",
    "test": "jest"
  },
  "dependencies": {
    "react": "18.2.0",
    "react-native": "0.71.2",
    "react-native-config": "^1.5.0"
  },
  "devDependencies": {
    "@babel/core": "^7.20.0",
    "@babel/preset-env": "^7.20.0",
    "@babel/runtime": "^7.20.0",
    "@react-native-community/eslint-config": "^3.2.0",
    "@tsconfig/react-native": "^2.0.2",
    "@types/jest": "^29.2.1",
    "@types/react": "^18.0.24",
    "@types/react-test-renderer": "^18.0.0",
    "babel-jest": "^29.2.1",
    "eslint": "^8.19.0",
    "jest": "^29.2.1",
    "metro-react-native-babel-preset": "0.73.7",
    "prettier": "^2.4.1",
    "react-test-renderer": "18.2.0",
    "typescript": "4.8.4"
  },
  "jest": {
    "preset": "react-native"
  }
}
```

Sau khi cấu hình xong ta dùng lệnh để run test config

1. dev

```
npm run dev-ios
```

{{< figure src="./images/9ffbfd3c-a5d4-4f1f-bfa6-8f8f2326f874.png" >}}

2. stg

```
npm run stg-ios
```

{{< figure src="./images/9e40289e-e501-45b9-9d5c-1233ab93fd1e.png" >}}

3. prod

```
npm run prod-ios
```

{{< figure src="./images/55126fba-be2e-448b-a2c7-b85624a3ce4f.png" >}}

Như vậy thì mình đã cấu hình thành công 3 môi trường dev-stg-prod
