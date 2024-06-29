---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-10-backup-and-restore.webp
images: [/courses/ubuntu-basics-using-virtualbox/lesson-10-backup-and-restore.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [ubuntu-basics-using-virtualbox]
tags: [ubuntu-server, virtualbox]
title: Lesson 10 - Backup and Restore
description: In this lesson, we will learn how to back up and restore data on an Ubuntu Server. Data backup is an essential part of protecting your critical data from loss and corruption, while helping you recover data quickly when needed, using tools like `tar` and `rsync`, setting up automated backups with `cron`, and restoring data from a backup.
date: 2024-06-29T02:24:30.000Z
weight: 10
---

In this lesson, we will learn how to back up and restore data on a Linux system using `tar` and `rsync`, set up automated backups with `cron`, and restore data from backups. Data backup is crucial for protecting your data from loss due to system failures or other issues.

### 1. Using tar and rsync for Data Backup

#### `tar`: Creating Backups

The `tar` command is used to create backups of files and directories by archiving them into a tarball file.

| Command     | Description                                            | Example                                    |
| ----------- | ------------------------------------------------------ | ------------------------------------------ |
| `tar -cvf`  | Create a tarball file from files and directories       | `tar -cvf backup.tar /home/user/Documents` |
| `tar -czvf` | Create a compressed tarball file with gzip             | `tar -czvf backup.tar.gz /home/user`       |
| `tar -rvf`  | Add files to an existing tarball file                  | `tar -rvf backup.tar newfile.txt`          |
| `tar -xvf`  | Extract files from a tarball file                      | `tar -xvf backup.tar`                      |
| `tar -xzvf` | Extract files from a compressed tarball file with gzip | `tar -xzvf backup.tar.gz`                  |

**Detailed Examples:**

1.  **Create a backup of the `/home/user/Documents` directory:**

    ```bash
    tar -cvf backup.tar /home/user/Documents
    ```

    This command creates a `backup.tar` file containing all files and directories in `/home/user/Documents`.

2.  **Create a compressed backup of the `/home/user` directory:**

    ```bash
    tar -czvf backup.tar.gz /home/user
    ```

    This command creates a compressed `backup.tar.gz` file containing all files and directories in `/home/user`.

3.  **Extract files from a tarball:**

    ```bash
    tar -xvf backup.tar
    ```

    This command extracts all files and directories from `backup.tar` to the current directory.

#### `rsync`: Synchronizing Backups

The `rsync` command is used to back up and synchronize files and directories between different locations.

| Command      | Description                                           | Example                                               |
| ------------ | ----------------------------------------------------- | ----------------------------------------------------- |
| `rsync -av`  | Backup and synchronize files and directories          | `rsync -av /home/user/Documents /backup`              |
| `rsync -avz` | Backup and synchronize files and directories over SSH | `rsync -avz /home/user/Documents user@remote:/backup` |

**Detailed Examples:**

1.  **Backup the `/home/user/Documents` directory to the `/backup` directory:**

    ```bash
    rsync -av /home/user/Documents /backup
    ```

    This command copies all files and directories from `/home/user/Documents` to the `/backup` directory, preserving permissions and directory structure.

2.  **Backup the directory over SSH:**

    ```bash
    rsync -avz /home/user/Documents user@remote:/backup
    ```

    This command copies all files and directories from `/home/user/Documents` to the `/backup` directory on the remote server `remote`, using SSH for secure transfer.

### 2. Setting Up Automated Backups with cron

`cron` is a powerful tool that allows you to automate tasks on a Linux system.

#### Creating an Automated Backup Task with cron

1.  **Open the crontab file:**

    ```bash
    crontab -e
    ```

2.  **Add the following line to the crontab file to set up a daily backup at 2 AM:**

    ```plaintext
    0 2 * * * tar -czvf /backup/backup_$(date +\%F).tar.gz /home/user
    ```

    This command creates a compressed backup of the `/home/user` directory and saves it to the `/backup` directory with the current date in the filename.

**Detailed Example:**

1.  **Set up a daily backup:**

    ```bash
    0 2 * * * rsync -av /home/user/Documents /backup
    ```

    This line runs the `rsync` command to back up the `/home/user/Documents` directory to the `/backup` directory every day at 2 AM.

### 3. Restoring Data from Backups

#### Extracting and Restoring from a tarball

1.  **Extract files from a tarball:**

    ```bash
    tar -xzvf /backup/backup_2024-06-28.tar.gz -C /home/user
    ```

    This command extracts the `backup_2024-06-28.tar.gz` file to the `/home/user` directory.

**Detailed Example:**

1.  **Restore from a tarball:**

    ```bash
    tar -xzvf /backup/backup_2024-06-28.tar.gz -C /home/user
    ```

    This command extracts the `backup_2024-06-28.tar.gz` file to the `/home/user` directory, restoring all backed-up files and directories.

#### Using rsync to Restore Data

1.  **Restore data from the backup directory:**

    ```bash
    rsync -av /backup /home/user/Documents
    ```

    This command copies all files and directories from the `/backup` directory to the `/home/user/Documents` directory.

**Detailed Example:**

1.  **Restore using rsync:**

    ```bash
    rsync -av /backup /home/user/Documents
    ```

    This command copies all files and directories from the `/backup` directory to the `/home/user/Documents` directory, restoring the backed-up data.

### Conclusion

In this lesson, you have learned how to use `tar` and `rsync` to back up and restore data, as well as how to set up automated backups with `cron`. These skills are essential for ensuring that your data is always protected and can be quickly restored when needed. Practice these commands to become proficient in managing backups on your Linux system.
