---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-8-network-configuration.webp
images: [/courses/ubuntu-basics-using-virtualbox/lesson-8-network-configuration.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [ubuntu-basics-using-virtualbox]
tags: [ubuntu-server, virtualbox]
title: Lesson 8 - Network Configuration on Ubuntu
description: Learn how to configure network settings on Ubuntu Server to connect to the internet and other devices on the network.
date: 2024-06-28T19:24:30.000Z
weight: 8
---

In this lesson, we will learn how to set up and configure the network on a Linux system. Proper network configuration is crucial to ensure stable and secure connections. We will learn how to set a static IP address, check network connections, and configure SSH to secure remote connections.

### 1. Setting Up and Configuring a Static IP Address

#### Setting Up a Static IP Address

To set up a static IP address on a Linux system, you need to edit the network configuration file. Below are the steps to configure a static IP address on Ubuntu.

**Example: Configuring a Static IP Address on Ubuntu**

1.  **Open the network configuration file:**

    ```bash
    sudo nano /etc/netplan/01-netcfg.yaml
    ```

2.  **Edit the configuration file:**

    Add or modify the following content in the file to set up a static IP address:

    ```yaml
    network:
      version: 2
      ethernets:
        eth0:
          dhcp4: no
          addresses:
            - 192.168.1.100/24
          gateway4: 192.168.1.1
          nameservers:
            addresses:
              - 8.8.8.8
              - 8.8.4.4
    ```

3.  **Apply the configuration:**

    ```bash
    sudo netplan apply
    ```

4.  **Check the configuration:**

    ```bash
    ip addr show eth0
    ```

    The output will display the configured static IP address.

    {{< admonition type="info" title="Configuration Details" >}}

    -   `dhcp4: no`: Disables DHCP for the `eth0` interface.
    -   `addresses`: Sets the static IP address to `192.168.1.100` with a `/24` subnet mask.
    -   `gateway4`: Sets the default gateway to `192.168.1.1`.
    -   `nameservers`: Sets the DNS servers to `8.8.8.8` and `8.8.4.4` (Google DNS).

    {{< /admonition >}}

### 2. Checking Network Connections

#### `ping`: Checking Network Connectivity

The `ping` command is used to check network connectivity to an IP address or domain name.

| Command | Description                                                | Example           |
| ------- | ---------------------------------------------------------- | ----------------- |
| `ping`  | Check network connectivity to an IP address or domain name | `ping google.com` |

**Detailed Example:**

1.  **Check connectivity to google.com:**

    ```bash
    ping google.com
    ```

    The output will display the sent and received packets, helping you check network connectivity.

    ```plaintext
    PING google.com (216.58.214.14): 56 data bytes
    64 bytes from 216.58.214.14: icmp_seq=0 ttl=54 time=24.5 ms
    64 bytes from 216.58.214.14: icmp_seq=1 ttl=54 time=24.0 ms
    ```

#### `ifconfig`: Display Network Information

The `ifconfig` command is used to display network configuration of network interfaces.

| Command    | Description                                       | Example    |
| ---------- | ------------------------------------------------- | ---------- |
| `ifconfig` | Display network information of network interfaces | `ifconfig` |

**Detailed Example:**

1.  **Display network information of all interfaces:**

    ```bash
    ifconfig
    ```

    The output will display detailed information about network interfaces, including IP address, subnet mask, and interface status.

    ```plaintext
    eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 192.168.1.100  netmask 255.255.255.0  broadcast 192.168.1.255
            inet6 fe80::a00:27ff:fe4e:66a1  prefixlen 64  scopeid 0x20<link>
            ether 08:00:27:4e:66:a1  txqueuelen 1000  (Ethernet)
            RX packets 3920  bytes 6294358 (6.2 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 2803  bytes 256392 (256.3 KB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    ```

#### `netstat`: Display Network Connections

The `netstat` command is used to display information about network connections, routing tables, and protocol statistics.

| Command   | Description                                                                            | Example         |
| --------- | -------------------------------------------------------------------------------------- | --------------- |
| `netstat` | Display information about network connections, routing tables, and protocol statistics | `netstat -tuln` |

**Detailed Example:**

1.  **Display listening connections:**

    ```bash
    netstat -tuln
    ```

    The output will display listening TCP and UDP connections, including port numbers and IP addresses.

    ```plaintext
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State      
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
    tcp6       0      0 :::80                   :::*                    LISTEN     
    udp        0      0 0.0.0.0:68              0.0.0.0:*                          
    ```

#### `ss`: Faster Network Information Display

The `ss` command is a more powerful tool than `netstat` to display information about network connections.

| Command | Description                                   | Example    |
| ------- | --------------------------------------------- | ---------- |
| `ss`    | Display information about network connections | `ss -tuln` |

**Detailed Example:**

1.  **Display listening connections:**

    ```bash
    ss -tuln
    ```

    The output will display listening TCP and UDP connections, including port numbers and IP addresses.

    ```plaintext
    Netid   State    Recv-Q   Send-Q     Local Address:Port     Peer Address:Port
    tcp     LISTEN   0        128          0.0.0.0:22              0.0.0.0:*     
    tcp     LISTEN   0        128             [::]:80                 [::]:*     
    udp     UNCONN   0        0            0.0.0.0:68              0.0.0.0:*     
    ```

### 3. Configuring SSH and Securing Remote Connections

#### Installing SSH

To install and configure SSH on Ubuntu, you can follow these steps:

1.  **Install OpenSSH Server:**

    ```bash
    sudo apt update
    sudo apt install openssh-server
    ```

2.  **Check the status of SSH:**

    ```bash
    sudo systemctl status ssh
    ```

3.  **Start the SSH service:**

    ```bash
    sudo systemctl start ssh
    ```

4.  **Enable SSH to start automatically on system boot:**

    ```bash
    sudo systemctl enable ssh
    ```

**Detailed Example:**

1.  **Install OpenSSH Server:**

    ```bash
    sudo apt update
    sudo apt install openssh-server
    ```

    This command will install OpenSSH Server on your system.

2.  **Check the status of SSH:**

    ```bash
    sudo systemctl status ssh
    ```

    The output will display the current status of the SSH service, including information about the PID, uptime, and any error messages.

3.  **Start the SSH service:**

    ```bash
    sudo systemctl start ssh
    ```

    This command will start the SSH service if it is not already running.

4.  **Enable SSH to start automatically on system boot:**

    ```bash
    sudo systemctl enable ssh
    ```

    This command will set the SSH service to start automatically when the system boots.

#### Securing SSH Connections

To secure SSH connections, you can perform the following steps:

1.  **Edit the SSH configuration file:**

    ```bash
    sudo nano /etc/ssh/sshd_config
    ```

2.  **Change the default port (optional):**

    Find the line `#Port 22` and change it to another port number, for example:

    ```plaintext
    Port 2222
    ```

3.  **Disable root login via SSH:**

    Find the line `PermitRootLogin` and change it to:

    ```plaintext
    PermitRootLogin no
    ```

4.  **Limit access by IP (optional):**

    Add the following lines to limit access by IP address:

    ```plaintext
    AllowUsers user@192.168.1.100
    ```

5.  **Restart the SSH service to apply the changes:**

    ```bash
    sudo systemctl restart ssh
    ```

6.  **Configure the firewall to allow SSH connections:**

    ```bash
    sudo ufw allow 2222/tcp
    sudo ufw enable
    ```

    Replace `2222` with the port number you chose if you changed the default port.

**More Details:**

-   **Edit the SSH configuration file:**

    ```bash
    sudo nano /etc/ssh/sshd_config
    ```

        This command will open the SSH configuration file in the Nano text editor.

-   \*\*Change the default port:

\*\*

    Find the line `#Port 22` and change it to another port number, for example:

    ```plaintext
    Port 2222
    ```

    This will change the default port for SSH connections from 22 to 2222, enhancing security by avoiding automatic attacks on the default port.

-   **Disable root login via SSH:**

        Find the line `PermitRootLogin` and change it to:

    ```plaintext
    PermitRootLogin no
    ```

        This will disable the ability to log in to the system via SSH using the root account, protecting the system from remote attacks.

-   **Limit access by IP:**

        Add the following lines to limit access by IP address:

    ```plaintext
    AllowUsers user@192.168.1.100
    ```

        This will only allow the user `user` from the IP address `192.168.1.100` to connect to the system via SSH.

-   **Restart the SSH service to apply the changes:**

    ```bash
    sudo systemctl restart ssh
    ```

        This command will restart the SSH service to apply the configuration changes.

-   **Configure the firewall to allow SSH connections:**

    ```bash
    sudo ufw allow 2222/tcp
    sudo ufw enable
    ```

        This will allow connections on port 2222 through the firewall and enable the firewall.

-   **Test SSH connection from another machine:**

    ```bash
    ssh user@ip -p 2222
    ```

### Conclusion

In this lesson, you have learned how to set up and configure the network on a Linux system, check network connectivity, and secure SSH remote connections. These skills are crucial to ensure your system is always connected securely and reliably. Happy practicing and applying this knowledge to your daily work.
