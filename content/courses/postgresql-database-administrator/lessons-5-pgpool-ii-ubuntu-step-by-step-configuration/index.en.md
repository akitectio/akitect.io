---
categories: [database]
date: 2024-02-24T00:00:00.000Z
draft: true
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images: [/labs/postgresql/postgresql-pgpool.jpeg]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [database, postgresql, ubuntu, pgpool]
title: Lesson 5 -  Postgres Pgpool-II Ubuntu
description: PGpool-II is a unique middleware solution, specially designed to optimize and scale the capabilities of the PostgreSQL database management system. It brings many benefits such as optimizing connections, load balancing, and performing data replication, making PGpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure PGpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.
weight: 5
---

# What is Pgpool-II

PGpool-II is a unique middleware solution, specially designed to optimize and scale the capabilities of the PostgreSQL database management system. It brings many benefits such as optimizing connections, load balancing, and performing data replication, making PGpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure PGpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.

# Installation Architecture

{{< figure src="./images/postgresql-pgpool.jpeg" >}}

Before we start, we need to prepare 4 servers

| IP           | Hostname            | vCPU   | RAM | DISK | OS           |
| ------------ | ------------------- | ------ | --- | ---- | ------------ |
| 192.168.56.5 | pgpool2             | 2 core | 4G  | 50G  | Ubuntu 22.04 |
| 192.168.56.2 | postgresql-master   | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave-01 | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | postgresql-slave-02 | 4 core | 8G  | 50G  | Ubuntu 22.04 |

### Install PostgreSQL Replication

[Install PostgreSQL 16 Replication](/setting-up-postgresql-replication-step-by-step-guide) on 3 servers `postgresql-master` and `postgresql-slave-01`, `postgresql-slave-02`.

### Install PGpool-II 4.5.0

#### Step 1: Install PGpool-II 4.5.0

##### Install make and gcc

\*\*\* GNU make version 3.80 or newer is required; other make programs or older GNU make versions will not work. (GNU make is sometimes installed under the name gmake.) To check GNU, enter:

```bash
sudo apt update
sudo apt install -y make gcc libpq-dev
```

after installing check the version of `make`

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

##### Download the PGpool-II installation package

```bash
wget https://www.pgpool.net/mediawiki/download.php?f=pgpool-II-4.5.0.tar.gz -O pgpool-II-4.5.0.tar.gz
```

##### Extract and install

```bash
# extract
tar -xvf pgpool-II-4.5.0.tar.gz
cd pgpool-II-4.5.0
```

Next, we install [memcached](/install-and-secure-memcached-on-ubuntu-22-04) and libmemcached-dev and libpq-dev

```bash
sudo apt install -y libmemcached-dev libpq-dev
```

Then, we will configure and install PGpool-II by performing the following steps:

```bash
./configure --prefix=/home/pgpool2 --with-memcached=/usr/sbin/memcached 
make && sudo make install
```

You can customize the build and installation process by providing one or more of the following command line options to configure:

| Option                   | Description                                                        | Default                 |
| ------------------------ | ------------------------------------------------------------------ | ----------------------- |
| `--prefix`               | PGpool-II installation path                                        | `/usr/local`            |
| `--with-pgsql`           | Directory of installed PostgreSQL client libraries                 | Provided by `pg_config` |
| `--with-openssl`         | Support for OpenSSL (AES256 password encryption)                   | Off                     |
| `--enable-sequence-lock` | Lock rows in the sequence table (compatible with PGpool-II 3.0)    | Off                     |
| `--enable-table-lock`    | Lock the target insert table (compatible with PGpool-II 2.2 & 2.3) | Off                     |
| `--with-memcached=path`  | Use memcached for memory query cache                               | Not used                |
| `--with-pam`             | Support for PAM authentication                                     | Off                     |

After configuring, we proceed to create ln -la to create a link to the `/usr/sbin` directory 

```bash
sudo ln -s /home/pgpool2/bin/* /usr/sbin
```

Continue to check the version of `pgpool` after successful installation with the following command:

```bash
pgpool --version
```

{{< figure src="./images/pgpool-version.jpg" >}}

#### Step 2: Install `pgpool_recovery`

`PGpool-II` requires including the functions `pgpool_recovery`, `pgpool_remote_start` and `pgpool_switch_xlog` when using online recovery as explained later. Moreover, the pgpoolAdmin management tool also supports the ability to stop, restart or reload PostgreSQL versions on the screen using `pgpool_pgctl`. Installing these functions in the initial template1 is enough, there is no need to install them in all databases.

First we need to install `postgresql-server-dev-16`: 

##### 2.1: Add key and repo of PostgreSQL 16

```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
apt update
```

##### 2.2: Install `postgresql-server-dev-16`

```bash
sudo apt install -y postgresql-server-dev-16 -y
```

##### 2.3: Install `pgpool_recovery`

Next, we will install `pgpool_recovery` by performing the following steps:

```bash
# move to the pgpool-recovery directory
 cd pgpool-II-4.5.0/src/sql/pgpool-recovery

# install
make && sudo make install
```

After successful build we will have the file `pgpool_recovery.so` in the directory `pgpool-II-4.5.0/src/sql/pgpool-recovery`

```bash
ls -la
```

{{< figure src="./images/pgpool-recovery.jpg" >}}

##### 2.4: Configure `pgpool_recovery` replication from `pgpool2` server to `postgresql-master` server using `scp`:

Copy the `pgpool_recovery.so` and `pgpool_recovery.sql` files from the `pgpool2` server to the `postgresql-master` server using the `scp` command:

```bash
scp pgpool-recovery.so ubuntu@192.168.56.2:/home/ubuntu 
scp pgpool-recovery.sql ubuntu@192.168.56.2:/home/ubuntu 
scp pgpool_recovery.control ubuntu@192.168.56.2:/home/ubuntu 
```

Continue to ssh into the `postgresql-master` server:

```bash
ssh ubuntu@192.168.56.2
```

Copy the `pgpool_recovery.so` and `pgpool_recovery.sql` files to the `/usr/lib/postgresql/16/lib` and `/usr/share/postgresql/16/extension` directories:

```bash
sudo mv pgpool-recovery.so /usr/lib/postgresql/16/lib
sudo mv pgpool-recovery.sql /usr/share/postgresql/16/extension
sudo mv pgpool_recovery.control /usr/share/postgresql/16/extension
```

After installation, we will configure `pgpool_recovery` on the `postgresql-master` server by performing the following steps:

```bash
 sudo -u postgres psql -d template1 -f /usr/share/postgresql/16/extension/pgpool-recovery.sql 
```

{{< figure src="./images/pgpool-recovery-sql.jpg" >}}

Where `template1` is the sample database where we install `pgpool_recovery`

#### Step 3: Configure PGpool-II

Create a configuration directory for PGpool-II:

```bash
sudo mkdir /etc/pgpool2
```

Copy from the sample config :

```bash
sudo cp /home/pgpool2/etc/pgpool.conf.sample /etc/pgpool2/pgpool.conf 
sudo cp /home/pgpool2/etc/pool_hba.conf.sample /etc/pgpool2/pool_hba.conf 
sudo cp /home/pgpool2/etc/pcp.conf.sample /etc/pgpool2/pcp.conf
```

#### Step 3: Configure connection management

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

Finally, restart the PGpool-II service to apply the changes:

```bash
sudo /usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
```

{{< figure src="./images/pgpool-start.jpg" >}}

Thus, we have installed and configured PGpool-II on the Ubuntu Linux operating system. By performing these steps, you can optimize and scale the capabilities of your PostgreSQL database, helping you to maximize the performance and high availability of your database.
