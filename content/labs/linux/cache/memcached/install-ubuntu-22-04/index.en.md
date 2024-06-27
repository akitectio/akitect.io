---
categories:
  - linux
  - network
date: 2024-02-26T10:23:30-09:00
draft: false
featuredImage: /labs/linux/memcached.png
images:
  - /labs/linux/memcached.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - cache
  - memcached
title: Install and Secure Memcached on Ubuntu 22.04
description: Learn how to install and secure Memcached on Ubuntu 22.04. Memcached is a high-performance, distributed memory object caching system, generic in nature, but intended for use in speeding up dynamic web applications by alleviating database load.
url: /install-and-secure-memcached-on-ubuntu-22-04/
weight: 1
---

### What is Memcached?

Memcached is a high-performance, distributed memory object caching system, generic in nature, but intended for use in speeding up dynamic web applications by alleviating database load. Memcached was developed by Brad Fitzpatrick in 2003. Memcached is open-source software and is distributed under the BSD license.

### Install Memcached on Ubuntu 22.04

To install Memcached on Ubuntu 22.04, we will use APT, the default package manager of Ubuntu.

```bash
sudo apt update
sudo apt install memcached -y
```

{{<figure src="/images/memcached-installation.png" alt="Installing Memcached on Ubuntu 22.04" caption="Installing Memcached on Ubuntu 22.04">}}


After the installation, you can check the status of Memcached using the following command:

```bash
sudo systemctl status memcached
```

{{<figure src="/images/memcached-status.png" alt="Checking the status of Memcached" caption="Checking the status of Memcached">}}

### Secure Memcached

By default, Memcached does not have any security mechanisms. This means that anyone can connect and perform operations on the Memcached server. To secure Memcached, we will use the following methods:

#### Enable SASL Authentication

SASL (Simple Authentication and Security Layer) is a robust and flexible authentication protocol. SASL allows Memcached to authenticate users using a username and password.

First, we need to install the `libmemcached-tools` package to use the `memcstat` command:

```bash
sudo apt install libmemcached-tools -y
```

{{<figure src="/images/libmemcached-tools-installation.png" alt="Installing libmemcached-tools" caption="Installing libmemcached-tools">}}

Finally, we will restart Memcached to apply the changes:

```bash
sudo systemctl restart memcached
```

#### Check the Status of Memcached

After the installation, you can check the status of Memcached using the following command:

```bash
sudo systemctl status memcached
```

{{<figure src="/images/memcached-status.png" alt="Checking the status of Memcached" caption="Checking the status of Memcached">}}

#### Enable IP Authentication

Another way to secure Memcached is to enable IP authentication. This means that only specified IPs can connect and perform operations on the Memcached server.

First, we will open the configuration file of Memcached:

```bash
sudo nano /etc/memcached.conf
```

Then, we will find and edit the following line:

```bash
# Specify which IP address to listen on. The default is to listen on all IP addresses
-l  127.0.0.1
```

Change `# Specify which IP address to listen on. The default is to listen on all IP addresses` to `# Specify which IP address to listen on. The default is to listen on all IP addresses` and add your IP after `-l`:

```bash
# Specify which IP address to listen on. The default is to listen on all IP addresses
-l 0.0.0.0
```

{{<figure src="/images/memcached-configuration.png" alt="Editing the configuration file of Memcached" caption="Editing the configuration file of Memcached">}}

Finally, we will restart Memcached to apply the changes:

```bash
sudo systemctl restart memcached
```

{{<figure src="/images/memcached-restart.png" alt="Restarting Memcached" caption="Restarting Memcached">}}

### Conclusion

In this article, we learned how to install and secure Memcached on Ubuntu 22.04. Memcached is a high-performance, distributed memory object caching system, generic in nature, but intended for use in speeding up dynamic web applications by alleviating database load. By securing Memcached, we can prevent attacks from the outside and keep our Memcached server safe.