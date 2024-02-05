---
categories:
  - microk8s
date: 2023-01-13T08:00:00+08:00
description: Jenkins is an open-source automation server that automates repetitive technical tasks involved in the continuous integration and delivery of software. Jenkins is based on Java, which can be installed from Ubuntu packages or by downloading and running its web application archive (WAR) file—a collection of files that make up a complete web application to run on a server.
draft: false
featuredImage: /series/microk8s/lesson-7-configuring-jenkins-on-ubuntu-2204-and-writing-pipeline-build-service.webp
images:
  - /series/microk8s/lesson-7-configuring-jenkins-on-ubuntu-2204-and-writing-pipeline-build-service.webp
  - /lesson-7-configuring-jenkins-on-ubuntu-2204-and-writing-pipeline-build-service/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - Kubernetes
  - k8s
  - ubuntu
  - virtualbox
  - virtualbox 7
  - virtualbox 7 on ubuntu 22.04
  - jenkins-agent
  - Pipeline
title: Lesson 7 - Configuring Jenkins on Ubuntu 22.04 and writing Pipeline Build Service
url: /lesson-7-configuring-jenkins-on-ubuntu-2204-and-writing-pipeline-build-service
weight: 7
---

In this guide, you will install Jenkins on Ubuntu 22.04, start the development server, and create an administrative user to begin exploring Jenkins automation. By the end of this guide, you will have a Jenkins server that is not yet ready for deployment.

## System Requirements

- A Ubuntu 22.04 server configured with a non-root sudo user and a firewall by following the initial server setup guide for Ubuntu 22.04. Here, I recommend starting with at least 1 GB of RAM. You can refer to the hardware requirements at: https://www.jenkins.io/doc/book/scaling/hardware-recommendations/.
- Oracle JDK 11 has been installed.

{{< youtube "fDpdOJF_BlY" >}}

### Step 1: Install Oracle JDK 11

To install the OpenJDK version of Java, first update your **apt** package index:

```nginx
sudo apt update
```

Next, check if Java is already installed:

```nginx
java -version
```

If Java is not yet installed, you will receive the following output:

```nginx
Output
Command 'java' not found, but can be installed with:

sudo apt install default-jre              # version 2:1.11-72build1, or
sudo apt install openjdk-11-jre-headless  # version 11.0.14+9-0ubuntu2
sudo apt install openjdk-17-jre-headless  # version 17.0.2+8-1
sudo apt install openjdk-18-jre-headless  # version 18~36ea-1
sudo apt install openjdk-8-jre-headless   # version 8u312-b07-0ubuntu1
```

Execute the following command to install the JRE from OpenJDK 11:

```nginx
sudo apt install openjdk-11-jre-headless
```

The JRE will allow you to run most Java software.

Verify the installation with:

```nginx
java -version
```

```nginx
Output
openjdk version "11.0.14" 2022-01-18
OpenJDK Runtime Environment (build 11.0.14+9-Ubuntu-0ubuntu2)
OpenJDK 64-Bit Server VM (build 11.0.14+9-Ubuntu-0ubuntu2, mixed mode, sharing)
```

You have successfully installed OpenJDK 11.

### Step 2: Install Jenkins

The version of Jenkins that comes with the default Ubuntu packages is usually the latest version.

First, add the repository key to your system:

```nginx
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key |sudo gpg --dearmor -o /usr/share/keyrings/jenkins.gpg
```

Next, add the Debian package repository address to your server's **sources.list**:

```nginx
sudo sh -c 'echo deb [signed-by=/usr/share/keyrings/jenkins.gpg] http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```

The **[signed-by=/usr/share/keyrings/jenkins.gpg]** part of the line ensures that **apt** will verify the files in the repository using the GPG key you just downloaded.

After both commands have been entered, run an **APT** update so that **APT** will use the new repository.

```nginx
sudo apt update
```

Finally, install Jenkins with the command:

```nginx
sudo apt install jenkins
```

Now that Jenkins and its dependencies are installed, we will start the Jenkins server.

### Step 3: Start Jenkins

Now that Jenkins is installed, let's start it using **systemctl**:

```nginx
sudo systemctl start jenkins.service
```

Since **systemctl** does not display status output, we will use the status command to verify that Jenkins has started successfully:

```nginx
sudo systemctl status jenkins
```

If everything went smoothly, the first part of the status output shows that the service is running and configured to start on boot:

```nginx
Output
● jenkins.service - Jenkins Continuous Integration Server
     Loaded: loaded (/lib/systemd/system/jenkins.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2023-01-19 16:07:28 UTC; 2min 3s ago
   Main PID: 88180 (java)
      Tasks: 42 (limit: 4665)
     Memory: 1.1G
        CPU: 46.997s
     CGroup: /system.slice/jenkins.service
             └─88180 /usr/bin/java -Djava.awt.headless=true -jar /usr/share/java/jenkins.war --webroot=/var/cache/jenkins/war --httpPort=8080
```

Now that Jenkins is up and running, you need to enable the firewall and use it.

### Step 4: Configure Firewall

By default, Jenkins runs on port **8080**. Open that port using **ufw**:

```nginx
sudo ufw allow 8080
```

Check the status of **UFW** to confirm the new rules:

```nginx
sudo ufw status
```

You will see that access is allowed to port 8080 from anywhere:

```nginx
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
8080                       ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
8080 (v6)                  ALLOW       Anywhere (v6)
```

With Jenkins installed and the firewall configured, you have completed the installation phase and can proceed to configure Jenkins.

### Step 5: Initial Setup for Jenkins

To set up your installation, access Jenkins on its default port, 8080, using your server's domain name or IP address: http://host:8080 (where Host is the IP of your host machine).

You will receive the Unlock Jenkins screen, displaying the location of the initial password:

{{< figure src="./de4f0dc9-e268-483d-ae7e-f51b3ddcd132.webp" >}}

In the terminal window, use the **cat** command to display the password:

```nginx
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the 32-character alphanumeric password from the terminal and paste it into the password field, then click Continue.

The next screen displays the option to install suggested plugins or to select specific plugins:

{{< figure src="./8a198c4d-8b5e-4185-acc1-4e29b28eff05.webp" >}}

Here, I recommend using **_Install suggested plugins_** to continue the installation process.

{{< figure src="./e96dd023-2710-4916-a1e9-bc3d921e846f.webp" >}}

Continue to wait for the installation process to complete.

After the installation process is complete, the next step is to create an admin user.

{{< figure src="./9a838a23-5d03-4058-a44a-c5083705f311.webp" >}}

Continue by entering a username with a password.

{{< figure src="./f00d8021-76eb-4d96-8aed-2895828271bb.webp" >}}

then click "**Save and Finish**" to complete the setup.

{{< figure src="./7e5f7ff4-359e-4b0d-a28b-96e75ab6c303.webp" >}}

So you have completed the installation and initial setup for Jenkins, next I will write the build for image service01 and service02.

### Step 6: Write Code Pipeline Build Service

{{< youtube "v3cWZGKiR-s" >}}

Here, I have written the Jenkins build code and pushed it to GitHub for you to download.

You can download the code from the following GitHub repository:

https://github.com/akitectio/microk8s-series/tree/main/microservices

In the build directory, there will be a Jenkinsfile that I have written with the following structure:

```nginx
// Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any
    stages {
        stage('Build jar') {
            steps {
                script {
                        sh 'sh ./microservices/service01/build-image.sh' // Trỏ đến thư mục build
                }
            }
        }
    }
    post {
        always {
            deleteDir() /* clean up our workspace */
        }
    }
}
```

build-image .sh

```bash
#!/bin/bash

#remove images
docker rmi $(docker images '192.168.56.2:32000/service01' -a -q) //Xoá image cũ

docker build --no-cache=true -t 192.168.56.2:32000/service01 .  -f ./microservices/service01/Dockerfile // Build lại image mới
echo "Finish build docker images  "

echo "Begin Push Images to Local Docker Repository"

docker push "192.168.56.2:32000/service01:latest" // Đẩy image lên repository

echo "Finish Push Images to Local Docker Repository"
docker image ls

exit 0
```

Thank you for watching, in the next article, I will guide you on how to use Kong Gateway to deploy an API Gateway for the Microservices system on the 2 images that you have just successfully built.
