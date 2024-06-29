---
categories: [database]
date: 2023-03-01T00:00:00.000Z
description: Install and secure PostgreSQL 16 on Ubuntu 22.04
draft: false
featuredImage: /series/postgresql.png
images: [/install-and-secure-postgresql-15-on-ubuntu-2204/images/index.en.png, /series/postgresql.png]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [database, postgresql, ubuntu]
title: Install and secure PostgreSQL 16 on Ubuntu 22.04
---

# PostgreSQL 16 Package Repository

    sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

    wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null

To begin, let's get the latest version of the packages. We can achieve this by using the apt update command as shown below:

    sudo apt update

{{< figure src="./images/a0b112e0-aa5f-493a-ad99-693f029a2c3c.webp" >}}

### Step 2: Install PostgreSQL 16 Database Server and Client

To install, we use the command

     sudo apt install postgresql postgresql-client -y

After running successfully, we check if the PostgreSQL service has been started:

     sudo systemctl status postgresql

{{< figure src="./images/7182fcd1-3e7f-4420-b3df-79eae97b8c08.webp" >}}

So we have successfully installed PostgreSQL and check the PostgreSQL version with the command

     psql --version

{{< figure src="./images/e052ebdb-9948-4edd-aa8a-f54488fd2425.webp" >}}

Here, we can see that the PostgreSQL version is 15.

### Step 3: Update the PostgreSQL Admin User Password

By default, we can connect to the PostgreSQL server without using any password. Let's see this in action using the psql utility:

    sudo -u postgres psql

{{< figure src="./images/485f7ad5-fc47-44d0-94c9-591625a85a45.webp" >}}

In the above output, the prompt **postgres=#** indicates that the connection is working with the PostgreSQL server.

Next, we use the command to change the password to **PassKhongChilaPasss**

    ALTER USER postgres PASSWORD 'PassKhongChilaPasss';

then we exit with the command `\q`

{{< figure src="./images/a1eb8fd0-13ee-4273-92b0-9f34b9780fcf.webp" >}}

Now, let's connect back to the database server:

    psql -h localhost -U postgres

Enter the **PassKhongChilaPasss** string as the password and now we are connected to the database.

{{< figure src="./images/7eb308fa-5c0e-4891-9ae1-b7c6b60bf5c0.webp" >}}

So we have successfully set the password for the admin user **postgres**

### Step 4: Configure PostgreSQL to Allow Remote Connections

By default, PostgreSQL only accepts connections from the local server. However, we can easily modify the configuration to allow connections from remote clients.

PostgreSQL reads its configuration from the **postgresql.conf** file located in the **/etc/postgresql/15/main/** directory. Here, the version indicates the major version of PostgreSQL.

{{< figure src="./images/89716e24-0d66-4873-8ab1-d5ebe405ea9c.webp" >}}

Now, let's open the **postgresql.conf** file in a text editor, uncomment the line starting with **listen_addresses** and replace '**localhost**' with **'\*'**.

{{< figure src="./images/6da30c3e-b236-433e-8577-625b736d6257.webp" >}}

Save and close the file.

Next, edit the IPv4 local connection section of the **pg_hba.conf** file to allow **IPv4** connections from all clients. Please note that this file is also located in the **/etc/postgresql/15/main/** directory.

{{< figure src="./images/77cb1ee8-37c7-459f-80ce-58493b8b8b47.webp" >}}

In case the Ubuntu firewall is running on your system, allow the PostgreSQL port 5432 with the following command,

    sudo ufw allow 5432/tcp

Then we use the **PgAdmin** tool to connect, you can download it here <https://www.pgadmin.org/download/>

{{< figure src="./images/7d35b81f-b04d-43e4-a16d-6e9da0382d4b.webp" >}}

We fill in the necessary information as shown in the image below and then click **Save**

{{< figure src="./images/b64860e4-bd93-4fae-9a34-b407a1f57b14.webp" >}}

So we have successfully connected.
