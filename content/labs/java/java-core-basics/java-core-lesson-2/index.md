---
categories:
  - java
date: 2023-02-01T08:00:00+08:00
description: Một số phần mềm còn sử dụng phiên bản cũ của Java, chẳng hạn như JDK 11, do đó cài đặt JDK 11 sẽ giúp bạn có thể chạy các ứng dụng đó trên máy tính Mac M1 của mình. Bài viết này sẽ hướng dẫn bạn cách cài đặt JDK 11 và thiết lập biến môi trường Java Home trên Mac M1.
draft: false
featuredImage: /series/java-core-basic/java-core-lesson-2-install-jdk-11-development-environment-and-tools-and-set-up-java-home-on-mac-m1.webp
images:
  - /series/java-core-basic/java-core-lesson-2-install-jdk-11-development-environment-and-tools-and-set-up-java-home-on-mac-m1.webp
  - /java-core-lesson-2-install-jdk-11-development-environment-and-tools-and-set-up-java-home-on-mac-m1/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - java-core-basics
tags:
  - Java
  - Java core
  - JDK 11
  - Apple M1 Silicon
  - Mac M1
  - eclipse
  - Java Home
title: Java Core Lesson 2 - Cài đặt môi trường và công cụ phát triển JDK 11 và thiết lập Java Home trên Mac M1
url: /java-core-lesson-2-cai-dat-moi-truong-va-cong-cu-phat-trien-jdk-11-va-thiet-lap-java-home-tren-mac-m1
weight: 2
---

Java là một trong những ngôn ngữ lập trình phổ biến nhất hiện nay với rất nhiều ứng dụng trên khắp thế giới. Trong bài học này, chúng ta sẽ tìm hiểu về quy tắc đặt tên, kiểu dữ liệu và ép kiểu, toán tử, cấu trúc điều khiển và rẽ nhánh. Việc hiểu và sử dụng thành thạo các khái niệm này sẽ giúp bạn trở thành một lập trình viên Java giỏi. Hãy bắt đầu học các khái niệm này ngay bây giờ!

### 1. Install Homebrew

To be able to install, you must have Homebrew installed first.

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

{{< figure src="./images/743a5aa1-a306-4e9c-96a2-8502dcef8a3f.webp" >}}

Add Homebrew to your path.

```
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/vishal/.zprofile eval "$(/opt/homebrew/bin/brew shellenv)"
```

Check if Homebrew is in your path.

```
brew --version
```

{{< figure src="./images/f38ab74d-cb58-476f-9460-da6a6d24e64f.webp" >}}

### 2. Install JAVA

Next, of course, you need to install Java if you haven't already.

Install **Rosetta 2**

```
sudo softwareupdate --install-rosetta
```

Install Java using Homebrew.

```
brew tap homebrew/cask-versions && brew install --cask zulu11
```

{{< figure src="./images/e81e97d2-eef7-4cbf-b54b-5616f4597973.webp" >}}

Check if Java is installed and working.

```
java --version
```

{{< figure src="./images/7cf129b3-1f4d-469c-b905-90c3b57c1feb.webp" >}}

So we have successfully installed Java 11.

### 3. Install Eclipse IDE for Apple M1 Silicon

Update January 2022: Since writing this article, official releases are now available with AArch64 MacOS versions for Mac M1 machines from the official download page: https://www.eclipse.org/downloads/packages/

{{< figure src="./images/25fe3e50-22d9-404e-89ac-ee741b8c63fc.webp" >}}

Then download the installation package for M1.

{{< figure src="./images/9bc50678-5380-4469-a843-874943ab1afc.webp" >}}

After successfully downloading, click on the installation icon.

{{< figure src="./images/b5456fd6-cdc4-4f5e-8db5-d9642cd7f37d.webp" >}}

Then we choose open.

{{< figure src="./images/c25eb979-3b0c-4871-b073-659333cfada6.webp" >}}

Select **"Eclipse IDE for Java Developers"**

{{< figure src="./images/516e8115-d66e-4d9c-8b91-b7f7f5a54648.png" >}}

Then select **Install**

{{< figure src="./images/92d47027-a6f9-4d94-a0a0-6b9e51531a7e.webp" >}}

Then select **Launch**

{{< figure src="./images/f2172f58-e0c4-43c8-8736-63ff9811e58a.webp" >}}

So we have successfully installed the development environment and tools.
