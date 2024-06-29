---
categories: [devops, react-native]
date: 2023-02-03T00:00:00.000Z
description: To facilitate the tester team to test the product and more automated, we need to do CI - CD
draft: false
featuredImage: /series/react-native-devops/lesson-3-register-firebase-and-configure-fastlane-to-push-apk-to-firebase-distribution-test-version.webp
images: [/series/react-native-devops/lesson-3-register-firebase-and-configure-fastlane-to-push-apk-to-firebase-distribution-test-version.webp, /lesson-3-register-firebase-and-configure-fastlane-to-push-apk-to-firebase-distribution-test-version/images/index.en.png]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [rn-devops]
tags: [react-native, jenkins-agent]
title: Lesson 3 - Register Firebase and configure Fastlane to push APK to Firebase Distribution (Test version)
weight: 3
---

# Summary

## The concept

-   Firebase is a Google-powered application development software that allows developers to develop iOS, Android, and web applications. Firebase provides tools to track analytics, report and fix app issues, and create marketing and product tests.

-   APK stands for **Android Package** (sometimes **Android Package Kit** or **Android Application Package** ). It's the file format Android uses to distribute and install apps. Therefore, APK contains all the elements that an app needs to install correctly on your device.

-   IPA is an extension to the iOS App Store package file which is an application archive file used to distribute applications on iOS. IPA contains files in uncompressed form, which can only be installed on iOS.

# Instruct

## Sign up for Firebase

-   We go to Firebase's homepage to register: <https://console.firebase.google.com/u/0/>

{{< figure src="./images/3da88c64-df98-4a25-89a9-3eff4b318e04.png" >}}

After successfully logging in, select "**Add Project**"

{{< figure src="./images/2228fac0-e13e-451d-b22a-ce8f10b0b88a.png" >}}

-   Enter the project name, here I will name it "**React Native DevOps**"

{{< figure src="./images/6a242cbc-ef9e-4bda-bab7-12d3e4b6e7b1.png" >}}

-   Continue clicking **Continue**

{{< figure src="./images/2c5ad938-c378-4836-b269-11c809ca075a.png" >}}

-   Continue clicking **Continue**

{{< figure src="./images/6fb20016-0e6e-457d-b13c-761cbd6b5eb6.png" >}}

-   In this step, we choose a GA account or create a new account for GA, then click "Create project"

{{< figure src="./images/daf6d2e6-4037-48e5-86e9-9278d0d84ca5.png" >}}

-   Continue clicking **Continue**, So you have successfully created the project on Firebase

{{< figure src="./images/1ee79b99-1865-4450-bf1b-dcafc542bd5e.png" >}}

## Configure Firebase for Android

-   Create an App Name for the Android project by clicking the Android icon on the Firebase dashboard page

{{< figure src="./images/d7893857-82c0-4f58-b649-f59925f50119.png" >}}

-   Enter **Android package name** and click "**Register app**"

{{< figure src="./images/dd9fc80f-997b-428e-bca1-7d6b4d90be15.png" >}}

-   Download the file **google-services.json** and click "**Next**"

{{< figure src="./images/467af0ce-2b39-480f-bde4-ad829e2dd5f2.png" >}}

-   Copy the downloaded file to the **android/app** folder

{{< figure src="./images/864cba07-2105-4305-b0b7-b7eb84fbbdc9.png" >}}

-   Then open **Android studio** and select your project

{{< figure src="./images/37743f0e-fce6-4472-bce2-367a1c932459.png" >}}

-   To make google-services.json configuration values accessible to the Firebase SDK, you need the Google Services Gradle plugin.

{{< figure src="./images/b252478f-18a7-44e4-9b15-968035c2875c.png" >}}

-   Then, in the module (app-level) build.gradle file, add both the google services plugin and any Firebase SDKs you want to use in your app:

{{< figure src="./images/0094549c-2839-4ef8-a12e-0a3cf89779b9.png" >}}

-   Keep coming back to the website and click "**Next**"

{{< figure src="./images/5b950f29-49a3-4330-ae39-9f1b4dde3900.png" >}}

-   Continue back to the website and click "**Continue console**"

{{< figure src="./images/01283676-e67e-4c84-a5e6-3dc15861c854.png" >}}

{{< figure src="./images/8e111288-058f-4d62-9252-cea150e6ff4a.png" >}}

-   So I have successfully created Firebase for Android

## Fastlane configuration for Android

First go to the **android** folder:

```bash
cd android
```

{{< figure src="./images/4a354dcb-3312-43e2-95aa-0aa39892b0d1.png" >}}

then init fastlane with command

```bash
fastlane init
```

{{< figure src="./images/fe829f2d-fcce-44a1-b7e4-00689bf4748f.png" >}}

After successful init, there will be a **fastlane** folder and 2 files "**Appfile**" , "**Fastfile**"

Install firebase-tools to get login token

```bash
  npm i firebase-tools -g
```

{{< figure src="./images/7c3251b4-74aa-416c-8d71-c4575a8c57b6.png" >}}

After I successfully installed and used the command to get the login token

```bash
firebase login:ci
```

{{< figure src="./images/e5e68563-3969-41a6-acb8-9e5e207ccdbb.png" >}}

So I have a login token

```bash
1//0gP2yENNqCT1oCgYIARAAGBASNwF-L9IrqQI9gih5h73FSmaozzyn0HSQXxS7Vyyav2h4zEk4ClMbgmz6CwbEShz_qJVMmTxiGas
```

Continuing, I will get the app-id

{{< figure src="./images/9b64793d-9975-456a-9253-49b5273b84db.png" >}}

```bash
1:320882145170:android:21837959d5ffc9fe65b34b
```

Continue to update "**Appfile**", "**Fastfile**"

-   Appfile

```bash
json_key_file("") # Path to the json secret file - Follow https://docs.fastlane.tools/actions/supply/#setup to get one
package_name("com.reactnativedevops") # e.g. com.krausefx.app
```

-   Fastfile

```bash
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

Continue adding plugins for fastlane

```bash
fastlane add_plugin firebase_app_distribution
```

{{< figure src="./images/7defe04d-d771-4b9c-b499-f53409bf26fb.png" >}}

Enable Firebase Distribution on the Firebase console:

{{< figure src="./images/8f37b6b1-6284-458d-8eb6-b315eddedbc0.png" >}}

{{< figure src="./images/b9549722-c18f-4d2b-b9fd-dee877e1b4e2.png" >}}

Then we use the command to build

```bash
cd android && fastlane beta
```

{{< figure src="./images/b717010c-f474-4d84-ad26-c8047ddc971c.png" >}}

When the build is successful, go to the Firebase console page to check if the build is up or not

{{< figure src="./images/b416fa7d-314f-45d0-b440-b740fa73b680.png" >}}

So the build has been built successfully, now I just need to add the tester's email to test the Android build

You can refer to [github](https://github.com/akitectio/ReactNativeDevOps)
