---
categories:
  - microk8s
date: 2023-01-05T08:00:00+08:00
description: In the previous article, I have guided you to install Oracle VM VirtualBox 7 on ubuntu 22.04, this article I will guide you to create Ubuntu VMs to practice this series
draft: false
featuredImage: /series/microk8s/lesson-2-create-an-ubuntu-2204-virtual-machine-using-oracle-vm-virtualbox-7.webp
images:
  - /series/microk8s/lesson-2-create-an-ubuntu-2204-virtual-machine-using-oracle-vm-virtualbox-7.webp
  - /lesson-2-create-an-ubuntu-2204-virtual-machine-using-oracle-vm-virtualbox-7/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - kubernetes
  - k8s
  - ubuntu
  - virtualbox
title: Lesson 1 - Create an Ubuntu 22.04 virtual machine using Oracle VM VirtualBox 7
url: /lesson-2-create-an-ubuntu-2204-virtual-machine-using-oracle-vm-virtualbox-7
weight: 2
---

Terminology used in the article

| Abbreviation | Meaning         |
| ------------ | --------------- |
| VM           | Virtual Machine |

In this article, I will create a VM configuration as follows:

| IP           | Hostname          | vCPU   | RAM | DISK |
| ------------ | ----------------- | ------ | --- | ---- |
| 192.168.56.2 | microk8s-master-1 | 1 core | 2G  | 50G  |
| 192.168.56.3 | microk8s-master-2 | 1 core | 2G  | 50G  |
| 192.168.56.4 | microk8s-master-3 | 1 core | 2G  | 50G  |
| 192.168.56.5 | microk8s-worker-1 | 1 core | 2G  | 50G  |
| 192.168.56.6 | microk8s-worker-2 | 1 core | 2G  | 50G  |
| 192.168.56.7 | microk8s-worker-3 | 1 core | 2G  | 50G  |
| 192.168.56.8 | microk8s-worker-4 | 1 core | 2G  | 50G  |

### Step 1: Download the Ubuntu installation file from the official website

- Download link: https://ubuntu.com/download/server

{{< figure src="./fbc3244b-88d1-43fe-bcc8-78c82d5c0515.webp" >}}

{{< figure src="./0652f90d-fd93-4eeb-a04e-3651dc88d8e5.webp" >}}

### Step 2: Start creating a Ubuntu 22.04 virtual machine

Click on "Create a new VM" with the following information:

Name: microk8s-master-01 (Name of the VM)
Folder: Storage folder
IOS Image: Path to the file created in step 1

{{< figure src="./92e7c5ee-b3ea-4ee0-850d-a744dc904b5e.webp" >}}

Enter complete information and click next

{{< figure src="./84d85194-2fbd-443e-96f5-e3efffbf5295.webp" >}}

Continue to click next

{{< figure src="./d975db34-61a4-4985-825d-9302e000a8ab.webp" >}}

As this is a test environment, I will only set 1 core and 2GB of RAM. Continue to click next

{{< figure src="./4e3a058e-fb51-46d7-b661-c20dfbddbe18.webp" >}}

I will set the hard drive to 50GB for this part

{{< figure src="./99793764-81a2-4268-8a3d-06f0b045fc17.webp" >}}

Continue to click next

{{< figure src="./b3b40851-534a-42e7-9853-cedaf5669204.webp" >}}

Review the screen and click "Finish" to successfully create the VM configuration.

### Step 3: Start and initial configuration for the virtual machine

{{< figure src="./baf58fde-6d2f-44d3-9eec-626b61f7a419.webp" >}}

{{< figure src="./aacb5261-62d2-4d27-93d0-aa53c00b5318.webp" >}}

{{< figure src="./0475c12f-d0f5-4555-916e-7a25aec814f3.webp" >}}

{{< figure src="./5f083762-20ab-4bd0-8fff-6f29860e428c.webp" >}}

{{< figure src="./b53db97b-a2fa-4f74-a6e3-5327a611de4b.webp" >}}

{{< figure src="./db9b38d7-295b-41d4-b359-74cfb3cc6f0f.webp" >}}

{{< figure src="./954f7809-597c-4e08-8b1f-b2d8f3c7158c.webp" >}}

{{< figure src="./6688afff-c902-406b-b9ec-add469fe0bd4.webp" >}}

Wait for the update and then click reboot.

{{< figure src="./9a52d76e-bae4-4f42-b59f-901f4be06e65.webp" >}}

### Step 4: Configure IP

Add an internal network on the virtualbox network.

In the tools section, select create host-only networks.

{{< figure src="./9a52d76e-bae4-4f42-b59f-901f4be06e65.webp" >}}

Here I create a network class C:

IP: 192.168.56.x

Gateway: 192.168.56.1

Subnet max: 255.255.255.0

In the settings of microk8s-master-01, select Adapter 2 and select as shown in the figure.

{{< figure src="./8fbf1a50-73b1-4d65-a2e6-418bb55988a9.webp" >}}

Then in the VM ssh interface, follow these steps:

1. Check the name of the network card just added.

```bash
ls /sys/class/net/
```

{{< figure src="./31f0f083-d301-4631-8b81-038c1b2e2345.webp" >}}

Here the name of the newly added network card is **enp0s8**.

2. Configure the static IP according to the description in the article, here is the microk8s-master-01 host with IP 192.168.56.1.

Use the command on the VM to set the IP.

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

{{< figure src="./c0664f29-8cbe-4e20-a026-c1911bd6dd0d.webp" >}}

Then add to the file as shown:

{{< figure src="./0474bfbd-8efa-49cf-a602-3ce2e6d7a3d3.webp" >}}

```yml
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      dhcp4: no
      addresses: [192.168.56.2/24]
  version: 2
```

Then use the command `sudo netplan apply` to apply the IP configuration.

Then I use the command `ip addr` to check if the IP configuration has been applied or not.

{{< figure src="./81cccf0d-3681-4959-9a25-a040b5392f10.webp" >}}

As shown above, I see that the IP **192.168.56.2** has been applied.

At this point, I have successfully set up a static IP for the VM.

### Step 5: Clone VM microk8s-master-01 and configure IP, hostname for the remaining 6 VMs

{{< youtube ntnTN4BSRPc >}}

Thank you for following the article <3.
