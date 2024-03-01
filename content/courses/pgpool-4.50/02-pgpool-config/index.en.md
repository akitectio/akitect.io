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
title: Lesson 2 - Configuring Pgpool-II  
url: /lesson-2-pgpool-ii-configuration
description: PGpool-II is a unique middleware solution, specially designed to optimize and scale the capabilities of the PostgreSQL database management system. It brings many benefits such as optimizing connections, load balancing, and performing data replication, making PGpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will go through the steps to install and configure PGpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.
weight: 2
---

Configuring PGpool-II is an important step in the process of deploying a PostgreSQL database cluster. In this guide, we will go through the steps to configure PGpool-II on the Ubuntu Linux operating system, helping you to maximize the performance and high availability of your database.

# Installation Architecture

{{< figure src="./images/postgresql-pgpool.jpeg" >}}

Before we start, we need to prepare 4 servers

| IP           | Hostname             | vCPU   | RAM | DISK | OS           |
| ------------ | -------------------- | ------ | --- | ---- | ------------ |
| 192.168.56.5 | pgpool2              | 2 core | 4G  | 50G  | Ubuntu 22.04 |
| 192.168.56.2 | postgresql-master    | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | postgresql-slave-01  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | postgresql-slave-02  | 4 core | 8G  | 50G  | Ubuntu 22.04 |

### Step 1: PGpool-II User Account

First, create a user account `pgpool` on all servers `pgpool2`, `postgresql-master`, `postgresql-slave-01`, `postgresql-slave-02`:

```bash
sudo useradd -m -s /bin/bash pgpool
sudo passwd pgpool
```

Then, add the user account `pgpool` to the `sudo` group:

```bash
sudo usermod -aG sudo pgpool
```

#### Step 2: Configure `pcp.conf`

First, create a configuration file `pcp.conf` on the `pgpool2` server:

```bash
password_hash=$(pg_md5 Ehi@123)
echo "pgpool:$password_hash" >> /etc/pgpool2/pcp.conf
```

Where `akitect@123` is the password of the `pgpool` account, you can change the password as desired.

Then, add access rights to the `pcp.conf` file:

```bash
sudo chown pgpool:pgpool /etc/pgpool2/pcp.conf
sudo chmod 0600 /etc/pgpool2/pcp.conf
```

#### Step 3: Configure Load Balancing and Replication

You need to configure Replication first, if you have not configured it, refer to [Installing PostgreSQL 16 Replication](/thiet-lap-postgresql-replication-huong-chi-tiet-tung-buoc)

Next, configure the `pgpool.conf` file on the `pgpool2` server:

```bash
sudo chown pgpool:pgpool /etc/pgpool2/pgpool.conf
sudo chmod 0600 /etc/pgpool2/pgpool.conf
```

Then, open the `pgpool.conf` file with your favorite text editor:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```

Add the following configuration to the `pgpool.conf` file:

```bash
listen_addresses = '*' 
port = 9999 

backend_hostname0 = '192.168.56.2'
backend_port0 = 5432
backend_weight0 = 0
backend_data_directory0 = '/home/ubuntu/postgresql/master'
backend_flag0 = 'ALLOW_TO_FAILOVER'
backend_application_name0 = 'postgresql-master'

backend_hostname1 = '192.168.56.3'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/home/ubuntu/postgresql/slave-01'
backend_flag1 = 'ALLOW_TO_FAILOVER'
backend_application_name1 = 'postgresql-slave-01'

backend_hostname2 = '192.168.56.4'
backend_port2 = 5432
backend_weight2 = 2
backend_data_directory2 = '/home/ubuntu/postgresql/slave-02'
backend_flag2 = 'ALLOW_TO_FAILOVER'
backend_application_name2 = 'postgresql-slave-02'

log_statement = on
log_per_node_statement = on

sr_check_user = 'replicator'
health_check_user = 'replicator'
health_check_period = 10
pid_file_name = 'pgpool.pid'
```

Where `listen_addresses` is the IP address that PGpool-II will listen to, `port` is the port that PGpool-II will listen to.

- `backend_hostname0`, `backend_port0`, `backend_weight0`, `backend_data_directory0`, `backend_flag0`, `backend_application_name0` are the connection information to the `postgresql-master` server.

- `backend_hostname1`, `backend_port1`, `backend_weight1`, `backend_data_directory1`, `backend_flag1`, `backend_application_name1` are the connection information to the `postgresql-slave-01` server.

- `backend_hostname2`, `backend_port2`, `backend_weight2`, `backend_data_directory2`, `backend_flag2`, `backend_application_name2` are the connection information to the `postgresql-slave-02` server.

- `log_statement` and `log_per_node_statement` are query log configurations.

- `sr_check_user` and `health_check_user` are the names of the users checking the health of the database server. `replicator` is the user that we created in the step [Installing PostgreSQL 16 Replication](/postgresql-16-replication-setup-detailed-step-by-step-guide/)

#### Step 4: Run PGpool-II Configuration Check

Finally, run the PGpool-II configuration check:

```bash
sudo /usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
```

{{< figure src="./images/run-pgpool-configuration.jpg" >}}

After configuring, you can check the status of PGpool-II by accessing it via pg4admin: 

{{< figure src="./images/pg4admin-pgpool.jpg" >}}

- `ip_address` is the IP address (192.168.56.5) of the `pgpool2` server.
- `username`  default is `postgresql`.
- `password` is the password of the `postgresql` account.

#### Step 5: Create Test Data 

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

{{< figure src="./images/pg4admin-pgpool-student.jpg" >}}

As the image shows, the `INSERT` has been performed on the `postgresql-master` server.

Continue we Query with the `SELECT` command on PG4Admin:

```bash
SELECT * FROM student;
```

{{< figure src="./images/pg4admin-pgpool-student-select.jpg" >}}

Then we go back to see the log of the `pgpool2` server 

{{< figure src="./images/pgpool-log.jpg" >}}

As the image shows, `SELECT` has been performed on the `postgresql-slave-01` server through the `pgpool2` server.

#### Step 6: Configure PGpool for High Availability PostgreSQL

##### 6.1: Configure `pgpool.conf` on `pgpool2` server

Copy `failover.sh` on `pgpool2` server:

```bash
sudo cp /home/pgpool2/etc/failover.sh.sample /etc/pgpool2/failover.sh
```

Then, add execute permissions to the `failover.sh` file:

```bash
sudo chmod +x /etc/pgpool2/failover.sh
```

Then open the `pgpool.conf` file:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```


Add the following configuration to the `pgpool.conf` file:

```bash
failover_command = '/etc/pgpool2/failover.sh %d  /home/ubuntu/postgresql/master/down.trg'
```

##### 6.1: Configure `postgresql.conf` on `postgresql-master` server

Add the following configuration to the `postgresql.conf` file:

```bash
synchonous_commit = remote_apply
```
`synchonous_commit` includes the following values: 

- `on`: Ensures that each transaction has been confirmed before it ends.
<!-- 
```mermaid
sequenceDiagram
    participant A as "Client"
    participant B as "Primary"
    participant C as "Standby"

    A->B: "Start transaction"
    B->B: "Write to WAL"
    B->C: "Write to WAL"
    C->C: "Write to cache"
    C->B: "Confirm write"
    B->A: "Confirm"
``` -->

- `remote_write`: Ensures that each transaction has been written to the standby server's cache before it ends.
<!-- 
```mermaid
sequenceDiagram
    participant A as "Client"
    participant B as "Primary"
    participant C as "Standby"

    A->B: "Start transaction"
    B->B: "Write to WAL"
    B->C: "Write to WAL"
    C->C: "Write to cache"
    C->B: "Confirm write"
    B->A: "Confirm"
``` -->

- `remote_apply`: Ensures that each transaction has been applied on the standby server before it ends.

<!-- ```mermaid
sequenceDiagram
    participant A as "Client"
    participant B as "Primary"
    participant C as "Standby"

    A->B: "Start transaction"
    B->B: "Write to WAL"
    B->C: "Write to WAL"
    C->C: "Apply to database"
    C->B: "Confirm apply"
    B->A: "Confirm"
```
 -->


##### 6.2:  Configure `postgresql.conf` on `postgresql-slave-01` server

Add the following configuration to the `postgresql.conf` file:

```bash
promote_trigger_file = '/home/ubuntu/postgresql/master/down.trg'
```

Then, restart the PostgreSQL service on the `postgresql-slave-01` server:

```bash
sudo systemctl restart postgresql
```

##### 6.3:  Configure `postgresql.conf` on `postgresql-slave-02` server

Add the following configuration to the `postgresql.conf` file:

```bash
promote_trigger_file = '/home/ubuntu/postgresql/master/down.trg'
```

Then, restart the PostgreSQL service on the `postgresql-slave-02` server:

```bash
sudo systemctl restart postgresql
```

#### Step 7: Create PGpool-II Service

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
User=postgres
ExecStart=/usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
ExecReload=/bin/kill -HUP $MAINPID
KillSignal=SIGINT
StandardOutput=syslog
SyslogFacility=local0

[Install]
WantedBy=multi-user.target
```

Then, restart the PGpool-II service:

```bash
sudo systemctl daemon-reload
sudo systemctl start pgpool2
sudo systemctl enable pgpool2
```

Thus, we have finished configuring PGpool-II for PostgreSQL with High Availability.