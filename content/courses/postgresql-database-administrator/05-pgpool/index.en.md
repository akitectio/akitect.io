---
categories:
  - database
date: 2024-02-24T08:00:00+08:00
draft: false
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images:
  - /labs/postgresql/postgresql-pgpool.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - postgresql
  - ubuntu
  - pgpool
title: Lesson 5 - Postgres Pgpool-II Ubuntu
url: /pgpool-ii-ubuntu-step-by-step-configuration
description: PGpool-II is a unique intermediate solution, designed specifically to optimize and scale PostgreSQL database management system. It brings many benefits such as optimizing connections, load balancing, and data replication, making PGpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure PGpool-II on the Ubuntu Linux operating system, helping you maximize the performance and high availability of your database.
weight: 5
---

# What is Pgpool-II

Pgpool-II is a unique middleware solution, specially designed to optimize and scale the capabilities of the PostgreSQL database management system. It offers numerous benefits such as optimizing connections, load balancing, and performing data replication, making Pgpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure Pgpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.

# Installation Architecture

{{< figure src="./images/postgresql-pgpool.jpeg" >}}


Before starting, we need to prepare 4 servers

| IP           | Hostname             | vCPU   | RAM | DISK | OS           |
| ------------ | -------------------- | ------ | --- | ---- | ------------ |
| 192.168.56.5 | pgpool2              | 2 core | 4G  | 50G  | Ubuntu 22.04 |
| 192.168.56.2 | postgresql-master    | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave-01  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | postgresql-slave-02  | 4 core | 8G  | 50G  | Ubuntu 22.04 |

### Install PostgreSQL Replication 

[Install PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc) on 3 servers `postgresql-master` and `postgresql-slave-01`, `postgresql-slave-02`.

### Install PGpool-II

#### Step 1: Install PGpool-II

First, install PGpool-II on the `pgpool2` server by performing the following steps:

```bash
sudo apt update
sudo apt install -y pgpool2
```

#### Step 2: Configure PGpool-II

After the installation is complete, we will configure PGpool-II by editing the configuration file `/etc/pgpool2/pgpool.conf`:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```

Add the following configuration to the file:

```bash
listen_addresses = '*'
port = 5432 

# only write queries are load balanced
backend_hostname0 = '192.168.56.2' 
backend_port0 = 5432
backend_weight0 = 2  
backend_data_directory0 = '/var/lib/postgresql/16/main' 
backend_application_name0 = 'postgresql-master'

# only read queries are load balanced
backend_hostname1 = '192.168.56.3'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/var/lib/postgresql/16/main' 
backend_application_name0 = 'postgresql-slave-01'

backend_hostname2 = '192.168.56.4'
backend_port2 = 5432
backend_weight2 = 1
backend_data_directory2 = '/var/lib/postgresql/16/main'
backend_application_name0 = 'postgresql-slave-02'

load_balance_mode = on
master_slave_mode = on 
master_slave_sub_mode = 'stream'
```

Where: 

- `listen_addresses`: The IP address that PGpool-II will listen for incoming connections.
- `port`: The port that PGpool-II will listen for incoming connections.
- `backend_hostname0`: The IP address of the PostgreSQL master server.
- `backend_port0`: The port of the PostgreSQL master server.
- `backend_weight0`: The weight of the PostgreSQL master server.
- `backend_data_directory0`: The path to the data directory of the PostgreSQL master server.
- `backend_hostname1`: The IP address of the PostgreSQL slave 1 server.
- `backend_port1`: The port of the PostgreSQL slave 1 server.
- `backend_weight1`: The weight of the PostgreSQL slave 1 server.
- `backend_data_directory1`: The path to the data directory of the PostgreSQL slave 1 server.
- `backend_hostname2`: The IP address of the PostgreSQL slave 2 server.
- `backend_port2`: The port of the PostgreSQL slave 2 server.
- `backend_weight2`: The weight of the PostgreSQL slave 2 server.
- `backend_data_directory2`: The path to the data directory of the PostgreSQL slave 2 server.
- `load_balance_mode`: Load balancing mode.
- `master_slave_mode`: Master/slave mode.
- `master_slave_sub_mode`: Data replication mode.

#### Step 3: Configure Connection Management

Next, we will configure connection management by editing the configuration file `/etc/pgpool2/pool_hba.conf`:

```bash
sudo nano /etc/pgpool2/pool_hba.conf
```

Add the following configuration to the file:

```bash
host    all         all         0.0.0.0/0          trust
```

#### Step 4: Start PGpool-II

Finally, restart the PGpool-II service to apply the changes:

```bash
sudo systemctl restart pgpool2
```

To ensure PGpool-II automatically starts when the system boots, you can enable the PGpool-II service with the following command:

```bash
sudo systemctl enable pgpool2
```

#### Step 5: Test the connection, we use pgadmin to connect to pgpool2

Login information as follows:

- `Host`: 192.168.56.5
- `Port`: 5432
- `Username`: postgres
- `Password`: at the PostgreSQL Replication installation step

{{< figure src="./images/pgpool-pgadmin.jpg" >}}

So we have successfully installed and configured PGpool-II.

### Conclusion
In this guide, we went through the steps to install and configure PGpool-II on the Ubuntu Linux operating system. By performing these steps, you can optimize and expand the capabilities of your PostgreSQL database, helping you to maximize the performance and high availability of your database.