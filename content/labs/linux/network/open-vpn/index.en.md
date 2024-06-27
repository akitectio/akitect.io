---
categories:
  - linux
  - network
date: 2024-02-17T10:23:30-09:00
description: Open VPN là một phần mềm mã nguồn mở giúp tạo ra một mạng riêng ảo (VPN) giữa các máy tính. OpenVPN có thể chạy trên nhiều hệ điều hành khác nhau, bao gồm cả Linux, Windows, Mac OS X, và các thiết bị di động.
draft: false
featuredImage: /labs/open-vpn/featured-image.webp
images:
  - /labs/open-vpn/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - vpn
title: Install Open VPN Server on ubuntu 22.04
weight: 1
---

> To connect from the external network to the servers in the LAN of the servers in the network, we need to configure the VPN Server for access, in this article, I will use Open VPN Server

## Concept
* OpenVPN is a full-featured SSL VPN (Virtual Private Network). It implements Layer 2 or 3 network extensions with the SSL/TLS protocol. It is open-source software and is distributed under the GNU GPL. VPN allows you to securely connect to an insecure public network such as WiFi at airports or hotels. VPN is also required to access your company, business, or home server resources. You can bypass geographically blocked websites and increase online privacy or security. This guide provides step-by-step instructions to configure an OpenVPN server on an Ubuntu Linux 22.04 server.

## Instructions

To perform the installation, follow these steps:

### Step 1: Update your system, your operating system, here I use Ubuntu 22.04

We update the packages with the apt command

```bash
sudo apt update && sudo apt upgrade -y
```
{{<figure src="/images/7f32aed5-2f3a-4396-8b09-3da3f714073f.webp" >}}

### Step 2: Find and record your IP 

To find your IP, we use the command: 

```bash
ip a
ip a show enp4s0
```

{{<figure src="/images/e0f12e13-e3ca-4aae-9333-0720b8e56d2c.webp" >}}


### Step 3: Download and run the ubuntu-22.04-lts-vpn-server.sh script

```bash
wget https://raw.githubusercontent.com/Nyr/openvpn-install/master/openvpn-install.sh -O ubuntu-22.04-lts-vpn-server.sh
```

{{<figure src="/images/78e79b5b-c865-4532-8f96-8148504edba2.webp" >}}

then we set permissions for the newly downloaded file with the command 

```bash
chmod -v +x ubuntu-22.04-lts-vpn-server.sh
```

Run **ubuntu-22.04-lts-vpn-server.sh** to install OpenVPN Server

```bash
sudo ./ubuntu-22.04-lts-vpn-server.sh
```

{{<figure src="/images/759f014d-721a-4a67-a41f-b0fce57005b3.webp" >}}
{{<figure src="/images/5a573a80-35f2-411c-bbf8-fb1837822fa7.webp" >}}

then we check the status of openVPN

```bash
sudo systemctl status openvpn-server@server
```
{{<figure src="/images/6f81e43a-4a69-41a8-b41c-6f4355e8e4f7.webp" >}}

Then we download the **client.ovpn** file to the machine that needs to go into VPN 

We find the .ovpn file with the command 

```bash
sudo find / -iname "*.ovpn" -ls
```

So we have successfully installed OpenVPN

Next, we install the Open VPN Client for the machine that needs to go into VPN

1. [Apple IOS client ](https://apps.apple.com/us/app/openvpn-connect/id590379981)
2. [ Android client ](https://play.google.com/store/apps/details?id=net.openvpn.openvpn&hl=en)
3. [Apple MacOS client](https://openvpn.net/client-connect-vpn-for-mac-os/)

Here I use Mac M1 and successfully connect

{{<figure src="/images/ca0bfdb6-495f-4cb6-a333-2dbef13d098e.webp" >}}

### Reference Links
* https://www.cyberciti.biz/faq/ubuntu-22-04-lts-set-up-openvpn-server-in-5-minutes/