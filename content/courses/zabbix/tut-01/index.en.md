---
categories:
  - devops
  - zabbix
date: 2023-03-01T08:00:00+08:00
description.en: A beautiful day after having breakfast, drinking coffee and going to the company, the boss called an urgent meeting and questions related to last night's incident, I didn't know what to say, because there were no logs, no tool monitoring system, and then I started researching and found out that Zabbix could do it, after a while I followed. follow, then the error is that the SAN array is out of memory.
draft: false
featuredImage: /series/zabbix/lession-1-monitoring-the-server-and-notifying-when-the-server-has-problems-has-never-been-difficult-with-zabbix.webp
images:
  - /series/zabbix/lession-1-monitoring-the-server-and-notifying-when-the-server-has-problems-has-never-been-difficult-with-zabbix.webp
  - /monitoring-the-server-and-notifying-when-the-server-has-problems-has-never-been-difficult-with-zabbix/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - zabbix-tutorial
tags:
  - zabbix-agent
  - Zabbix Server
  - zabbix-agent-6.2
title: Lesstion 1 - Monitoring the server and notifying when the server has problems has never been difficult with Zabbix
url: /monitoring-the-server-and-notifying-when-the-server-has-problems-has-never-been-difficult-with-zabbix
weight: 1
---

To install Zabbix server on Ubuntu 22.04, the following steps are required:

Step 1: Install Zabbix 6.2

```
sudo su
apt update && apt -y upgrade
wget https://repo.zabbix.com/zabbix/6.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.2-1+ubuntu22.04_all.deb
dpkg -i zabbix-release_6.2-1+ubuntu22.04_all.deb
apt update
root@zabbix-serverr:~# apt install -y zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-sql-scripts zabbix-agent
```

Step 2: Install and configure MySQL

```
sudo su
apt install -y mysql-server
systemctl start mysql
systemctl enable mysql
systemctl status mysql
```

Step 3: Create Zabbix database and user

```
sudo su
 mysql -uroot -p
```

After logging in, run the following commands to create the database and user:

```
create database zabbix_db character set utf8 collate utf8_bin;
create user zabbix_user@localhost identified by 'zabbix@123';
GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'zabbix_user'@'localhost' WITH GRANT OPTION;
grant all privileges on zabbix_db.* to zabbix_user@localhost;
SET GLOBAL log_bin_trust_function_creators = 1;
FLUSH PRIVILEGES;
\q
```

Step 4: Import Zabbix default database

```
sudo su
zcat /usr/share/doc/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uroot -p zabbix_db
```

Step 5: Find and change Zabbix configuration

Use the following command to open the Zabbix configuration file:

```
sudo su
nano /etc/zabbix/zabbix_server.conf
```

Add the following lines to the end of the file and save:

```
DBName=zabbix_db
DBUser=zabbix_user
DBPassword=zabbix@123
```

Step 6: Configure Nginx to point to the IP address of the server (in this case, 10.19.2.1)

Use the following command to open the Nginx configuration file:

```
sudo su
nano /etc/zabbix/nginx.conf
```

Find the line and change the configuration:

```
listen          80;
server_name	10.19.2.1
```

Step 7: Restart the services

```
sudo su
systemctl restart zabbix-server zabbix-agent nginx php8.1-fpm
systemctl enable zabbix-server zabbix-agent nginx php8.1-fpm
```

After successfully restarting the services, go to http://10.19.2.1/setup.php to perform the initial configuration. Follow the on-screen instructions to complete the setup.

{{< figure src="./images/7e7f508a-2cd5-4807-abaf-49ae0bfb57fb.png" >}}

Click on Next Step to check the system installation requirements.

{{< figure src="./images/fc27ce52-156f-4a4f-adbf-21897df87804.png" >}}

Click on Next Step to configure the database connection.

{{< figure src="./images/d3eab79b-53f6-4e22-beea-93c86f93a834.png" >}}

Click on Next Step to choose a theme or skip this step.

{{< figure src="./images/2ea5c846-7a45-4f3b-8178-5406aa22ddbf.png" >}}

Click on Next Step to complete the installation!

{{< figure src="./images/69ead9a3-7652-4250-bbe8-7d7d670d5417.png" >}}

Click on Finish to redirect to the default login page.

username: Admin

password: zabbix

{{< figure src="./images/50835f26-bca6-4fa8-847e-3591cb0fe516.png" >}}

Check the Zabbix dashboard as shown below.

{{< figure src="./images/14ed06e7-6b5b-4602-9641-034ab141d806.png" >}}
