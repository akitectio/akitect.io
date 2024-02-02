---
categories:
  - devops
  - zabbix
date: 2023-03-02T08:00:00+08:00
description: In this article I will guide you to install Zabbix Agent 2 on Ubuntu 22.04 to monitor PostgreSQL server
draft: false
featuredImage: /series/zabbix/zabbix-agent-2-on-centos-7-server-used-to-monitor-mongodb-replica-set.webp
images:
  - /series/zabbix/zabbix-agent-2-on-centos-7-server-used-to-monitor-mongodb-replica-set.webp
  - /zabbix-agent-2-tren-ubuntu-2204-theo-doi-may-chu-postgresql/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - zabbix-tutorial
tags:
  - zabbix-agent
  - Zabbix Server
  - zabbix-agent-6.2
title: Zabbix Agent 2 on Ubuntu 22.04 monitors a PostgreSQL server
url: /zabbix-agent-2-tren-ubuntu-2204-theo-doi-may-chu-postgresql
weight: 3
---

Step 1: Install Zabbix Agent 2

- Install Zabbix repository

```
# wget https://repo.zabbix.com/zabbix/6.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.2-4%2Bubuntu22.04_all.deb
# dpkg -i zabbix-release_6.2-4+ubuntu22.04_all.deb
# apt update
```

- Install Zabbix Agent2

```
 apt install zabbix-agent2 zabbix-agent2-plugin-*
```

- Start Zabbix Agent2 process

```perl
# systemctl restart zabbix-agent2
# systemctl enable zabbix-agent2
```

Step 2: Configure Zabbix Agent 2 to point to Zabbix Server

As in the Zabbix server installation at address 10.19.2.1

Use the following command to open the Zabbix configuration file:

```perl
nano /etc/zabbix/zabbix_agent2.conf
```

Find and change the configuration:

```python
ListenIP=0.0.0.0
Server=10.19.2.1
Hostname=Zabbix PostgreSQL
```

Save and restart the service:

```perl
 systemctl restart zabbix-agent2
```

Step 2: Log in to Zabbix and add to hosts: http://10.19.2.1/zabbix/zabbix.php?action=host.view

Select as shown in the image

{{< figure src="./images/3d973b40-9232-4ba5-bd28-36ab5bb27a34.png" >}}

Step 3: Configure the **PostgreSQL by Zabbix agent 2** template

1. Create a PostgreSQL user for monitoring (password here is set to **Password@123**):

```
CREATE USER zbx_monitor WITH PASSWORD 'Password@123' INHERIT;
GRANT EXECUTE ON FUNCTION pg_catalog.pg_ls_dir(text) TO zbx_monitor;
GRANT EXECUTE ON FUNCTION pg_catalog.pg_stat_file(text) TO zbx_monitor;
GRANT EXECUTE ON FUNCTION pg_catalog.pg_ls_waldir() TO zbx_monitor;
```

2. Edit pg_hba.conf to allow connections from Zabbix:

```
# TYPE  DATABASE        USER            ADDRESS                 METHOD
  host       all        zbx_monitor     localhost               md5
```

For more information, please refer to the PostgreSQL documentation https://www.postgresql.org/docs/civerse/auth-pg-hba-conf.html

3. Set the system data source name of the PostgreSQL version in the **{$PG.URI}** macro, such as **<protocol(host:port)>**
4. Set the username and password in the server macro **({$PG.USER}** and **{$PG.PASSWORD}**) if you want to override the parameters from the Zabbix agent configuration file

{{< figure src="./images/003c2e76-a17a-4a8c-ab41-3c544e8a3417.png" >}}

Reference link: https://www.zabbix.com/integrations/postgresql#postgresql_agent2
