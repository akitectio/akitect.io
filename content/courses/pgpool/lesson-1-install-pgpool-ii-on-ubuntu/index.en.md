---
categories: [database]
date: 2024-02-24T00:00:00.000Z
draft: false
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images: [/labs/postgresql/postgresql-pgpool.jpeg]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [database, postgresql, ubuntu, pgpool]
title: Lesson 1 -  Install Pgpool-II on Ubuntu
description: PGpool-II is a unique middleware solution, specially designed to optimize and scale the capabilities of the PostgreSQL database management system. It brings many benefits such as optimizing connections, load balancing, and performing data replication, making PGpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure PGpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.
weight: 1
---

# What is Pgpool-II

Pgpool-II is a unique middleware solution, specially designed to optimize and scale the capabilities of the PostgreSQL database management system. It offers numerous benefits such as connection optimization, load balancing, and data replication, making Pgpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure Pgpool-II on the Ubuntu Linux operating system, helping you maximize the performance and high availability of your database.

# Installation Architecture

{{< figure src="./images/postgresql-pgpool.jpeg" >}}

Before we start, we need to prepare 4 servers

| IP            | Hostname    | vCPU   | RAM | DISK | OS           |
| ------------- | ----------- | ------ | --- | ---- | ------------ |
| 192.168.50.10 | pgpool2     | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.11 | pg-master   | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.12 | pg-slave-01 | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.13 | pg-slave-02 | 2 core | 4G  | 60G  | Ubuntu 22.04 |

### Install PostgreSQL Replication

[Install PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc) on 3 servers `postgresql-master` and `postgresql-slave-01`, `postgresql-slave-02`.

### Install PGpool-II 4.5.0

#### Step 1: Install PGpool-II 4.5.0

##### Install make and gcc

\*\*\* GNU make version 3.80 or newer is required; other make programs or older GNU make versions will not work. (GNU make is sometimes installed under the name gmake.) To check GNU, enter:

```bash
sudo apt update
sudo apt install -y make gcc libpq-dev
```

After installation, check the version of `make`

```bash
make --version
```

{{< figure src="./images/make-version.jpg" >}}

The current version of `make` is `4.3`

```bash
gcc --version
```

{{< figure src="./images/gcc-version.jpg" >}}

The current version of `gcc` is `11.4.0`

##### Download PGpool-II installation package

```bash
wget https://www.pgpool.net/mediawiki/download.php?f=pgpool-II-4.5.0.tar.gz -O pgpool-II-4.5.0.tar.gz
```

##### Extract and install

```bash
# extract
tar -xvf pgpool-II-4.5.0.tar.gz
cd pgpool-II-4.5.0
```

Next, we install [memcached](/cai-dat-va-bao-mat-memcached-tren-ubuntu-22-04) and libmemcached-dev and libpq-dev

```bash
sudo apt install -y libmemcached-dev libpq-dev
```

Then, we will configure and install PGpool-II by performing the following steps:

```bash
./configure --prefix=/home/pgpool2 --with-memcached=/usr/sbin/memcached 
make && sudo make install
```

You can customize the build and installation process by providing one or more of the following command line options to configure:

| Option                   | Description                                                    | Default                 |
| ------------------------ | -------------------------------------------------------------- | ----------------------- |
| `--prefix`               | PGpool-II installation path                                    | `/usr/local`            |
| `--with-pgsql`           | Directory where PostgreSQL client libraries are installed      | Provided by `pg_config` |
| `--with-openssl`         | Support for OpenSSL (AES256 password encryption)               | Off                     |
| `--enable-sequence-lock` | Lock rows in sequence table (compatible with PGpool-II 3.0)    | Off                     |
| `--enable-table-lock`    | Lock target insert table (compatible with PGpool-II 2.2 & 2.3) | Off                     |
| `--with-memcached=path`  | Use memcached for memory query cache                           | Not used                |
| `--with-pam`             | Support for PAM authentication                                 | Off                     |

After configuring, we proceed to create ln -la to create a link to the `/usr/sbin` directory 

```bash
sudo ln -s /home/pgpool2/bin/* /usr/sbin
```

Continue to check the version of `pgpool` after successful installation with the following command:

```bash
pgpool --version
```

{{< figure src="./images/pgpool-version.jpg" >}}

#### Step 2: Configure PGpool-II

Create a configuration directory for PGpool-II:

```bash
sudo mkdir /etc/pgpool2
```

Copy from sample config :

```bash
sudo cp /home/pgpool2/etc/pgpool.conf.sample /etc/pgpool2/pgpool.conf 
sudo cp /home/pgpool2/etc/pool_hba.conf.sample /etc/pgpool2/pool_hba.conf 
sudo cp /home/pgpool2/etc/pcp.conf.sample /etc/pgpool2/pcp.conf
```

#### Step 3: Configure Connection Management

Next, we will configure connection management by editing the configuration file `/etc/pgpool2/pool_hba.conf`:

```bash
sudo nano /etc/pgpool2/pool_hba.conf
```

Add the following configuration to the file:

```bash
host    all         all         0.0.0.0/0          trust
```

{{< figure src="./images/pgpool-pool-hba-config.jpg" >}}

#### Step 4: Start PGpool-II

Create a directory to store the pid file:

```bash
sudo mkdir /var/run/pgpool
```

Finally, restart the PGpool-II service to apply the changes:

```bash
sudo /usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
```

{{< figure src="./images/pgpool-start.jpg" >}}

#### Step 5: Configure pcp.conf

**Pgpool-II provides an interface for administrators to perform management operations, such as viewing the status of Pgpool-II or shutting down Pgpool-II processes remotely. `pcp.conf` is the file that contains the user/password used for authentication for this interface. All operating modes require the `pcp.conf` file to be set up.**

Create the `pcp.conf` file:

```bash
password_hash=$(pg_md5 Ehi@123)
echo "pgpool:$password_hash" >> /etc/pgpool2/pcp.conf
```

Where `Ehi@123` is the password for the `pgpool` account, you can change the password as desired.

Then, add access permissions to the `pcp.conf` file:

```bash
sudo chown pgpool:pgpool /etc/pgpool2/pcp.conf
sudo chmod 0600 /etc/pgpool2/pcp.conf
```

#### Step 6: Create PGpool-II Service

Create a systemd service file `pgpool2.service` on the `pgpool2` server:

```bash
sudo nano /etc/systemd/system/pgpool2.service
```

Add the following content to the `pgpool2.service` file:

```bash
Description=pgpool-II
Documentation=man:pgpool(8)
Wants=postgresql.service
After=network.target



[Service]
User=root
ExecStart=/usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
ExecReload=/bin/kill -HUP $MAINPID
KillSignal=SIGINT
StandardOutput=syslog
SyslogFacility=local0

[Install]
WantedBy=multi-user.target
```

> `/usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf -m smart` trong đó : 

-   `-n` : Không chạy dưới dạng daemon
-   `-f` : Đường dẫn đến tệp cấu hình `pgpool.conf`
-   `-F` : Đường dẫn đến tệp cấu hình quản lý `pcp.conf`
-   `-m` : Chế độ hoạt động của Pgpool-II. Có 3 chế độ hoạt động:
    -   `fast` : Chế độ hoạt động nhanh
    -   `smart` : Chế độ hoạt động thông minh, mặc định

Automatically enable at system startup:

```bash
sudo systemctl enable pgpool2
```

Restart the PGpool-II service:

```bash
sudo systemctl start pgpool2
```

Thus, we have installed and configured PGpool-II on the Ubuntu Linux operating system. By performing these steps, you can optimize and expand the capabilities of your PostgreSQL database, helping you maximize the performance and high availability of your database.
