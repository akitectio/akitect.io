---
categories:
  - database
date: 2024-02-25T08:00:00+08:00
draft: false
featuredImage: /labs/postgresql/postgresql-pgpooladmin.jpeg
images:
  - /labs/postgresql/postgresql-pgpooladmin.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - postgreSQL
  - ubuntu
  - pgpool
title: Lesson 3 - Pgpool-II Admin Ubuntu 
url: /install-and-configure-pgpool-ii-admin-on-ubuntu
description: Pgpool Admin is a configuration management and monitoring tool for PGpool-II, helping you manage and monitor the performance of your PostgreSQL database. In this guide, we will go through the steps to install and configure Pgpool Admin on Ubuntu Linux, helping you manage and monitor the performance of your database.
weight: 3
---

## What is Pgpool Admin

Pgpool Admin is a configuration management and monitoring tool for PGpool-II, helping you manage and monitor the performance of your PostgreSQL database. In this guide, we will go through the steps to install and configure Pgpool Admin on Ubuntu Linux, helping you manage and monitor the performance of your database.


### Installing Pgpool Admin on Ubuntu

#### Step 1: Install apache2 and php

Add php repository

```bash
sudo apt-add-repository ppa:ondrej/php
sudo apt update
```

First, install Apache2 and PHP on the `pgpool2` server by performing the following steps:

```bash
sudo apt update
sudo apt install -y apache2 php sudo apt install php7.4 libapache2-mod-php7.4 php7.4-cli php7.4-common php7.4-pgsql 
```

After the installation is complete, check the PHP version:

```bash
php -v
```

{{< figure src="./images/php-version.jpg" >}}

#### Step 2: Download PGpool Admin 4.2.0

First, download PGpool Admin from the official website of the PGpool-II project:

```bash
wget https://www.pgpool.net/mediawiki/download.php?f=pgpoolAdmin-4.2.0.tar.gz -O pgpoolAdmin-4.2.0.tar.gz
```

After the download is complete, extract the downloaded file:

```bash
tar -xvf pgpoolAdmin-4.2.0.tar.gz
```

Move the extracted file to the `/var/www/html` directory:

```bash
sudo mv pgpoolAdmin-4.2.0 /var/www/html/pgpooladmin
```

after extracting, we need to reconfigure the access rights for the `pgpooladmin` directory:

```bash
sudo chown -R www-data:www-data /var/www/html/pgpooladmin
```

{{< figure src="./images/pgpooladmin-directory.jpg" >}}

#### Step 3: Configure apache2

Next, configure Apache2 to run PGpool Admin by creating a new configuration file:

```bash
sudo nano /etc/apache2/sites-available/pgpooladmin.conf
```

Add the following configuration to the file:

```bash
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/pgpooladmin
    <Directory /var/www/html/pgpooladmin>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

{{< figure src="./images/pgpooladmin-apache2.jpg" >}}

After the configuration is complete, save and close the configuration file.

#### Activate the configuration

Next, activate the new configuration file with the following command:

```bash
sudo a2ensite pgpooladmin.conf
```

Finally, restart the Apache2 service to apply the changes:

```bash
sudo systemctl restart apache2
```

### Step 4: Configure PGpool-II

Create pgpooladmin password

```bash
pg_md5 akitect@123
```

{{< figure src="./images/pg_md5.jpg" >}}

Next, we will configure PGpool-II by editing the configuration file `/etc/pgpool2/pgpool.conf`:

```bash
sudo nano /etc/pgpool2/pcp.conf
```

{{< figure src="./images/pgpool-pcp.jpg" >}}

where `admin` is the user you want to use to log in to pgpooladmin, `md5` is the password you created above.

Next, create and assign rights to the `.pcppass` file:

```bash
# Create .pcppass file
touch /var/www/.pcppass

# Assign rights to the .pcppass file
chmod -R 0600 /var/www/.pcppass 
```

### Step 5: Access PGpool Admin and initial configuration

After the installation is complete, access PGpool Admin by opening a web browser and accessing the IP address of the `pgpool2` server: [http://192.168.56.5/pgpooladmin/install](http://192.168.56.5/pgpooladmin/install)

{{< figure src="./images/pgpooladmin-install.jpg" >}}

select the language and press `Next`

{{< figure src="./images/pgpooladmin-install-2.jpg" >}}

Continue to add connection configuration information to `pgpool2`

{{< figure src="./images/pgpooladmin-install-3.jpg" >}}

- `pgpool.conf` enter `/etc/pgpool2/pgpool.conf`
- `pcp.conf` enter `/etc/pgpool2/pcp.conf`
- `pgpool Command` enter `/usr/sbin/pgpool`
- `PCP directory` enter `/usr/sbin`

Continue to press `Next`

{{< figure src="./images/pgpooladmin-install-4.jpg" >}}

So the initial setup is complete, we delete the `install` directory for better security.

```bash
sudo rm -rf /var/www/html/pgpooladmin/install
```

{{< figure src="./images/pgpooladmin-install-5.jpg" >}}

### Step 6: Log in to PGpool Admin

After the installation is complete, access PGpool Admin by opening a web browser and accessing the IP address of the `pgpool2` server: [http://192.168.56.5/pgpooladmin](http://192.168.56.5/pgpooladmin)

{{< figure src="./images/pgpooladmin-install-6.jpg" >}}

enter the login information created above and press `Login`

- `Username`: admin
- `Password`: akitect@123 <- password created above

{{< figure src="./images/pgpooladmin-install-7.jpg" >}}

So we have successfully configured and logged in to PGpool Admin.

### Conclusion
In this guide, we went through the steps to install and configure PGpool-II on Ubuntu Linux. By performing these steps, you can optimize and expand the capabilities of your PostgreSQL database, helping you maximize the performance and high availability of your database.