---
categories:
  - devops
  - react-native
date: 2023-02-04T08:00:00+08:00
description: In this article, I will guide you to eat the broken apple 🍎 🫢
draft: false
featuredImage: /series/react-native-devops/lesson-4-install-fastlane-for-ios-to-push-ipa-to-firebase-distribution-and-testflight.webp
images:
  - /lesson-4-install-fastlane-for-ios-to-push-ipa-to-firebase-distribution-and-testflight/images/index.en.png
  - /series/react-native-devops/lesson-4-install-fastlane-for-ios-to-push-ipa-to-firebase-distribution-and-testflight.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - rn-devops
tags:
  - react-native
  - jenkins-agent
title: Lesson 4 - Install Fastlane for IOS to push IPA to Firebase Distribution and Testflight
url: /lesson-4-install-fastlane-for-ios-to-push-ipa-to-firebase-distribution-and-testflight
weight: 4
---

# Summary

## The concept

- **.IPA files** are iOS and iPados app archive files that store iOS/iPados apps. Each .IPA file consists of a binary and can only be installed on ARM-based iOS, iPados, or MacOS devices. Files with the .IPA extension can be uncompressed by changing the extension to .zip and unzipping. Most .ipa files cannot be installed on iPhone emulators because they do not contain binaries for x86, ARM ant of mobile phones and tablets. To run applications on the emulator, native project files can be opened using the Xcode SDK. However, some .ipa files can be opened on the emulator by extracting and copying over the .APP file found in the payload folder. Some simple applications can run on the emulator through this method.
- **Apple Developer account** is one of the most important processes in IOS application development, when running, debugging, exporting .ipa installation files or uploading apps to Testflight. There are 2 types of accounts

1. Apple Developer Program for Individuals ($99/Year): For individuals.
2. Membership Program for Organizations ($299/Year): For organizations and companies.

- **Apple Distribution Certificates** is a certificate that identifies whether the account has the authority to upload the app to the product's appstore. For a $99 account, a maximum of 3 certificates can only be created.
- **Apple Development Certificates** are certificates used for development purposes only. Supports all devices, and can only create up to 12 certificates for a $99 account.

## Instruct

1. Configure Fastlane and push the IPA to Firebase Distribution
2. Configure Fastlane and push Testflight

### Configure Fastlane and push IPA to Firebase Distribution

#### Create an App Name for your Android project by clicking the Android icon on the Firebase dashboard

{{< figure src="./images/09ee3dc3-6d2d-4463-a73e-876c44dbb431.png" >}}

{{< figure src="./images/f91c3223-74a8-41c3-b2da-806238fb2537.png" >}}

- Enter Apple bundle ID and click "Register app"

{{< figure src="./images/deb54c5b-a90d-43e6-92d4-b91ffc16592c.png" >}}

Download the file **GoogleService-Info.plist** and click "Next

After downloading, we open xcode with the command

```bash
open ios/ReactNativeDevOps.xcworkspace
```

{{< figure src="./images/deb54c5b-a90d-43e6-92d4-b91ffc16592c.png" >}}

Next we drag the downloaded file into xcode

{{< figure src="./images/8e951631-2a3d-42bd-819f-391a279a74b3.png" >}}

Next, select the image and select **OK**

{{< figure src="./images/823ccdb4-14d0-4b46-a132-07d4b82143b2.png" >}}

Next, select **Next**

{{< figure src="./images/ed7468be-1bda-4814-8011-81eb5d94bfe0.png" >}}

Next, select **Next**

{{< figure src="./images/242b89f4-6729-4de6-8b74-a881bd0d9589.png" >}}

Continue to select **" Continue to console "**

{{< figure src="./images/82fd2b24-f2be-4e52-8a57-af3dade3256b.png" >}}

After successful registration, you will have the ios section on the dashboard page

{{< figure src="./images/ba16a2fe-c936-4d02-b307-2f051938c930.png" >}}

So I have successfully registered iOS on Firebase

#### Next, we will enable Firebase Distribution

We select Firebase Distribution in the right menu, then select IOS

{{< figure src="./images/7b5e2cd7-d453-42d0-9da2-37b00243d911.png" >}}

Next select **Get started**
{{< figure src="./images/257a17c9-32ce-4841-af99-6ff53dad2adc.png" >}}

So we have successfully enabled Firebase Distribution

{{< figure src="./images/d57b9358-5acd-4238-bbd1-5e52d6d8628d.png" >}}

#### Build .IPA

You need an Apple Developer Account to perform this step

1. Create Certificates Distribution:

We access the link to create: https://developer.apple.com/account then select **Certificates**

{{< figure src="./images/ffcddf8a-d551-4dc9-8400-44a9c4442d18.png" >}}

We open Keychain Access and select **Certificate Assistant -> Request a Certificate From a Certificate Authority..**

{{< figure src="./images/77310c5d-5ab0-4a3c-845b-1b04ff0e62fb.png" >}}

Next, enter your email and select **Saved to disk**

{{< figure src="./images/e4400680-3fa9-4182-9493-7c5346e976a4.png" >}}

Select the newly created icon

{{< figure src="./images/a970ae8d-78e8-407f-a44e-25ca254bdfc1.png" >}}

Next select **Apple Distribution** and click **Continue**

{{< figure src="./images/7e27bf85-ca6d-4adc-a187-bfbe7f9bc629.png" >}}

{{< figure src="./images/12d932f7-13f9-4092-b9e4-0a08863575cc.png" >}}

Select the file **CertificateSigningRequest.certSigningRequest** just created above

{{< figure src="./images/64223d18-9636-4198-9338-8f5e94186efe.png" >}}

Click continue and download the created file

{{< figure src="./images/abbb38db-0f23-4699-aad7-ea696d22c17c.png" >}}

Double click on the newly downloaded file and import it into keychain accounts and check if the file has been imported

{{< figure src="./images/f520c374-9797-4556-bee1-ae8a1c2310d9.png" >}}

So the file has been imported, let's continue to create **Provisioning Profile Ad Hoc**

We select the menu on the left **Profiles**

{{< figure src="./images/70d6ad8e-ff7d-4011-a08d-6bb410ff6540.png" >}}

Continue to select **Continue**

{{< figure src="./images/9a8bdce1-4009-4cba-8a9b-59c3ca99570f.png" >}}

Continue to select **Continue**

{{< figure src="./images/22e64aaf-60c5-4094-8f16-c5a9d8021fb2.png" >}}

We select the newly created Certificate and click **Continue**

{{< figure src="./images/813e60d8-eba1-497b-b7a1-93eb317fba3a.png" >}}

Select the device to be used, in this part we are building Ad Hoc so we need to add additional devices to the build

{{< figure src="./images/7d170dfa-1f84-491e-80f8-7569b7230615.png" >}}

Next, choose a name for the file and select **Generate** then download the file

{{< figure src="./images/5980a74e-43a0-4699-94e0-a49d335d6de3.png" >}}

Double click on the newly downloaded file and import it into keychain accounts

Next we use the fastlane init command to add the iOS build kit

```bash
fastlane init
```

{{< figure src="./images/d3b2205f-6bce-42aa-a931-fc32cac6beb0.png" >}}

Next we choose number 4

{{< figure src="./images/838d40c7-68d6-4883-a660-588572e5fd04.png" >}}

Next we add the firebase_app_distribution plugin

```
fastlane add_plugin firebase_app_distribution
```

{{< figure src="./images/7dbc58c6-9748-47ce-89d8-e6f141ab2257.png" >}}

Continue updating the file ios->fastlane->Fastfile

```bash
# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
# https://docs.fastlane.tools/actions
#
# For a list of all available plugins, check out
#
# https://docs.fastlane.tools/plugins/available-plugins
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

firebase_app_distribution part app vs firebase_cli_token, you can see in the previous post, I have instructions in the previous post, continue using the command **fastlane firebase**

```bash
fastlane firebase
```

{{< figure src="./images/bbf7780a-c0d3-4e28-a8c9-6be3c60b3544.png" >}}

So you have successfully built and uploaded to firebase. Let's continue to log in to the firebase console to see if the file has been uploaded.

{{< figure src="./images/756e709a-bd22-4f5e-a496-cfa1bc29db01.png" >}}

So I have successfully built, and uploaded the build version 1 to Firebase Distribution

### Configure Fastlane and push Testflight

#### Step 1: Create identifiers

{{< figure src="./images/f351579a-c252-4aba-a7d5-dda07aeeec1c.png" >}}

Continue selecting **Continue**

{{< figure src="./images/9cab9629-9323-4f89-ae94-ebeff9d8e24a.png" >}}

Select App and click **Continue**

{{< figure src="./images/0f471687-6cff-4220-b939-5c8443f0bbb1.png" >}}

Continue entering **Description** and **Bundle ID **

{{< figure src="./images/9ed769fa-583e-4cc4-9328-f360d69365a5.png" >}}

So we have successfully created identifiers for the app with the package name com.reactnativedevops

{{< figure src="./images/db945c18-f958-4e9c-82fc-2dc32073ddea.png" >}}

Continue to go to the appconnect home page to create a New App

{{< figure src="./images/c53e5954-b425-4996-bac9-1fdbf0189cd9.png" >}}

So we have successfully created **New App** with app name **React Native Devops**

{{< figure src="./images/62054b25-8755-47b1-8f44-55de2c07233f.png" >}}

Continue updating the ios->fastlane->Fastfile file to add the new build config section

```bash
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

Continue updating the ios->fastlane->Appfile file to add the new build config section

```bash
# app_identifier("[[APP_IDENTIFIER]]") # The bundle identifier of your app
# apple_id("[[APPLE_ID]]") # Your Apple Developer Portal username
app_identifier("com.reactnativedevops") # The bundle identifier of your app

apple_id("tdduy.dev@gmail.com") # Your Apple email address

itc_team_id("") # App Store Connect Team ID
team_id("") # Developer Portal Team ID


# For more information about the Appfile, see:
# https://docs.fastlane.tools/advanced/#appfile
```

After updating, we use the command **fastlane beta -verbose** to build

```bash
fastlane beta -verbose
```

![Screenshot 2023-03-01 at 12.57.15.png](images/react-native-devops/06c1ab78-c282-4557-9994-8c075e0159b2.png)

After the build was successful, my app was on **testflight**

![Screenshot 2023-03-01 at 12.58.58.png](images/react-native-devops/97f084fe-9339-4ec2-8c68-6dc4e1a1ee52.png)

So I have shared with you how to build for iOS through fastlane. Thank you for reading my article.
