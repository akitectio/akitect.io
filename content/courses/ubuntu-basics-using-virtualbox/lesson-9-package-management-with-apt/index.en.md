---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-9-package-management-with-apt.webp
images: [/courses/ubuntu-basics-using-virtualbox/lesson-9-package-management-with-apt.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [ubuntu-basics-using-virtualbox]
tags: [ubuntu-server, virtualbox]
title: Lesson 9 - Managing Packages with APT
description: In this lesson, we will learn how to manage software packages on an Ubuntu Server using APT, a popular package management tool on Debian-based operating systems, including Ubuntu, that helps you install, update, and remove software efficiently.
date: 2024-06-29T01:24:30.000Z
weight: 9
---

In this lesson, we will learn how to manage software packages on a Linux system using the `apt` tool. Managing software packages is a crucial part of maintaining and operating a system, helping you install, update, and remove software efficiently. We will learn how to use the `apt-get install`, `apt-get update`, `apt-get upgrade` commands and how to manage software repositories.

### 1. Installing and Updating Software Packages

#### `apt-get install`: Installing Software Packages

The `apt-get install` command is used to install new software packages on the system.

| Command           | Description                   | Example                        |
| ----------------- | ----------------------------- | ------------------------------ |
| `apt-get install` | Install new software packages | `sudo apt-get install apache2` |

**Detailed Example:**

1.  **Install Apache2:**

    ```bash
    sudo apt-get install apache2
    ```

    This command will download and install Apache2, one of the most popular web servers.

    ```plaintext
    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    The following additional packages will be installed:
      apache2-bin apache2-data apache2-utils
    Suggested packages:
      www-browser apache2-doc apache2-suexec-pristine | apache2-suexec-custom
    The following NEW packages will be installed:
      apache2 apache2-bin apache2-data apache2-utils
    0 upgraded, 4 newly installed, 0 to remove and 0 not upgraded.
    Need to get 1,720 kB of archives.
    After this operation, 6,859 kB of additional disk space will be used.
    Do you want to continue? [Y/n]
    ```

    Press `Y` and Enter to proceed with the installation.

#### `apt-get update`: Updating Package Lists

The `apt-get update` command is used to update the list of available packages from the software repositories.

| Command          | Description                                             | Example               |
| ---------------- | ------------------------------------------------------- | --------------------- |
| `apt-get update` | Update the list of available packages from repositories | `sudo apt-get update` |

**Detailed Example:**

1.  **Update package lists:**

    ```bash
    sudo apt-get update
    ```

    This command will update the list of available software packages from the configured repositories in the `/etc/apt/sources.list` file.

    ```plaintext
    Hit:1 http://us.archive.ubuntu.com/ubuntu focal InRelease
    Get:2 http://us.archive.ubuntu.com/ubuntu focal-updates InRelease [111 kB]
    Get:3 http://us.archive.ubuntu.com/ubuntu focal-backports InRelease [98.3 kB]
    Get:4 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]
    Fetched 323 kB in 2s (142 kB/s)
    Reading package lists... Done
    ```

#### `apt-get upgrade`: Upgrading Installed Packages

The `apt-get upgrade` command is used to upgrade all the installed packages to the latest version.

| Command           | Description                                          | Example                |
| ----------------- | ---------------------------------------------------- | ---------------------- |
| `apt-get upgrade` | Upgrade all installed packages to the latest version | `sudo apt-get upgrade` |

**Detailed Example:**

1.  **Upgrade installed packages:**

    ```bash
    sudo apt-get upgrade
    ```

    This command will download and install the latest versions of all installed software packages on the system.

    ```plaintext
    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    Calculating upgrade... Done
    The following packages will be upgraded:
      apache2 apache2-bin apache2-data apache2-utils
    4 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
    Need to get 1,320 kB of archives.
    After this operation, 1,024 B of additional disk space will be used.
    Do you want to continue? [Y/n]
    ```

    Press `Y` and Enter to proceed with the upgrade.

### 2. Managing Software Repositories

Software repositories are the sources from which packages are obtained. You can add, remove, and manage these repositories through the configuration file `/etc/apt/sources.list`.

#### Adding a New Repository

To add a new repository, you need to edit the `/etc/apt/sources.list` file or create a new file in the `/etc/apt/sources.list.d/` directory.

**Example:**

1.  **Open the repository configuration file:**

    ```bash
    sudo nano /etc/apt/sources.list
    ```

2.  **Add the following line to add a new repository:**

    ```plaintext
    deb http://repository.example.com/ubuntu focal main
    ```

3.  **Save the file and update the package list:**

    ```bash
    sudo apt-get update
    ```

#### Removing a Repository

To remove a repository, you need to delete the corresponding line in the `/etc/apt/sources.list` file or delete the file in the `/etc/apt/sources.list.d/` directory.

**Example:**

1.  **Open the repository configuration file:**

    ```bash
    sudo nano /etc/apt/sources.list
    ```

2.  **Delete the line corresponding to the repository you want to remove.**

3.  **Save the file and update the package list:**

    ```bash
    sudo apt-get update
    ```

### Conclusion

In this lesson, you have learned how to install, update, and upgrade software packages on a Linux system using `apt`. You have also learned how to manage software repositories, an essential part of keeping your system updated and secure. We hope you practice these commands and apply this knowledge to your daily work tasks.
