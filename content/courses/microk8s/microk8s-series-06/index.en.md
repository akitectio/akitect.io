---
categories:
  - microk8s
date: 2023-01-12T08:00:00+08:00
description.en: First of all, the system I guide in the series will be deployed in the VM environment and the images will not be public to the Internet so I use a private registry
draft: false
featuredImage: /series/microk8s/lesson-6-installing-and-using-private-registry-in-the-microk8s-integrator.webp
images:
  - /series/microk8s/lesson-6-installing-and-using-private-registry-in-the-microk8s-integrator.webp
  - /lesson-6-installing-and-using-private-registry-in-the-microk8s-integrator/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - kubernetes
  - k8s
  - ubuntu
  - virtualbox
title: Lesson 5 - Installing and using private registry in the Microk8s integrator
url: /lesson-6-installing-and-using-private-registry-in-the-microk8s-integrator
weight: 6
---

## Kích hoạt registry

## Activate registry

I ssh into the VM **microk8s-master-01 (192.168.56.2)** to perform the following steps, where microk8s-master-01 is currently acting as the Master.

After successfully logging into microk8s-master-01, I activate the registry using the command:

```bash
microk8s enable registry
```

After activating the registry, the default persistent volume for storing images is 20G. However, you can allocate more space to fit your application needs using the command:

```bash
microk8s enable registry:size=40Gi
```

{{< figure src="/adadf521-dc7c-4a3b-9ced-3b7ba4dc5902.png" >}}

## How to use Private Registry

### 1. Install **Docker Engine** on the **Server Host**

- Refer to https://docs.docker.com/engine/install/ubuntu/ for more information.

- Uninstall the old version:

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

{{< figure src="/90625428-54b1-4f12-a0af-275518928cf9.png" >}}

### Set up repository

- Update the apt package index and install packages to allow apt to use a repository over HTTPS:

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release
```

- Add the official Docker GPG key:

```bash
 sudo mkdir -p /etc/apt/keyrings
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

- Use the following command to set up the repository:

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

{{< figure src="/7c16258e-9349-45cd-8c43-b750dd4cce2b.webp" >}}

### Install Docker Engine

1. Update the apt packages:

```bash
sudo apt-get update
```

2. Install Docker Engine, containerd, and Docker Compose:

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

3. Add Docker permissions:

```bash
sudo gpasswd -a $USER docker
newgrp docker
sudo su $USER
```

{{< figure src="/7ea474c3-8db9-463a-9a6c-cc5f42dabb01.png" >}}

### Configure Insecure registry (Allow pushing images without SSL)

On the Docker build and push machines, we need to add configuration to the **/etc/docker/daemon.json** file.

```bash
sudo nano /etc/docker/daemon.json
```

Add the following configuration:

```bash
{
  "insecure-registries" : ["192.168.56.2:32000"]
}
```

{{< figure src="/dd099e8f-6d2d-4537-9b8c-daf4a0949819.webp" >}}

Save the file and restart the Docker service:

```bash
sudo systemctl restart docker
```

Now, we can test the configuration with the following steps:

1. Pull the Nginx image:

```bash
docker pull nginx
```

{{< figure src="/8d58395e-78ad-4337-ac00-b3b428e65d1f.png" >}}

2. Tag the nginx image file to 192.168.56.2:32000/duy-tran-nginx:

```bash
docker tag nginx 192.168.56.2:32000/duy-tran-nginx
```

{{< figure src="/e8de8112-11a3-46b7-8ef2-dab56a8dfa72.webp" >}}

4. Push the image 10.19.2.92:32000/duy-tran-nginx to the registry:

```bash
docker push 192.168.56.2:32000/duy-tran-nginx
```

{{< figure src="/0c4d3201-daf1-4172-ad79-ba724f543d9a.webp" >}}

### Configure microk8s

Microk8s 1.23 and newer versions use separate server files for each image registry. For the registry http://192.168.56.2:32000, the configuration file will be located at **/var/snap/microk8s/current/args/certs.d/192.168.56.2:32000**

First, create the directory and file if they do not exist:

```bash
sudo mkdir -p /var/snap/microk8s/current/args/certs.d/192.168.56.2:32000
sudo touch /var/snap/microk8s/current/args/certs.d/192.168.56.2:32000/hosts.toml
```

Then, edit the file you just created and ensure it has the following content:

```toml
# /var/snap/microk8s/current/args/certs.d/192.168.56.2:32000/hosts.toml
server = "http://192.168.56.2:32000"

[host."http://192.168.56.2:32000"]
capabilities = ["pull", "resolve"]
```

{{< figure src="/a8d81df1-f2d5-4c51-a2e8-680e3b487cbe.png" >}}

Save the file and restart the microk8s service:

```bash
microk8s stop
microk8s start
```

### Deploy and test the newly pushed image

1. Create the file **duy-tran-nginx-all.yaml**.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: duy-tran-nginx-deployment
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      app: duy-tran-nginx
  template:
    metadata:
      labels:
        app: duy-tran-nginx
    spec:
      containers:
        - name: duy-tran-nginx
          image: 192.168.56.2:32000/duy-tran-nginx
          ports:
            - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: duy-tran-nginx-svc
  labels:
    app: duy-tran-nginx-svc
spec:
  type: NodePort
  selector:
    app: duy-tran-nginx
  ports:
    - name: http
      port: 80
      targetPort: 80
      nodePort: 30039
      protocol: TCP
```

2. Run file **duy-tran-nginx-all.yaml**

```bash
 microk8s kubectl apply -f duy-tran-nginx-all.yaml
```

{{< figure src="/92dfade4-1da8-4033-8233-bb6463997ba5.webp" >}}

There are 2 ways to test:

1. Access port 30039 to check: http://192.168.56.2:30039

{{< figure src="/6fda0347-2055-4c17-aa4d-7ca063cd074c.webp" >}}

2. Run the dashboard to check:

```bash
microk8s dashboard-proxy
```

{{< figure src="/0c878dc2-ba78-4a92-9cbd-488fcd79e1fc.webp" >}}

{{< figure src="/516a3f6a-8f07-420a-a4c6-c466d3e54e23.webp" >}}

If you find this article helpful, please give it a like and subscribe to support me.
Thank you very much ♥️♥️♥️♥️

Link to [GitHub](https://github.com/akitectio/microk8s-series/) for you to copy the files quickly.
