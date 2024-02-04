---
categories:
  - devops
  - react-native
date: 2023-02-02T08:00:00+08:00
description.en: Minimize errors when pushing to production environment
draft: false
featuredImage: /series/react-native-devops/lesson-2-building-a-development-staging-production-environment.webp
images:
  - /series/react-native-devops/lesson-2-building-a-development-staging-production-environment.webp
  - /lesson-2-building-a-development-staging-production-environment/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - rn-devops
tags:
  - react-native
  - Jenkins Agent
  - Build React Native app
  - build reactjs
  - build react native
title: Lesson 2 - Building a DEVELOPMENT - STAGING - PRODUCTION environment
url: /lesson-2-building-a-development-staging-production-environment
weight: 2
---

# Summary

## Environment files concept

- Keys, secrets, and configuration are often stored in **.env** files, with different parameters corresponding to specific build types. You may be familiar with .env files as part of the [12-factor app](https://12factor.net/) approach. The 12-factor principles are intended for software applications as a service - this method is not a perfect fit for RN development. However, approaching build configuration in a 12-factor model is a good idea.

- To use .env configuration files in React Native (RN) applications, we need to add [react-native-config](https://www.npmjs.com/package/react-native-config) to your project. Here, I will create 3 environments **DEVELOPMENT - STAGING - PRODUCTION**

## Instructions

## Create a base project

To create a project, use the command:

```
npx react-native init ReactNativeDevOps
```

{{< figure src="./images/0ae1addc-06f4-469c-9c78-72beb0e6fc00.png" >}}

## Install and configure react-native-config

Install **react-native-config**

```
npm i react-native-config --save
```

{{< figure src="./images/52905fe3-1fb1-45c8-857e-11972f7f2c70.png" >}}

### Configure IOS

1. Run the command

```
npx pod-install
```

{{< figure src="./images/5d25c95e-2c45-41a4-bca1-9bf63faac813.png" >}}

Open Xcode with the command

```
open ios/ReactNativeDevOps.xcworkspace
```

then open the **AppDelegate.mm** file

{{< figure src="./images/b9c5e25a-4b2f-4c1a-84f3-ccbb0a75dc16.png" >}}

### Configure Android

Go to the **android/app/build.gradle** file and add the following code to line 2:

```
apply from: project(':react-native-config').projectDir.getPath() + "/dotenv.gradle"
```

{{< figure src="./images/ed317a4f-8085-40ec-955e-602a0926f3c0.png" >}}

In the **android/app/proguard-rules.pro** file, add the line:

```
-keep class com.reactnativedevops.BuildConfig { *; }
```

{{< figure src="./images/c804a7d5-0c7a-4566-b8a5-bc4ba305d0b9.png" >}}

Next, create 3 files in the root directory:

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

Then, use the config by adding to scripts in package.json

```
{
  "name": "ReactNativeDevOps",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "dev-android": "ENVFILE=.env react-native run-android", # <- Add here
    "stg-android": "ENVFILE=.env.stg react-native run-android", # <- Add here
    "prod-android": "ENVFILE=.env.prod react-native run-android",  # <- Add here
    "stg-ios": "ENVFILE=.env.stg react-native run-ios", # <- Add here
    "dev-ios": "ENVFILE=.env react-native run-ios", # <- Add here
    "prod-ios": "ENVFILE=.env.prod react-native run-ios", # <- Add here
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

After configuring, use the command to run the test config

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

Thus, I have successfully configured 3 environments dev-stg-prod.
