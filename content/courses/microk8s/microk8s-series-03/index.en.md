---
categories:
  - microk8s
date: 2023-01-06T08:00:00+08:00
description: If you are a programmer, software developer, software tester, designer, one day the project team will ask you to run the software on your computer, you have to build a system just like your computer, for easy development and testing, and limit the question "Why my computer can run, but your computer can't run", then we can use Vagrant to synchronize the development environment
draft: false
featuredImage: /series/microk8s/lesson-3-install-and-run-the-oracle-vm-virtualbox-7-virtualization-environment-using-vagrant.webp
images:
  - /series/microk8s/lesson-3-install-and-run-the-oracle-vm-virtualbox-7-virtualization-environment-using-vagrant.webp
  - /lesson-3-install-and-run-the-oracle-vm-virtualbox-7-virtualization-environment-using-vagrant/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - kubernetes
  - k8s
  - ubuntu
  - virtualbox
title: Lesson 2 - Install and run the Oracle VM VirtualBox 7 virtualization environment using Vagrant
url: lesson-2-install-and-run-the-oracle-vm-virtualbox-7-virtualization-environment-using-vagrant
weight: 3
---

what is Vagrant?

Vagrant is a tool for working with virtual environments, and in most cases, this means working with virtual machines. Vagrant provides a simple and easy-to-use command-line application for managing these environments and an interpreter for text-based definitions of how each environment should look, called Vagrantfiles. Vagrant is open source, meaning anyone can download, modify, and share it freely.

While many virtual machine programming tools provide their own command-line interfaces and technically providing virtual machines through these programs can be done directly or through shell scripts, another advantage provided by adding an additional layer is simplicity, the ability to interact across multiple systems, and a more consistent approach in theory that can be used with any virtual environment running on any other system.

By providing a popular text-based format for working with virtual machines, your environment can be defined in code, making it easy to back up, modify, share, and manage with change control. This also means that instead of sharing an entire virtual machine image, which could be many gigabytes, every time a change is made to the configuration, a simple text file can be shared that may only be a few kilobytes.

| IP           | Hostname          | vCPU   | RAM | DISK |
| ------------ | ----------------- | ------ | --- | ---- |
| 192.168.56.2 | microk8s-master-1 | 1 core | 2G  | 50G  |
| 192.168.56.3 | microk8s-master-2 | 1 core | 2G  | 50G  |
| 192.168.56.4 | microk8s-master-3 | 1 core | 2G  | 50G  |
| 192.168.56.5 | microk8s-worker-1 | 1 core | 2G  | 50G  |
| 192.168.56.6 | microk8s-worker-2 | 1 core | 2G  | 50G  |
| 192.168.56.7 | microk8s-worker-3 | 1 core | 2G  | 50G  |
| 192.168.56.8 | microk8s-worker-4 | 1 core | 2G  | 50G  |

# Installing Vagrant

### Step 1: Install Oracle VM VirtualBox 7

Refer to the previous article.

### Step 2: Install Vagrant

We go to the Vagrant homepage to download the installation file at https://developer.hashicorp.com/vagrant/downloads. Depending on the operating system, we will choose the appropriate installation file.

{{< figure src="./edbe3216-d88f-4982-8607-486cc1958c1f.png" >}}

### Step 3: After successful installation, create a Vagrantfile.

```bash
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.winrm.timeout = 1800 # 30 minutes
  config.vm.boot_timeout = 1800 # 30 minutes
  config.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
  config.vm.provision "shell", inline: <<-EOF
    apt install net-tools
    ifconfig
    snap install microk8s --classic
    microk8s.status --wait-ready
    microk8s.enable dns
    usermod -a -G microk8s vagrant
    echo "alias kubectl='microk8s.kubectl'" > /home/vagrant/.bash_aliases
    chown vagrant:vagrant /home/vagrant/.bash_aliases
    echo "alias kubectl='microk8s.kubectl'" > /root/.bash_aliases
    chown root:root /root/.bash_aliases
    echo "

    #master
    192.168.56.2  microk8s-master-01
    192.168.56.3  microk8s-master-02
    192.168.56.4  microk8s-master-03

    #worker
    192.168.56.5  microk8s-worker-01
    192.168.56.6  microk8s-worker-02
    192.168.56.7  microk8s-worker-03
    192.168.56.8  microk8s-worker-04

    " > /etc/hosts
    cat /etc/hosts
  EOF

  config.vm.define "microk8s_master_01" do |microk8s_master_01|
    microk8s_master_01.vm.hostname = "microk8s-master-01"
    microk8s_master_01.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_master_01.vm.network "private_network", ip: "192.168.56.2"
    microk8s_master_01.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-master-01"
    end
  end
  config.vm.define "microk8s_master_02" do |microk8s_master_02|
    microk8s_master_02.vm.hostname = "microk8s-master-02"
    microk8s_master_02.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_master_02.vm.network "private_network", ip: "192.168.56.3"
    microk8s_master_02.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-master-02"
    end
  end
  config.vm.define "microk8s_master_03" do |microk8s_master_03|
    microk8s_master_03.vm.hostname = "microk8s-master-03"
    microk8s_master_03.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_master_03.vm.network "private_network", ip: "192.168.56.4"
    microk8s_master_03.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-master-03"
    end
  end
  config.vm.define "microk8s_worker_01" do |microk8s_worker_01|
    microk8s_worker_01.vm.hostname = "microk8s-worker-01"
    microk8s_worker_01.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_worker_01.vm.network "private_network",  ip: "192.168.56.5"
    microk8s_worker_01.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-woker-01"
    end
  end
  config.vm.define "microk8s_worker_02" do |microk8s_worker_02|
    microk8s_worker_02.vm.hostname = "microk8s-worker-02"
    microk8s_worker_02.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_worker_02.vm.network "private_network", ip: "192.168.56.6"
    microk8s_worker_02.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-worker-02"
    end
  end
  config.vm.define "microk8s_worker_03" do |microk8s_worker_03|
    microk8s_worker_03.vm.hostname = "microk8s-worker-03"
    microk8s_worker_03.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_worker_03.vm.network "private_network", ip: "192.168.56.7"
    microk8s_worker_03.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-worker-03"
    end

  end
  config.vm.define "microk8s_worker_04" do |microk8s_worker_04|
    microk8s_worker_04.vm.hostname = "microk8s-worker-04"
    microk8s_worker_04.vm.network "public_network", bridge: "VirtualBox Host-Only Ethernet Adapter"
    microk8s_worker_04.vm.network "private_network",  ip: "192.168.56.8"
    microk8s_worker_04.vm.provider "virtualbox" do |vb|
      vb.name = "microk8s-worker-04"
    end
  end
end
```

{{< figure src="./46a04e77-c66f-4b8a-94cf-ba43d01c7fc4.png" >}}

### Step 4: Configure network

Here I create 2 networks **public_network** and **private_network**

- public_network: Connect to the Internet
- private_network: Do not connect to the Internet, only connect to the internal LAN network of VirtualBox

### Step 5: Vagrant up

After creating, we use the command **Vagrant up** to start the virtual environment.

```bash
vagrant up
```

> Depending on your network and machine configuration, this step may take 30 -> 60 minutes.

{{< figure src="./6a303659-3afa-4291-a22d-4fd4d5cb1be9.png" >}}

Then you open **Oracle VM VirtualBox** and check if the virtual machines have been created and started.

{{< figure src="./388b6633-8ff5-4d47-8552-c7ec30a86f72.png" >}}

### Step 5: Vagrant ssh and join nodes to the master

After successfully starting, we continue to ssh into the microk8s-master-01 machine using the command:

```bash
vagrant ssh microk8s_master_01
```

{{< figure src="./4923dfb0-1710-4068-b958-f6db06d5a823.png" >}}

Next, we create a token to connect to the microk8s cluster.

```bash
microk8s add-node
```

{{< figure src="./cd2e5200-4905-4ca7-82c1-6f4736f5be64.png" >}}

Then we ssh into the **microk8s-master-02** and **microk8s-master-03** machines to join them to the master nodes.

{{< figure src="./5f13ea88-a045-488f-8d5f-d07fc1bc1421.png" >}}

We use the command **microk8s join** to connect the 2 machines to the master cluster.

```bash
microk8s join 192.168.56.2:25000/f9b92e3f904dd17cba2332a88a3092da/a25a7c633d5c
```

{{< figure src="./fe7179c0-9934-4b35-bece-f3761a956f4d.png" >}}

{{< figure src="./3945f334-da82-4782-9b36-8c59eac4a499.png" >}}

We use the command **microk8s kubectl get no** to check if the 2 master nodes have joined.

```bash
microk8s kubectl get no
```

{{< figure src="./8007393e-cbb0-4565-a9d4-9095f295683d.png" >}}

So the master cluster has successfully joined, now we proceed with the worker cluster. Then we ssh into the **microk8s-worker-01**, **microk8s-worker-02**, **microk8s-worker-03**, **microk8s-worker-04** machines to join them to the worker nodes.

```bash
vagrant ssh microk8s_worker_01
```

{{< figure src="./d2b2744c-4192-456a-bbf4-070175392a4c.png" >}}

```bash
vagrant ssh microk8s_worker_02
```

{{< figure src="./c01e6c0e-e040-48c9-8d19-57ba610815eb.png" >}}

```bash
vagrant ssh microk8s_worker_03
```

{{< figure src="./d827a85c-afa0-4532-90e3-74c9ecc08250.png" >}}

```bash
vagrant ssh microk8s_worker_04
```

{{< figure src="./35623fc0-db5b-4c47-9266-812125831d00.png" >}}

After successfully logging into the workers, ssh into the **microk8s-master-01** machine and run the command **microk8s add-node**.

```bash
microk8s add-node
```

{{< figure src="./e4d809a4-7660-4b7c-8919-64f695894dbe.png" >}}

We use the token with the IP address of **192.168.56.2** and add **--worker** at the end, and apply it to all **worker** machines.

```bash
microk8s join 192.168.56.2:25000/{toke} --worker
```

{{< figure src="./844ec79a-ac53-4d78-8d59-517f08ffad37.png" >}}

We use the command **microk8s kubectl get no** on the **master 01** machine to check if the **worker nodes** have been connected.

{{< figure src="./8e830ded-3564-42b0-b457-25ce61fa4b99.png" >}}

So we have successfully created virtual machines and connected **3 master** with **4 worker**.
