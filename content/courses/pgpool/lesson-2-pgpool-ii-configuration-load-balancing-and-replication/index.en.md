---
categories: [database]
date: 2024-02-24T00:00:00.000Z
draft: false
featuredImage: /labs/postgresql/postgresql-pgpool.jpeg
images: [/labs/postgresql/postgresql-pgpool.jpeg]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [database, postgresql, ubuntu, pgpool]
title: Lesson 2 - Load Balancing and Replication
description: PGpool-II is a unique middleware solution specifically designed to optimize and enhance the capabilities of the PostgreSQL database management system. It offers various benefits such as connection optimization, load distribution, and data replication, making PGpool-II an indispensable tool in managing PostgreSQL deployments. In this detailed guide, we will walk through the steps to install and configure PGpool-II on an Ubuntu Linux operating system, helping you maximize the performance and high availability of your database.
weight: 2
---

Configuring PGpool-II is a crucial step in deploying a PostgreSQL database cluster. In this guide, we will walk through the steps to configure PGpool-II on an Ubuntu Linux operating system, helping you maximize the performance and high availability of your database.

# Installation Architecture

{{< figure src="./images/postgresql-pgpool.jpeg" >}}

Before we begin, we need to prepare 4 servers:

| IP            | Hostname    | vCPU   | RAM | DISK | OS           |
| ------------- | ----------- | ------ | --- | ---- | ------------ |
| 192.168.50.10 | pgpool2     | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.11 | pg-master   | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.12 | pg-slave-01 | 2 core | 4G  | 60G  | Ubuntu 22.04 |
| 192.168.50.13 | pg-slave-02 | 2 core | 4G  | 60G  | Ubuntu 22.04 |

#### Step 3: Configure Load Balancing and Replication

You need to configure Replication first, if you haven't configured it, refer to [Setting Up PostgreSQL 16 Replication](/setup-postgresql-replication-step-by-step-guide).

Add the user `replicator`:

Create the file `pcp.conf`:

```bash
password_hash=$(pg_md5 PassKhongChilaPasss) && echo "postgre:$password_hash" >> /etc/pgpool2/pcp.conf
```

Next, configure the file `pgpool.conf` on the `pgpool2` server:

```bash
sudo chown pgpool:pgpool /etc/pgpool2/pgpool.conf
sudo chmod 0600 /etc/pgpool2/pgpool.conf
```

Then, open the file `pgpool.conf` with a text editor `nano`:

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

sr_check_user = 'replicator'
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

In this configuration:

-   `listen_addresses` is the IP address PGpool-II will listen on, `port` is the port PGpool-II will listen on.
-   `backend_hostname0`, `backend_port0`, `backend_weight0`, `backend_data_directory0`, `backend_flag0`, `backend_application_name0` are the connection details to the `postgresql-master` server.
-   `backend_hostname1`, `backend_port1`, `backend_weight1`, `backend_data_directory1`, `backend_flag1`, `backend_application_name1` are the connection details to the `postgresql-slave-01` server.
-   `backend_hostname2`, `backend_port2`, `backend_weight2`, `backend_data_directory2`, `backend_flag2`, `backend_application_name2` are the connection details to the `postgresql-slave-02` server.
-   `log_statement` and `log_per_node_statement` are the log query configurations.
-   `sr_check_user` and `health_check_user` are the usernames for database server health checks. `replicator` is the user we created in the step [Setting Up PostgreSQL 16 Replication](/setup-postgresql-replication-step-by-step-guide).
-   `backend_flag` is the flag that allows `FAILOVER` when the `postgresql-master` server fails, `ALLOW_TO_FAILOVER` is the flag that allows `FAILOVER` when the `postgresql-slave-01` and `postgresql-slave-02` servers fail. Other flags:
    -   `DISALLOW_TO_FAILOVER`: does not allow `FAILOVER`
    -   `ALWAYS_MASTER`: always the `postgresql-master` server
-   `process_management_mode` is the process management mode, `static` is the static mode, `dynamic` is the dynamic mode.
    -   `static`: The number of processes is statically managed and does not change.
    -   `dynamic`: The number of processes is dynamically managed depending on the system load.
-   `process_management_strategy` is the process management strategy, `lazy` is the lazy mode, `smart` is the smart mode.
    -   `lazy`: Only create processes when needed.
    -   `smart`: Create processes in advance to avoid delays when needed.
-   `num_init_children` is the initial number of processes, if below it will create more processes.
-   `min_spare_children` is the minimum number of processes, if below it will create more processes.
-   `max_spare_children` is the maximum number of processes, if exceeded they will be killed.
-   `max_pool` is the maximum number of connections, if exceeded connections will be rejected.

#### Step 4: Run PGpool-II Configuration Check

Finally, run the PGpool-II configuration check:

```bash
sudo /usr/sbin/pgpool -n -f /etc/pgpool2/pgpool.conf -F /etc/pgpool2/pcp.conf
```

{{< figure src="./images/run-pgpool-configuration.jpg" >}}

After configuration, you can check the status of PGpool-II by accessing it through pg4admin:

{{< figure src="./images/pg4admin-pgpool.jpg" >}}

-   `ip_address` is the IP address (192.168.56.5) of the `pgpool2` server.
-   `username` is the default username `postgresql`.
-   `password` is the password for the `postgresql` account.

#### Step 5: Create Test Data

Here we will create a `student` database and `10000` test data:

```bash
# Create the student database
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

As shown in the image, the `INSERT` statement has been executed on the `postgresql-master` server.

Next, we query with the `SELECT` statement on PG4Admin:

```bash
SELECT * FROM student;
```

{{< figure src="./images/pg4admin-pgpool-student-select.jpg" >}}

Then we check the log of the `pgpool2` server:

{{< figure src="./images/pgpool-log.jpg" >}}

As shown in the image, the `SELECT` statement has been executed on the `postgresql-slave-01` server through the `pgpool2` server.

Thus, we have successfully configured PGpool-II for Load Balancing and Replication.
