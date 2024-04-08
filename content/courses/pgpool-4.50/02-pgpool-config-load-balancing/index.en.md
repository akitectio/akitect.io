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
title: Lesson 2 - Load Balancing and Replication
url: /lesson-2-pgpool-ii-configuration-load-balancing-and-replication
description: PGpool-II is a unique middleware solution, specially designed to optimize and scale the capabilities of the PostgreSQL database management system. It offers numerous benefits such as optimizing connections, load balancing, and performing data replication, making PGpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure PGpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.
weight: 2
---

Configuring PGpool-II is an important step in the deployment process of the PostgreSQL database cluster. In this guide, we will go through the steps to configure PGpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.


# Installation Architecture

{{< figure src="./images/postgresql-pgpool.jpeg" >}}


Before we start, we need to prepare 4 servers

| IP            | Hostname     | vCPU   | RAM | DISK | OS           |
| ------------  | ------------ | ------ | --- | ---- | ------------ |
| 192.168.50.10 | pgpool2      | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.11 | pg-master    | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.12 | pg-slave-01  | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.13 | pg-slave-02  | 2 core | 4G  | 60G  | Ubuntu 22.04 |



#### Step 3: Configure Load Balancing and Replication

You need to configure Replication first, if you haven't configured it yet, refer to [Setting up PostgreSQL 16 Replication](/setting-up-postgresql-replication-step-by-step-guide)

Add `replicator` user 

Create `pcp.conf` file:

```bash
password_hash=$(pg_md5 ThisIsNotAPassword) && echo "postgre:$password_hash" >> /etc/pgpool2/pcp.conf
```

Next, configure the `pgpool.conf` file on the `pgpool2` server:

```bash
sudo chown pgpool:pgpool /etc/pgpool2/pgpool.conf
sudo chmod 0600 /etc/pgpool2/pgpool.conf
```

Then, open the `pgpool.conf` file with the `nano` text editor:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```

Add the following configuration to the `pgpool.conf` file:

```bash
listen_addresses = '*' 
port = 9999 

backend_hostname0 = '192.168.56.11'
backend_port0 = 5432
backend_weight0 = 0
backend_data_directory0 = '/home/ubuntu/postgresql/master'
backend_flag0 = 'ALLOW_TO_FAILOVER'
backend_application_name0 = 'postgresql-master'

backend_hostname1 = '192.168.56.12'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/home/ubuntu/postgresql/slave-01'
backend_flag1 = 'ALLOW_TO_FAILOVER'
backend_application_name1 = 'postgresql-slave-01'

backend_hostname2 = '192.168.56.13'
backend_port2 = 5432
backend_weight2 = 2
backend_data_directory2 = '/home/ubuntu/postgresql/slave-02'
backend_flag2 = 'ALLOW_TO_FAILOVER'
backend_application_name2 = 'postgresql-slave-02'

log_statement = on
log_per_node_statement = on



sr

_check_user = 'replicator'
health_check_user = 'replicator'
health_check_period = 10


# Process management configuration 
process_management_mode = static
process_management_strategy = lazy
num_init_children = 320
min_spare_children = 1000
max_spare_children = 5000
max_pool = 1000

```
Here is the English translation of the selected text:

In which `listen_addresses` is the IP address that PGpool-II will listen to, `port` is the port that PGpool-II will listen to.

- `backend_hostname0`, `backend_port0`, `backend_weight0`, `backend_data_directory0`, `backend_flag0`, `backend_application_name0` are the connection information to the `postgresql-master` server.

- `backend_hostname1`, `backend_port1`, `backend_weight1`, `backend_data_directory1`, `backend_flag1`, `backend_application_name1` are the connection information to the `postgresql-slave-01` server.

- `backend_hostname2`, `backend_port2`, `backend_weight2`, `backend_data_directory2`, `backend_flag2`, `backend_application_name2` are the connection information to the `postgresql-slave-02` server.

- `log_statement` and `log_per_node_statement` are query log configuration.

- `sr_check_user` and `health_check_user` are the names of the users who check the health of the database server. `replicator` is the user that we created in the step [Setting up PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc)

- `backend_flag` is a flag that allows `FAILOVER` when the `postgresql-master` server fails, `ALLOW_TO_FAILOVER` is a flag that allows `FAILOVER` when the `postgresql-slave-01` and `postgresql-slave-02` servers fail. Other flags: 
 - `DISALLOW_TO_FAILOVER` : does not allow `FAILOVER`
 - `ALWAYS_MASTER` : always the `postgresql-master` server

- `process_management_mode` is the process management mode, `static` is the static mode, `dynamic` is the dynamic mode.
  - `static` : The number of processes will be managed statically, not changing.
  - `dynamic` : The number of processes will be managed dynamically, depending on the system load.
- `process_management_strategy` is the process management strategy, `lazy` is the lazy mode, `smart` is the smart mode.
  - `lazy` : Only create processes when necessary.
  - `smart` : Create processes in advance, avoiding delays when necessary.
- `num_init_children` is the initial number of processes, if below, more processes will be created.
- `min_spare_children` is the minimum number of processes, if below, more processes will be created.
- `max_spare_children` is the maximum number of processes, if exceeded, they will be killed.
- `max_pool` is the maximum number of connections, if exceeded, the connection will be refused.

#### Step 4: Run PGpool-II configuration check

Finally, run the PGpool-II configuration check:

```bash
sudo /usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
```

After configuring, you can check the status of PGpool-II by accessing it via pg4admin:

- `ip_address` is the IP address (192.168.56.5) of the `pgpool2` server.
- `username` is by default `postgresql`.
- `password` is the password of the `postgresql` account.

#### Step 5: Create test data

Here I will create a `student` database and `10000` test data:

```bash

# Create student database
CREATE TABLE student (
    student_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100),
    date_of_birth DATE,
    grade_level INT CHECK (grade_level BETWEEN 1 AND 12)
);



WITH generated_data AS (
       SELECT 
           substring(md5(random()::text) from 1 for 8) AS first_name,
           substring(md5(random()::text) from 1 for 8) AS last_name,
           concat(substring(md5(random()::text) from 1 for 6), '@example.com') AS email,
           '1990-01-01'::date + random() * (age(now(), '1990-01-01'::date)) AS date_of_birth,
           (random() * 11 + 1)::int AS grade_level
       FROM generate_series(1,1000000) -- Generate 100 rows
   )
   INSERT INTO student (first_name, last_name, email, date_of_birth, grade_level)
   SELECT * FROM generated_data; 
  
```

As the image shows, the `INSERT` data has been executed on the `postgresql-master` server.

Next, we Query with the `SELECT` command on PG4Admin:

```bash
SELECT * FROM student;
```

Then we go back to see the log of the `pgpool2` server

As the image shows, `SELECT` has been executed on the `postgresql-slave-01` server through the `pgpool2` server.

Thus, we have successfully configured PGpool-II to perform Load Balancing and Replication.