---
categories:
  - devops
  - zabbix
date: 2023-03-02T08:00:00+08:00
description: In this article I will guide you to install Zabbix Agent 2 on CentOS 7 to monitor Mongodb server
draft: false
featuredImage: /series/zabbix/zabbix-62-canh-bao-qua-telegram.webp
images:
  - /series/zabbix/zabbix-62-canh-bao-qua-telegram.webp
  - /zabbix-agent-2-on-centos-7-server-used-to-monitor-mongodb-replica-set/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - zabbix-tutorial
tags:
  - zabbix-agent
  - zabbix-server
  - zabbix-agent-6.2
title: Lesstion 2 - Zabbix Agent 2 on CentOS 7 server used to monitor Mongodb Replica Set
url: /zabbix-agent-2-on-centos-7-server-used-to-monitor-mongodb-replica-set
weight: 2
---

Step 1: Install Zabbix Agent 2

- Install Zabbix repository

```shell
yum update -y
 rpm -Uvh https://repo.zabbix.com/zabbix/6.2/rhel/7/x86_64/zabbix-release-6.2-3.el7.noarch.rpm
yum clean all
```

- Install Zabbix Agent2

```shell
yum install zabbix-agent2 zabbix-agent2-plugin-* -y
```

- Start Zabbix Agent2 process

```shell
systemctl restart zabbix-agent2
systemctl enable zabbix-agent2
```

Step 2: Configure Zabbix Agent 2 to point to Zabbix Server

As in the Zabbix server installation at address 10.19.2.1

Use the following command to open the Zabbix configuration file:

```shell
nano /etc/zabbix/zabbix_agent2.conf
```

Find and change the configuration:

```shell
ListenIP=0.0.0.0
Server=10.19.2.1
Hostname=Zabbix Mongodb
```

Save and restart the service:

```shell
systemctl restart zabbix-agent2
```

Step 2: Log in to Zabbix and add to hosts: http://10.19.2.1/zabbix/zabbix.php?action=host.view

Select as shown in the image

{{< figure src="/images/c1706075-2222-4358-b3bf-9b97e719b869.webp" >}}

Step 3: Configure the **MongoDB node by Zabbix agent 2** template

1. Create a MongoDB user for monitoring

When the agent has been deployed and configured, you need to ensure that you have a MongoDB user that we can use for monitoring purposes. Below you can find a brief example of how you can create a MongoDB user:

Access mongodb using the command:

```shell
mongosh
```

Switch to the MongoDB admin database:

```shell
use admin
```

Create a user with the ‘userAdMinanyDatabase‘ role:

```shell
db.createUser(
... {
..... user: "zabbix_mon",
..... pwd: "zabbix_mon",
..... roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
..... }
... )
```

The username for the newly created user is ‘zabbix_mon'
The password is also ‘zabbix_mon'

2. Configure Macros as shown in the image below

{{< figure src="/images/2040661e-6603-4394-8e2e-d941b3104a3d.webp" >}}

- **{$MONGODB.PASSWORD}** – MongoDB user name. In our example, we will set this to zabbix_mon
- **{$MONGODB.USER}** – MongoDB password. In our example, we will set this value to zabbix_mon
- **{$MONGODB.CONNSTRING}** – MongoDB connection string. Specify the MongoDB address and port where Zabbix Agent 2 will connect and perform data collection

You can refer to 2 more articles:

- https://blog.zabbix.com/monitoring-mongodb-nodes-and-clusters-with-zabbix/16031/
- https://www.zabbix.com/integrations/mongodb#mongodb
