---
categories:
  - linux
date: 2023-12-06T12:18:39+07:00
description: Installing and configuring MAAS (Metal as a Service) on Ubuntu 22.04 LTS provides an efficient physical hardware management solution, with automation capabilities and easy scalability. This process optimizes server configuration and resource management, supporting cloud environments and data center operations
draft: false
featuredImage: /series/mass-101/installation-and-configuration-of-maas-metal-as-a-service-on-ubuntu-22-04-lts.webp
images:
  - /series/mass-101/installation-and-configuration-of-maas-metal-as-a-service-on-ubuntu-22-04-lts.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - maas-101
tags: [
 'maas',
 'ubuntu-22.04-lts',
 'server-management',
 'cloud-infrastructure',
 'data-center-automation',
 'bare-metal-provisioning'
 ]
title: Installation and Configuration of MAAS (Metal as a Service) on Ubuntu 22.04 LTS
url: /installation-and-configuration-of-maas-metal-as-a-service-on-ubuntu-22-04-lts
weight: 2
---

## Step 1: Install postgresql 14, create database and user for MAAS

You can refer to this post: [Install and configure PostgreSQL 14 on Ubuntu 22.04 LTS](/install-and-secure-postgresql-14-on-ubuntu-2404)

## Step 3: Install MAAS

```bash
sudo snap install --channel=3.4 maas
```

{{<figure src="./images/maas-101-02-01.png" >}}

{{< admonition warning "Some notes when installing" >}}
When installing MAAS on Ubuntu, there may be conflicts between the current NTP client, `systemd-timesyncd`, and the NTP client/server provided by MAAS, chrony. This can lead to time synchronization issues, especially if MAAS is configured with different upstream NTP servers than those used by `systemd-timesyncd`. To avoid conflicts, users can manually disable and stop `systemd-timesyncd` with the following command:

```bash
sudo systemctl stop systemd-timesyncd
sudo systemctl disable systemd-timesyncd
```

{{< figure src="./images/maas-101-02-02.png" >}}

{{< /admonition >}}

## Step 4: Initialize MAAS

```bash

$MAAS_DBUSER = "myuser"
$MAAS_DBPASS = "mypassword"
$MAAS_DBNAME = "mydatabase"
$HOSTNAME = "localhost"

sudo maas init --mode all --database-uri "postgres://$MAAS_DBUSER:$MAAS_DBPASS@$HOSTNAME/$MAAS_DBNAME"
```

{{< figure src="./images/maas-101-02-03.png" >}}

{{< admonition warning "Some notes when initializing MAAS" >}}

- `--mode all` will initialize MAAS with all services, including DHCP and DNS.
- `--database-uri` will declare the path to the MAAS database. This path is in the form `postgres://<username>:<password>@<hostname>/<database_name>`
  {{< /admonition >}}

Use the command to check the status of MAAS

```bash
sudo maas status
```

{{< figure src="./images/maas-101-02-04.png" >}}

## Step 6: Create a user account for MAAS

```bash
sudo maas createadmin --username admin --password admin --email akitect.io@gmail.com
```

{{< admonition warning "Some notes when creating a user account for MAAS" >}}

- `--username` will declare the username for MAAS
- `--password` will declare the password for MAAS
- `--email` will declare the email for MAAS
  {{< /admonition >}}

{{< figure src="./images/maas-101-02-05.png" >}}

## Step 7: Log in to MAAS

Access the address `http://10.86.140.147/MAAS` to log in to MAAS

{{< admonition warning "Note" >}}

To log in to MAAS, you need to use the user account that you created in step 6
{{< /admonition >}}

MAAS welcome screen

{{< figure src="./images/maas-101-02-06.png" >}}

{{< admonition info "Fields on the MAAS welcome screen" >}}

- `Region name`: Name of the MAAS server (default is `maas`)
- DNS Forwarding: IP address of the external DNS server that MAAS will use to query domain names
- `Ubuntu archive`: IP address of the Ubuntu archive mirror that MAAS will use to install software packages

- `Ubuntu extra archive`: IP address of the Ubuntu extra archive mirror that MAAS will use to install software packages

- `APT & HTTP proxy`: IP address of the proxy server that MAAS will use to access the Ubuntu archive mirror and Ubuntu extra archive mirror

{{< /admonition >}}

## Step 8: Select the Ubuntu image to install

{{< figure src="./images/maas-101-02-07.png" >}}

{{< admonition info "Fields on the Ubuntu image selection screen" >}}

- `Ubuntu release`: The version of Ubuntu you want to install
- `Architecture`: The architecture of the server you want to install
- `Sub-architecture`: The sub-architecture of the server you want to install
- `Release`: The version of Ubuntu you want to install
- `Image`: The image of Ubuntu you want to install
- `Sync now`: Synchronize the image of Ubuntu you want to install
- `Download`: Download the image of Ubuntu you want to install
- `Delete`: Delete the image of Ubuntu you want to install
- `Edit`: Edit the image of Ubuntu you want to install
- `Add`: Add the image of Ubuntu you want to install
- `Save`: Save the image of Ubuntu you want to install
- `Cancel`: Cancel the image of Ubuntu you want to install
- `Update selection`: Update the image of Ubuntu you want to install
  {{< /admonition >}}

## Step 9: Complete the MAAS installation

Select `Finish Setup` to complete the MAAS installation
{{< figure src="./images/maas-101-02-08.png" >}}

So, you have successfully installed and configured MAAS on Ubuntu 22.04 LTS.
