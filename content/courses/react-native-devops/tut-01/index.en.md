---
categories:
  - devops
  - react-native
date: 2023-02-01T08:00:00+08:00
description: Instructions for setting up jenkins agent to start building mobile with jenkins so developers can feel secure in developing products
draft: false
featuredImage: /series/react-native-devops/lesson-1-react-native-devops-basic-concepts-and-settings.webp
images:
  - /series/react-native-devops/lesson-1-react-native-devops-basic-concepts-and-settings.webp
  - /lesson-1-react-native-devops-basic-concepts-and-settings/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - rn-devops
tags:
  - react-native-devops
  - react-native
  - jenkins-agent
title: Lesson 1 - React Native DevOps basic concepts and settings
url: /lesson-1-react-native-devops-basic-concepts-and-settings
weight: 1
---

# Summary

## Concepts

- Hardware requirements
- Source code control
- iOS provisioning profiles
- Build environment

## Instructions

- Initial computer setup
- Installing applications
- Adding a new node to Jenkins master
- Running test job

# System Overview

## Concepts

{{< figure src="/images/c13c1882-325a-4228-9c5b-948f24cf1766.png" >}}

## Hardware requirements

- Here I am using hardware Mac Mini M1 2020 (3.2Ghz Apple M1 Chip With 8-CPU, 16G RAM, SSD 512), but if you want to use it on the cloud, you can refer to the server here [macincloud.com](https://macincloud.com)
- My Mac Mini will be kept at the company and can only be built on the company's internal network via the machine's IP address, while you can bring the network out by NATing the IP to the network or using a service on Mac Cloud.

## Source code control

In this guide, I will use Github to manage source code, Jenkins and GitHub work together to bring consistency and speed to the development team. Jenkins runs all types of automated DevOps workflows, such as hourly build verification, nightly builds, on-demand builds, and many more.

## iOS provisioning profiles

- My favorite part of iOS development is Provisioning profiles. I like the Android approach because of its simplicity, especially when managing different types of builds. Apple's approach to building destinations is limiting and requires additional work from developers to test all cases.
- To use Provisioning profiles, we use [Match](https://docs.fastlane.tools/actions/match/), which stores and updates iOS provisioning profiles and signing certificates. All files created by Match are then stored in an encrypted Github repository, with the decryption key shared by the development team.

## Build environment

- Releasing a React Native (RN) project for production requires a lot of tools. At a minimum, a successful release will use Node.js, NPM/Yarn, Xcode, and Android Studio. Projects that incorporate third-party SDKs add complexity and additional tools to releases. Managing this complexity is critical to creating a build process.
- It is very important for all developers working on the project to have the same environment. Automatically incrementing the version in the environment for RN is not as simple as other platforms - there are many tools and many places to make small mistakes. The small differences between the development machine and the build machine can lead to wasted developer time and the potential to sit and debug build errors.
- Setting up and installing Jenkins requires standardizing the build environment for yourself and the development team. Develop in the same environment that runs your builds.

# Instructions

## Initial computer setup

### 1. Update to the latest operating system

To ensure that the build runs smoothly and the environment is synchronized with the developer, we need to update the system by updating the operating system:

1. Select the Apple icon {{< figure src="/images/ab0708e4-0038-42ac-9c7b-aa4b7d2a3755.png" >}}
2. Select 'Software Update'
3. Update macOS

### 2. Install Xcode

1. Open the Appstore application
2. Search for Xcode
   {{< figure src="/images/f8faa7ab-26ce-450d-830e-09617aec19e3.png" >}}
3. Click on download and install
   {{< figure src="/images/7875a55d-538a-4f2b-8494-1e53c579811c.png" >}}

4. Open Xcode and complete the initial setup.

   {{< figure src="/images/c8e3de99-87df-438e-af14-da5c793a88d5.png" >}}

Click on **_Agree_**

{{< figure src="/images/fc7199ac-63bc-45b6-8555-dd6cae7479a9.png" >}}

Click on **_Install_**

{{< figure src="/images/33aec20d-970a-4592-9798-b74032292751.png" >}}

Click on **_Continue_**

{{< figure src="/images/a1ebdd26-93b5-4f20-97a1-e74d5ab6c6d0.png" >}}

Finish the setup

{{< figure src="/images/2bd14f42-4bff-4c55-9c50-edaeba4338ec.png" >}}

### 3. Install xcode cli

Open the terminal and type the command:

```
xcode-select --install
```

{{< figure src="/images/7736cf30-961f-47e8-8791-1f3ce0a559a6.png" >}}

Click on **_Install_**

{{< figure src="/images/61730892-9410-49e9-a602-23f769c0e96d.png" >}}

Click on **_Agree_**

{{< figure src="/images/bdd6ba93-2580-4c19-9d53-cd27204753fd.png" >}}

Installation successful

{{< figure src="/images/ab8fdb3e-c39c-4110-a660-015513c5be37.png" >}}

Next, use the command:

```
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -license
```

{{< figure src="/images/1a985e57-59c3-4d88-a780-8a9c02559ff5.png" >}}

Enable developer mode with the command:

```
DevToolsSecurity -enable
```

{{< figure src="/images/d8f424bc-3305-448b-9b53-01c51ef61075.png" >}}

### 4. Install Android Studio

Go to the Android Studio homepage to download it: https://developer.android.com/studio

{{< figure src="/images/a86c6597-11a7-4423-991d-d3476fdc08fe.png" >}}

Click on **Download Android Studio Electric Eel**

{{< figure src="/images/d795746a-2e11-47f6-8d46-74488d984d9f.png" >}}

Select {{< figure src="/images/66662f19-0d33-4bf9-819f-931cfac969d6.png" >}} because I am using an Apple M1 chip.

After downloading, open the file and drag it to the Applications folder.

{{< figure src="/images/232f475d-1b74-4800-a388-c92f8908bf7e.png" >}}

Then open Android Studio in the Applications folder.

{{< figure src="/images/008de8a5-bb8e-4c83-b0da-d85e1becb5f4.png" >}}

Click on **Open**

{{< figure src="/images/b1f7f83c-2bd5-48c5-a59c-8589a0ad8628.png" >}}

Click on **Ok**

{{< figure src="/images/e090ea88-693a-4450-929f-3e6813cd8ee5.png" >}}

Click on **Next**

{{< figure src="/images/088bf7b7-2957-47ce-894c-3e0cb7fe9a82.png" >}}

Select **Standard**, then click on **Next**

{{< figure src="/images/94af1101-3806-4639-abb2-ab4e0d093280.png" >}}

Click on **Next**

{{< figure src="/images/9ae4ba9d-e5be-4456-b9cb-136fe4b7f84a.png" >}}

Click on **Next**

{{< figure src="/images/7af747d3-1fa1-4c08-8afe-891a51a5613c.png" >}}

Select **Accept**, then click on **Finish**

{{< figure src="/images/2a176bf1-f04b-4429-b34d-a21d5e681297.png" >}}

Click on **Finish**

{{< figure src="/images/9cf851c7-afde-400e-aa6d-ea2e8fa2b47c.png" >}}

### 5. Install Android SDK

Android Studio is now set up. Set up the necessary SDK components on the welcome screen.

{{< figure src="/images/25979e9e-3052-4781-974f-b1354c1509ab.png" >}}

Click on **More Actions**, then select **SDK Manager**. Check the boxes as shown in the image and then click on **OK**.

{{< figure src="/images/d4196ebf-5592-430c-9b16-186ef008869e.png" >}}

{{< figure src="/images/207cbdbc-1ac1-422c-967d-cd39dd1415eb.png" >}}

### 6. Install CLI software

1. Homebrew

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

This command installs Homebrew, a package manager for macOS.

2. JDK 11

```bash
brew tap homebrew/cask-versions
brew install --cask zulu11
```

{{< figure src="/images/fc93bd72-0df0-4ec4-9488-920e7172770e.png" >}}

### 7. Install .zshrc

Open the .zshrc file and add the line below

```bash
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

{{< figure src="/images/d1d15826-5420-4580-9620-abe3e24f6d54.png" >}}

Use command to apply configuration

```bash
source $HOME/.zshrc
```

{{< figure src="/images/0c8289c3-7536-44cb-847b-c11f485eb6e2.png" >}}

### 8. Install Jenkins

We install jenkins using the command

```bash
brew install jenkins-lts
```

{{< figure src="/images/633dd259-3919-4748-946c-52e4952fa471.png" >}}

{{< figure src="/images/9195be15-f7af-4399-ab03-677a76227ee2.png" >}}

After installing successfully, we start with the command

```bash
brew services start jenkins-lt
```

{{< figure src="/images/74e1ab55-c220-4f85-bdb3-c11a85dddf1a.png" >}}

Then we go to http://127.0.0.1:8080/login?from=%2F to log into the system

{{< figure src="/images/0c1d06e6-f8b4-4631-b854-a1f97d3f2485.png" >}}

We get the default password with the command

```bash
cat /Users/duytran/.jenkins/secrets/initialAdminPassword
```

{{< figure src="/images/2fbda5d8-8a5f-4c73-ad57-d4cc542e95e7.png" >}}

Configure the public ip connection by opening all connection ips to jenkins
by command

```bash
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

Then restart the service and connect normally using the network's IP

```bash
brew services restart jenkins-lt
```

### 9. Install fastlane

We install using the fastlane command

```bash
brew install fastlane
```

{{< figure src="/images/7330aeff-0a73-4ec5-b78a-f4b74e5b1264.png" >}}

{{< figure src="/images/b3aadf4a-ccc0-4f76-91e0-1f495e19de89.png" >}}

So you have understood and installed the basic jenkins build server for React native, the next article I will guide you to build and push it to firebase distribution.
