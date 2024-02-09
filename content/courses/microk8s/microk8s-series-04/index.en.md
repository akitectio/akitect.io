---
categories:
  - microk8s
date: 2023-01-07T08:00:00+08:00
description: A friend of mine once asked, why do I like microk8s more than minikube? ... Since then, we never talked again. It's a difficult question, especially for an engineer. The answer is not too clear, because it has to be experienced and personal preference. Let me show you why.
draft: false
featuredImage: /series/microk8s/lesson-4-installing-kubernetes-with-microk8s-1-26-on-ubuntu-22-04.webp
images:
  - /series/microk8s/lesson-4-installing-kubernetes-with-microk8s-1-26-on-ubuntu-22-04.webp
  - /lesson-4-installing-kubernetes-with-microk8s-1-26-on-ubuntu-22-04/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - kubernetes
  - k8s
  - ubuntu
  - virtualbox
title: Lesson 3 - Installing Kubernetes with Microk8s 1.26 on Ubuntu 22.04
url: /lesson-4-installing-kubernetes-with-microk8s-1-26-on-ubuntu-22-04
weight: 4
---

What is MicroK8s?

MicroK8s is a lightweight and independent Kubernetes platform, developed by Canonical, the provider of the Ubuntu operating system. It is designed to help developers and system administrators quickly and easily deploy and manage cloud applications and services on Kubernetes.

MicroK8s provides a simple way to deploy a Kubernetes cluster on a personal computer, a development environment, or a data center. It provides users with some important features such as:

- Quick and easy installation: MicroK8s can be installed with just one command and does not require specialized knowledge of Kubernetes.
- Independent and flexible: MicroK8s operates as an independent platform and can be deployed on any operating system.
- Good integration with popular development and deployment tools: MicroK8s integrates well with popular development and deployment tools such as Docker, Helm, kubectl, and other DevOps tools.
- Security and safety: MicroK8s provides security and safety features such as network firewall, TLS, and RBAC to keep your Kubernetes cluster safe and secure.

> In summary, MicroK8s is a lightweight and independent Kubernetes solution that helps users quickly and easily deploy and manage cloud applications in different environments.

## **Benefits of using MicroK8s**

1. Easy to install, only requires your machine to support [snap](https://snapcraft.io/)
2. Supports over 42 Linux, Windows, and Mac OS operating systems
3. Quickly supports core Kubernetes addons such as DNS and dashboard
4. Control your cluster with the kubectl CLI tool
5. Quickly deploy containers in the cluster

> In this article, I will guide you on how to install an 11-node cluster according to the HA (High Availability) model, including 3 master nodes and 4 worker nodes with high availability.

You need to prepare 7 servers with the following configuration:

**Server List**

| IP           | Hostname          | vCPU   | RAM | DISK |
| ------------ | ----------------- | ------ | --- | ---- |
| 192.168.56.2 | microk8s-master-1 | 1 core | 2G  | 50G  |
| 192.168.56.3 | microk8s-master-2 | 1 core | 2G  | 50G  |
| 192.168.56.4 | microk8s-master-3 | 1 core | 2G  | 50G  |
| 192.168.56.5 | microk8s-worker-1 | 1 core | 2G  | 50G  |
| 192.168.56.6 | microk8s-worker-2 | 1 core | 2G  | 50G  |
| 192.168.56.7 | microk8s-worker-3 | 1 core | 2G  | 50G  |
| 192.168.56.8 | microk8s-worker-4 | 1 core | 2G  | 50G  |

{{< youtube "0EAkiuABZwM" >}}

\*_I will proceed with the installation according to the following model_

{{< figure src="./4306a5e6-a54d-4d9b-8cd5-fcace72b60e8.jpg" >}}

As this is my first time drawing by hand, it may be a bit difficult to see, but I will try to make it look better in the next update <3

**Let's get started:**

### Step 1: ssh into each node and change the hostname as shown above

- Go to the **/etc/hostname** directory
- Change the **hostname** according to each server in the **List of servers**

* Master servers

{{< figure src="./7d487a2e-cd78-472e-888e-7a8521183cf7.png" >}}

- Worker servers

{{< figure src="./ff654f99-1af8-412d-b843-c8e80ecd6f60.png" >}}

### Step 2: Update and upgrade all nodes

```
sudo apt update && apt upgrade -y
```

### Step 3: Add fields to the hosts file

- Mở file: /etc/hosts

```bash
nano /etc/hosts
```

- Add at the end of the file:

```bash
#master
192.168.56.2  microk8s-master-01
192.168.56.3  microk8s-master-02
192.168.56.4  microk8s-master-03

#worker
192.168.56.5  microk8s-worker-1
192.168.56.6  microk8s-worker-2
192.168.56.7  microk8s-worker-3
192.168.56.8  microk8s-worker-4
```

- Master nodes

  {{< figure src="./a35e4a4a-d9c3-4ed8-832d-b687a1e3d7ce.png" >}}

- Worker nodes

  {{< figure src="./5548e489-d1b9-4656-b956-a96bd506665a.png" >}}

### Step 4: Install MicroK8s

```bash
sudo snap install microk8s --classic --channel=1.26
```

MicroK8s creates a group to enable seamless use of commands that require root privileges. To add your current user to the group and have access to the Kube cache directory, run the following two commands:

```bash
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
su - $USER
```

Check the status of MicroK8s with the command `microk8s status --wait-ready`

- Master servers

  {{< figure src="./f5a0cde6-bec6-4cb1-ae58-d247e1e037db.png" >}}

### Step 5: Add nodes to the cluster

As my system runs on the host machine using VirtualBox, I opened the firewall. You need to allow the microk8s port to execute the command without being blocked by the port. Please refer to the link https://microk8s.io/docs/services-and-ports.

Please execute the following command on all nodes:

```bash
sudo ufw allow 16443/tcp
sudo ufw allow 10250/tcp
sudo ufw allow 10255/tcp
sudo ufw allow 25000/tcp
sudo ufw allow 12379/tcp
sudo ufw allow 10257/tcp
sudo ufw allow 10259/tcp
sudo ufw allow 19001/tcp
sudo ufw allow 4789/udp

 sudo ufw allow 10248/tcp
sudo ufw allow 10249/tcp
sudo ufw allow 10251/tcp
sudo ufw allow 10252/tcp
sudo ufw allow 2380/tcp
sudo ufw allow 1338/tcp
```

- Create add-node token on **microk8s-master-01**:

```bash
microk8s add-node --token-ttl 3600
```

{{< figure src="./4a7b8e80-85bd-4a81-9862-947fed46027b.png" >}}

I have 2 networks:

1. Internet
2. LAN (used to connect nodes)

- Join the master servers, here I execute the add command on the **microk8s-master-01** server, so I need to add 2 servers **microk8s-master-02** and **microk8s-master-03**

```bash
microk8s join 192.168.56.2:25000/e523c2d3aef2e3679c3e5ccf605d97c2/dbc9df54be3b
```

- Continue to join the workers to the nodes **microk8s-worker-1, microk8s-worker-2, microk8s-worker-3, microk8s-worker-4** with the same token above, and just add **_--worker_** at the end of the join command.

```bash
microk8s join 192.168.56.2:25000/e523c2d3aef2e3679c3e5ccf605d97c2/dbc9df54be3b --worker
```

After successfully joining, use the command `microk8s kubectl get no` on the VM **microk8s-master-01** to check if the nodes have joined as master and worker nodes.

{{< figure src="./5b7291da-efb3-4e4f-ba93-f9be3646cc1d.png" >}}

To check the master nodes, use the command `microk8s status` on the VM **microk8s-master-01**.

{{< figure src="./abfe6089-285c-4111-91a2-658fa74f9bbc.png" >}}

### Step 6: Activate the dashboard, DNS, and storage addons.

```bash
microk8s enable dns dashboard hostpath-storage
```

After successful installation, use the command **microk8s dashboard-proxy** on the VM **microk8s-master-01** to open the dashboard.

```bash
microk8s dashboard-proxy
```

{{< figure src="./959033c4-3f29-4284-a076-d6da2e730f95.png" >}}

Access the link https://192.168.56.2:10443 to enter the **Kubernetes Dashboard**.

{{< figure src="./9773ef9c-0ada-4643-8e91-5988d4df4ba3.png" >}}

You can refer to the official website for more information: https://microk8s.io/docs/high-availability

Thank you for following my article!
