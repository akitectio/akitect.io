---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-5-basic-linux-commands.webp
images: [/courses/ubuntu-basics-using-virtualbox/lesson-5-basic-linux-commands.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [ubuntu-basics-using-virtualbox]
tags: [ubuntu-server, virtualbox]
title: Lesson 6 - Basic Linux Commands on Ubuntu
description: Learn how to use basic Linux commands to manage your system and interact with the operating system, including navigating directories, managing files and directories, and viewing and editing files.
date: 2024-06-28T19:24:30.000Z
weight: 5
---

In this lesson, we will learn about the basic Linux commands for navigating directories, managing files and directories, as well as viewing and editing files. These commands are essential for effectively operating and managing a Linux system.

### 1. Navigating Directories

| Command | Description                                         | Example                   |
| ------- | --------------------------------------------------- | ------------------------- |
| `ls`    | List files and directories in the current directory | `ls`                      |
| `cd`    | Change the current working directory                | `cd /home/user/Documents` |
| `pwd`   | Display the full path of the current directory      | `pwd`                     |

#### Detailed Examples:

1.  **List Files and Directories (`ls`):**

    ```bash
    ls
    ```

    This command lists all files and directories in the current directory.

    ```plaintext
    Documents  Downloads  Music  Pictures  Videos
    ```

    To display detailed information including file permissions, owner, size, and modification date:

    ```bash
    ls -l
    ```

    ```plaintext
    drwxr-xr-x 2 user user 4096 Apr 10 09:00 Documents
    -rw-r--r-- 1 user user  512 Apr  8 12:30 file1.txt
    ```

    To display hidden files (those starting with a dot):

    ```bash
    ls -a
    ```

    ```plaintext
    .bashrc  .profile  Documents  Downloads
    ```

2.  **Change Directory (`cd`):**

    ```bash
    cd /home/user/Documents
    ```

    This command changes the current working directory to `/home/user/Documents`.

    ```bash
    cd ..
    ```

    This command moves up one directory level.

    ```bash
    cd ~
    ```

    This command changes the directory to the user's home directory.

3.  **Print Working Directory (`pwd`):**

    ```bash
    pwd
    ```

    This command displays the full path of the current directory.

    ```plaintext
    /home/user/Documents
    ```

### 2. Managing Files and Directories

| Command | Description                                            | Example                              |
| ------- | ------------------------------------------------------ | ------------------------------------ |
| `cp`    | Copy files or directories from one location to another | `cp file1.txt /home/user/backup/`    |
| `mv`    | Move or rename files and directories                   | `mv file1.txt /home/user/Documents/` |
| `rm`    | Delete files or directories                            | `rm file1.txt`                       |
| `mkdir` | Create a new directory                                 | `mkdir /home/user/newfolder`         |

#### Detailed Examples:

1.  **Copy Files and Directories (`cp`):**

    ```bash
    cp file1.txt /home/user/backup/
    ```

    This command copies `file1.txt` to the `/home/user/backup/` directory.

    To copy a directory and its contents:

    ```bash
    cp -r /home/user/Documents /home/user/backup/
    ```

    This command recursively copies the `Documents` directory and its contents to the `/home/user/backup/` directory.

2.  **Move or Rename Files and Directories (`mv`):**

    ```bash
    mv file1.txt /home/user/Documents/
    ```

    This command moves `file1.txt` to the `/home/user/Documents/` directory.

    To rename a file:

    ```bash
    mv oldname.txt newname.txt
    ```

    This command renames `oldname.txt` to `newname.txt`.

    To move and rename a file:

    ```bash
    mv oldname.txt /home/user/Documents/newname.txt
    ```

    This command moves `oldname.txt` to `/home/user/Documents/` and renames it to `newname.txt`.

3.  **Delete Files and Directories (`rm`):**

    ```bash
    rm file1.txt
    ```

    This command deletes `file1.txt`.

    To delete a directory and all its contents:

    ```bash
    rm -r /home/user/Documents/
    ```

    This command deletes the `/home/user/Documents/` directory and all files and subdirectories within it.

4.  **Create a New Directory (`mkdir`):**

    ```bash
    mkdir /home/user/newfolder
    ```

    This command creates a new directory named `newfolder` in the `/home/user/` directory.

    To create nested directories:

    ```bash
    mkdir -p /home/user/newfolder/subfolder
    ```

    This command creates `newfolder` and `subfolder` inside it if they do not already exist.

### 3. Viewing and Editing Files

| Command | Description                         | Example          |
| ------- | ----------------------------------- | ---------------- |
| `cat`   | Display the contents of a file      | `cat file1.txt`  |
| `nano`  | Open a file in the Nano text editor | `nano file1.txt` |
| `vim`   | Open a file in the Vim text editor  | `vim file1.txt`  |

#### 3.1. Vim Editor

| Command    | Description                        | Example                                                          |
| ---------- | ---------------------------------- | ---------------------------------------------------------------- |
| `vim`      | Open a file in the Vim text editor | `vim file1.txt`                                                  |
| `Esc`      | Command mode                       | Press `Esc` to enter command mode                                |
| `i`        | Insert mode                        | Press `i` to enter insert mode                                   |
| `:wq`      | Save and exit                      | Type `:wq` and press `Enter` to save the file and exit Vim       |
| `:q!`      | Exit without saving                | Type `:q!` and press `Enter` to exit without saving the file     |
| `/text`    | Search for text                    | Type `/text` and press `Enter` to search for "text" in the file  |
| `dd`       | Delete the current line            | Press `dd` to delete the current line                            |
| `yy`       | Copy the current line              | Press `yy` to copy the current line                              |
| `p`        | Paste the copied or cut line       | Press `p` to paste the copied or cut line below the current line |
| `u`        | Undo                               | Press `u` to undo the last action                                |
| `Ctrl + r` | Redo the undone action             | Press `Ctrl + r` to redo the undone action                       |

**Detailed Examples:**

1.  **Open a file in Vim:**

    ```bash
    vim file1.txt
    ```

2.  **Command mode and insert mode in Vim:**

    -   Press `Esc` to enter command mode.
    -   Press `i` to enter insert mode.

3.  **Save and exit Vim:**

    -   Type `:wq` and press `Enter` to save the file and exit.
    -   Type `:q!` and press `Enter` to exit without saving.

4.  **Search and replace text:**

    -   Type `/text` and press `Enter` to search for "text".
    -   To replace text, use the command `:%s/oldtext/newtext/g`.

5.  **Delete, copy, and paste lines:**

    -   Press `dd` to delete the current line.
    -   Press `yy` to copy the current line.
    -   Press `p` to paste the copied or cut line below the current line.

6.  **Undo and redo actions:**

    -   Press `u` to undo the last action.
    -   Press `Ctrl + r` to redo the undone action.

#### 3.2. Nano Editor

| Command    | Description                         | Example                                                                             |
| ---------- | ----------------------------------- | ----------------------------------------------------------------------------------- |
| `nano`     | Open a file in the Nano text editor | `nano file1.txt`                                                                    |
| `Ctrl + O` | Save the file                       | Press `Ctrl + O`, then press `Enter` to save the file                               |
| `Ctrl + X` | Exit Nano                           | Press `Ctrl + X` to exit Nano                                                       |
| `Ctrl + K` | Cut text                            | Move the cursor to the line to cut and press `Ctrl + K`                             |
| `Ctrl + U` | Paste text                          | Move the cursor to the position to paste and press `Ctrl + U`                       |
| `Ctrl + W` | Search for text                     | Press `Ctrl + W`, type the search term, and press `Enter`                           |
| `Ctrl + \` | Replace text                        | Press `Ctrl + \`, type the search term and the replacement term, then press `Enter` |
| `Ctrl + G` | Display help                        | Press `Ctrl + G` to display the Nano help menu                                      |

**Detailed Examples:**

1.  **Open a file in Nano:**

    ```bash
    nano file1.txt
    ```

2.  **Save a file in Nano:**

    -   Press `Ctrl + O`.
    -   Press `Enter` to confirm saving the file.

3.  **Exit Nano:**

    -   Press `Ctrl + X`.

4.  **Cut and paste text:**

    -   To cut, move the cursor to the line to cut and press `Ctrl + K`.
    -   To paste, move the cursor to the position to paste and press `Ctrl + U`.

5.  **Search and replace text:**

    -   Press

    `Ctrl + W` to search for a term.

    -   Press `Ctrl + \` to replace a term.

6.  **Display help:**

    -   Press `Ctrl + G` to display the Nano help menu.

### Conclusion

In this lesson, you have learned the basic commands to navigate directories, manage files and directories, as well as view and edit files on a Linux system. These commands are essential for effectively operating and managing your system. Practice these commands regularly to become proficient in using them.
