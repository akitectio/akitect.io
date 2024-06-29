---
categories: [database]
date: 2023-10-26T00:00:00.000Z
description: Install and secure PostgreSQL 14 on Ubuntu 22.04
draft: false
featuredImage: /series/postgresql.png
images: [/series/postgresql.png]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [Database, PostgreSQL, Ubuntu, PostgreSQL 14]
title: Install and secure PostgreSQL 14 on Ubuntu 22.04
---

## Step 1: Install PostgreSQL 14 on Ubuntu 22.04.2 LTS

1.  Open the terminal and update the package list:

    ```bash
    sudo apt update
    ```

    {{< figure src="./images/01.png" >}}

2.  Install PostgreSQL 14 with the following command:

    ```bash
    sudo apt install postgresql-14 -y
    ```

    {{< figure src="./images/02.png" >}}

3.  PostgreSQL will automatically start after installation. You can check its status with the command:

    ```bash
    sudo systemctl status postgresql
    ```

    {{< figure src="./images/03.png" >}}

## Step 2: Secure PostgreSQL

1.  Start by setting a password for the "postgres" user account (default):

    ```bash
    sudo passwd postgres
    ```

    {{< figure src="./images/04.png" >}}

2.  Switch to the "postgres" account and open the PostgreSQL shell:

    ```bash
    sudo -i -u postgres
    psql
    ```

    {{< figure src="./images/05.png" >}}

3.  Set a password for the "postgres" account:

    ```sql
    \password postgres
    ```

    {{< figure src="./images/06.png" >}}

    Enter the new password and confirm.

4.  Exit the PostgreSQL shell:

    ```sql
    \q
    exit
    ```

    {{< figure src="./images/07.png" >}}

## Step 3: Create Database and User

1.  Log in to PostgreSQL with the "postgres" account:

    ```bash
    sudo -i -u postgres
    ```

2.  Open the PostgreSQL shell:

    ```bash
    psql
    ```

3.  Create a database (example: "mydatabase"):

    ```sql
    CREATE DATABASE mydatabase;
    ```

4.  Create a user (example: "myuser") and set a password:

    ```sql
    CREATE USER myuser WITH PASSWORD 'mypassword';
    ```

5.  Assign privileges to the user to access and manage the database:

    ```sql
    GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;
    ```

6.  End the session and exit:

    ```sql
    \q
    exit
    ```

## Step 4: Check the connection

```bash
psql -h localhost -U myuser -d mydatabase
```

{{< figure src="./images/08.png" >}}

## Step 5: Configure security

1.  Open the configuration file:

    ```bash
    sudo nano /etc/postgresql/14/main/pg_hba.conf
    ```

    {{< figure src="./images/09.png" >}}

2.  Change the authentication method of all connections to "md5":

    ```bash
    local   all             all                                     md5
    host    all             all
    ```

    {{< figure src="./images/10.png" >}}

3.  Restart PostgreSQL:

    ```bash
    sudo systemctl restart postgresql
    ```

Thank you for reading this article.
