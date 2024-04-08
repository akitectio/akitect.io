---
categories:
  - database
date: 2024-02-22T08:00:00+08:00
draft: false
featuredImage: /labs/postgresql/postgresql-16-replication.jpeg
images:
  - /labs/postgresql/postgresql-16-replication.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - replication 
  - ubuntu
title: Lesson 4 - PostgreSQL 16 Replication Setup
url: /postgresql-16-replication-setup-detailed-step-by-step-guide
description: PostgreSQL has a layer replication feature, allowing data to be copied from one DB to another, creating multiple copies of data. This feature helps distribute data, ensure the latest data, and support replacing the primary server.
weight: 4
---

## What is PostgreSQL Replication

PostgreSQL is a flexible open-source relational database management system, known for its performance and robustness. Setting up replication between a primary (master) and a secondary (slave) server is an important step in ensuring high availability and data redundancy in a PostgreSQL environment. In this guide, we will walk you through the process of installing PostgreSQL on the secondary server and configuring replication to replicate data from the primary server.

## Installation Architecture

{{< figure src="./images/postgresql-16-replication.jpeg" >}}

## Environment Preparation

Before we start, we need to prepare 3 servers

| IP           | Hostname             | vCPU   | RAM | DISK | OS           |
| ------------ | -----------------    | ------ | --- | ---- | ------------ |
| 192.168.50.11 | pg-master    | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.12 | pg-slave-01  | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.13 | pg-slave-02  | 2 core | 4G  | 60G  | Ubuntu 22.04 |

## Installing PostgreSQL 16

[Install and secure PostgreSQL 16 on Ubuntu 22.04](/install-and-secure-postgresql-16-on-ubuntu-2204) on 3 servers `postgresql-master`, `postgresql-slave-01`, and `postgresql-slave-02`.

Check the PostgreSQL version

```shell
psql --version
```
{{< figure src="./images/check-version-postgresql.png" >}}

## Configuring PostgreSQL 16 Replication

### Step 1: Configure PostgreSQL Master

First, we will configure the master server to allow data replication from the master server to the slave server. To start, open the PostgreSQL configuration file on the master server with your favorite text editor. In this guide, we will use the nano editor.

```shell
sudo nano /etc/postgresql/16/main/postgresql.conf
```

Find and edit the following lines:

```shell
listen_addresses = '*'
wal_level = replica
max_wal_senders = 10
max_replication_slots = 10

max_connections = 1000
```

- `wal_level` is the level of the WAL (Write-Ahead Log) records that the master server will generate. This level needs to be set to `replica` to allow the master server to send WAL records to the slave server.

- `max_replication_slots` is the maximum number of replication slots that the master server can create. Replication slots are a mechanism to keep copies of WAL records for the slave server. This level needs to be set to `10` to allow the master server to create up to 10 replication slots.

- `max_connections` is the maximum number of client connections that the master server can accept. This level needs to be set to `1000` to allow the master server to accept up to 1000 client connections.

After editing, save and exit the configuration file.

Next, we will configure the `pg_hba.conf` file to allow the slave server to connect to the master server and receive replication data. Open the `pg_hba.conf` file with your favorite text editor on the master server.

```shell
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

Add the following lines to the end of the file:

```shell
host    replication    replicator              192.168.56.12/32         md5
host    replication    replicator              192.168.56.13/32         md5
```

{{< figure src="./images/pg_hba.conf.jpg" >}}

After editing, save and exit the configuration file.

Next, we will create a user with the right to replicate data from the `postgresql-master` server to the `postgresql-slave` server. First, log in to the master server and open the `psql` console.

```shell
sudo -u postgres psql
CREATE USER replicator

 WITH

 REPLICATION LOGIN ENCRYPTED PASSWORD '71e1b930e5d53ea4c8f02ccfd255d7cc';
```

`71e1b930e5d53ea4c8f02ccfd255d7cc` is the strong password you want to use for the replicator user. Here I set it to `71e1b930e5d53ea4c8f02ccfd255d7cc`, please use md5 to create a stronger password.

After creating, exit the `psql` console.

Restart the PostgreSQL service

```shell
sudo systemctl restart postgresql
```
### Step 2: Configure PostgreSQL Slave

Next, we will configure the slave server to receive data from the master server. Open the PostgreSQL configuration file on the slave server with your favorite text editor.

```shell
sudo nano /etc/postgresql/16/main/postgresql.conf
```

Find and edit the following lines:

```shell
listen_addresses = '*'
hot_standby = on
```

After editing, save and exit the configuration file.

#### 2.1 : Stop PostgreSQL service

```shell
sudo systemctl stop postgresql
```
#### 2.2 : Delete current PostgreSQL data

```shell
sudo rm -rf /var/lib/postgresql/16/main
```
#### 2.2 : Backup data from the master server to the slave server

On `postgresql-slave-01`, run the following command:

```shell
export PGPASSWORD=71e1b930e5d53ea4c8f02ccfd255d7cc

pg_basebackup -h 192.168.56.11 -U replicator -R -X stream -C -S replica_1 -v -R -W -D/var/lib/postgresql/16/main -w

# Set permission
chown -R postgres:postgres /var/lib/postgresql/16/main
```

`postgresql-slave-01` enter the following command:

```shell
export PGPASSWORD=71e1b930e5d53ea4c8f02ccfd255d7cc

pg_basebackup -h 192.168.56.11 -U replicator -R -X stream -C -S replica_2 -v -R -W -D/var/lib/postgresql/16/main -w

# Set permission
chown -R postgres:postgres /var/lib/postgresql/16/main
```

{{< figure src="./images/pg_basebackup.jpg" >}}

#### 2.3 : Configure recovery.conf file on `postgresql-slave-01` and `postgresql-slave-02`

```shell
sudo nano /var/lib/postgresql/16/main/recovery.conf
```

Add the following content to the `recovery.conf` file:

```shell
standby_mode = 'on'
primary_conninfo = 'host=192.168.56.11 port=5432 user=replicator password=71e1b930e5d53ea4c8f02ccfd255d7cc'
```

After editing, save and exit the configuration file.

`71e1b930e5d53ea4c8f02ccfd255d7cc` is the password you created in step 1.
`192.168.56.11` is the IP address of the `postgresql-master` machine.

#### 2.4 : Start PostgreSQL service

```shell
sudo systemctl start postgresql
```

## Check PostgreSQL Replication

### Step 1: Check the status of PostgreSQL Master

First, log in to the `postgresql-master` server and open the `psql` console.

```shell
sudo -u postgres psql
```

Check the status of the `postgresql-master` server:

```shell
SELECT client_addr, state
FROM pg_stat_replication;
```

{{< figure src="./images/pg_stat_replication.jpg" >}}

Thus, we have successfully configured 2 `postgresql-slave-01` and `postgresql-slave-02` machines to receive data from the `postgresql-master` machine.

### Step 2: Check the status of PostgreSQL Slave

Log in to the `postgresql-slave` server and open the `psql` console.

```shell
sudo -u postgres psql
```

Check the status of the `postgresql-slave` server:

```shell
SELECT status, receive_start_lsn, sender_host  FROM pg_stat_wal_receiver;
```

{{< figure src="./images/pg_stat_wal_receiver.jpg" >}}

## Check data replication setup

To check whether data replication is working or not, add some data to the main server and check if the data is copied to the secondary server.

### Step 1: Add data to the `postgresql-master` server

```shell
sudo -u postgres psql
```

```shell
CREATE DATABASE test;

\c test

CREATE TABLE users (id SERIAL PRIMARY KEY, name VARCHAR(100));

INSERT INTO users (name) VALUES ('DUY TRAN - AKITECT.IO');
```

{{< figure src="./images/check-replication-postgresql-master.jpg" >}}

### Step 2: Check data on the `postgresql-slave` server

```shell
sudo -u postgres psql
```

```shell
\c test

SELECT * FROM users;
```

{{< figure src="./images/check-replication-slave.jpg" >}}

Thus, we have successfully configured PostgreSQL 16 Replication between the `postgresql-master` server and `postgresql-slave-01`, `postgresql-slave-02`.

## Conclusion

In this guide, we have shown you how to install and configure PostgreSQL 16 Replication between the main server and the secondary server. By configuring data replication, you can clone data from the main server to the secondary server, helping to distribute data, ensure the latest data, and support replacing the main server. We hope this guide has helped you understand how to set up PostgreSQL 16 Replication. If you have any questions, please leave a comment below. We are happy to help you. Thank you for reading!