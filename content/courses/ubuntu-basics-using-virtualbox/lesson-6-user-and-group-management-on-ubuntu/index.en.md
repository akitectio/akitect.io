---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-6-user-and-group-management.webp
images: [/courses/ubuntu-basics-using-virtualbox/lesson-6-user-and-group-management.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [ubuntu-basics-using-virtualbox]
tags: [ubuntu-server, virtualbox]
title: Lesson 6 - User and Group Management on Ubuntu
description: In this lesson, we will learn how to manage users and groups in the Linux system. Managing users and groups is an important part of ensuring security and organization in the system. We will learn how to create and manage users, manage groups, and set access permissions using basic commands such as `adduser`, `deluser`, `usermod`, `groupadd`, `groupdel`, `groupmod`, `chmod`, `chown`, and `chgrp`.
date: 2024-06-28T19:24:30.000Z
weight: 6
---

In this lesson, we will learn how to manage users and groups in the Linux system. Managing users and groups is an important part of ensuring security and organization in the system. We will learn how to create and manage users, manage groups, and set access permissions using basic commands such as `adduser`, `deluser`, `usermod`, `groupadd`, `groupdel`, `groupmod`, `chmod`, `chown`, and `chgrp`.

### 1. Creating and Managing Users

| Command   | Description                                | Example                               |
| --------- | ------------------------------------------ | ------------------------------------- |
| `adduser` | Create a new user in the system            | `sudo adduser username`               |
| `deluser` | Remove a user from the system              | `sudo deluser username`               |
| `usermod` | Modify the information of an existing user | `sudo usermod -aG groupname username` |

### 2. Managing Groups

| Command    | Description                                 | Example                                      |
| ---------- | ------------------------------------------- | -------------------------------------------- |
| `groupadd` | Create a new group in the system            | `sudo groupadd groupname`                    |
| `groupdel` | Remove a group from the system              | `sudo groupdel groupname`                    |
| `groupmod` | Modify the information of an existing group | `sudo groupmod -n newgroupname oldgroupname` |

### 3. Permissions and Access Rights

| Command | Description                                          | Example                          |
| ------- | ---------------------------------------------------- | -------------------------------- |
| `chmod` | Change the access permissions of a file or directory | `sudo chmod 755 filename`        |
| `chown` | Change the owner of a file or directory              | `sudo chown user:group filename` |
| `chgrp` | Change the group ownership of a file or directory    | `sudo chgrp groupname filename`  |

### 1. Creating and Managing Users

#### `adduser`: Create New User

The `adduser` command is used to create a new user in the system.

| Command   | Description       | Example                 |
| --------- | ----------------- | ----------------------- |
| `adduser` | Create a new user | `sudo adduser username` |

**Example:**

```bash
sudo adduser username
```

This command will create a new user named `username` and prompt you to enter detailed information such as password, full name, and contact information.

#### `deluser`: Remove User

The `deluser` command is used to remove a user from the system.

| Command   | Description   | Example                 |
| --------- | ------------- | ----------------------- |
| `deluser` | Remove a user | `sudo deluser username` |

**Example:**

```bash
sudo deluser username
```

This command will remove the user `username` from the system.

#### `usermod`: Modify User Information

The `usermod` command is used to modify the information of an existing user.

| Command   | Description             | Example                               |
| --------- | ----------------------- | ------------------------------------- |
| `usermod` | Modify user information | `sudo usermod -aG groupname username` |

**Example:**

```bash
sudo usermod -aG groupname username
```

This command will add the user `username` to the group `groupname`.

### 2. Managing Groups

#### `groupadd`: Create New Group

The `groupadd` command is used to create a new group in the system.

| Command    | Description        | Example                   |
| ---------- | ------------------ | ------------------------- |
| `groupadd` | Create a new group | `sudo groupadd groupname` |

**Example:**

```bash
sudo groupadd groupname
```

This command will create a new group named `groupname`.

#### `groupdel`: Remove Group

The `groupdel` command is used to remove a group from the system.

| Command    | Description    | Example                   |
| ---------- | -------------- | ------------------------- |
| `groupdel` | Remove a group | `sudo groupdel groupname` |

**Example:**

```bash
sudo groupdel groupname
```

This command will remove the group `groupname` from the system.

#### `groupmod`: Modify Group Information

The `groupmod` command is used to modify the information of an existing group.

| Command    | Description              | Example                                      |
| ---------- | ------------------------ | -------------------------------------------- |
| `groupmod` | Modify group information | `sudo groupmod -n newgroupname oldgroupname` |

**Example:**

```bash
sudo groupmod -n newgroupname oldgroupname
```

This command will change the group name from `oldgroupname` to `newgroupname`.

### 3. Permissions and Access Rights

#### `chmod`: Change Access Permissions

The `chmod` command is used to change the access permissions of a file or directory.

| Command | Description               | Example                   |
| ------- | ------------------------- | ------------------------- |
| `chmod` | Change access permissions | `sudo chmod 755 filename` |

**Example:**

```bash
sudo chmod 755 filename
```

This command will change the access permissions of `filename` to `755`.

#### `chown`: Change Owner

The `chown` command is used to change the owner of a file or directory.

| Command | Description  | Example                          |
| ------- | ------------ | -------------------------------- |
| `chown` | Change owner | `sudo chown user:group filename` |

**Example:**

```bash
sudo chown user:group filename
```

This command will change the owner of `filename` to `user` and group `group`.

#### `chgrp`: Change Group Ownership

The `chgrp` command is used to change the group ownership of a file or directory.

| Command | Description            | Example                         |
| ------- | ---------------------- | ------------------------------- |
| `chgrp` | Change group ownership | `sudo chgrp groupname filename` |

**Example:**

```bash
sudo chgrp groupname filename
```

This command will change the group ownership of `filename` to `groupname`.

### Conclusion

Through this lesson, you have mastered how to manage users and groups in the Linux system, as well as how to assign access permissions to files and directories. These skills are very important to ensure security and organization in the system. I wish you successful practice and application of this knowledge in your daily work.
