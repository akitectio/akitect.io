---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-7-managing-services-and-processes.webp
images: [/courses/ubuntu-basics-using-virtualbox/lesson-7-managing-services-and-processes.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [ubuntu-basics-using-virtualbox]
tags: [ubuntu-server, virtualbox]
title: Lesson 7 - Managing Services and Processes
description: Learn how to manage services and processes on Ubuntu Server to start, stop, and manage services, as well as view and manage running processes on the system.
date: 2024-06-28T19:24:30.000Z
weight: 7
---

In this lesson, we will learn how to manage services and processes in the Linux system. Managing services and processes is an important part of maintaining stable and efficient system operations. We will learn how to use commands like `systemctl`, `service` to manage services and commands like `ps`, `top`, `htop`, and `kill` to manage processes.

### 1. Service Management

#### `systemctl`: System Service Management

The `systemctl` command is the main tool for managing services in systems using systemd.

| Command             | Description                                           | Example                        |
| ------------------- | ----------------------------------------------------- | ------------------------------ |
| `systemctl start`   | Start a service                                       | `sudo systemctl start nginx`   |
| `systemctl stop`    | Stop a service                                        | `sudo systemctl stop nginx`    |
| `systemctl restart` | Restart a service                                     | `sudo systemctl restart nginx` |
| `systemctl enable`  | Enable a service to start automatically on boot       | `sudo systemctl enable nginx`  |
| `systemctl disable` | Disable a service from starting automatically on boot | `sudo systemctl disable nginx` |
| `systemctl status`  | Check the status of a service                         | `sudo systemctl status nginx`  |

**Detailed examples:**

1.  **Install Nginx:**

    ```bash
    sudo apt update
    sudo apt install nginx
    ```

2.  **Start Nginx:**

    ```bash
    sudo systemctl start nginx
    ```

3.  **Check the status of Nginx:**

    ```bash
    sudo systemctl status nginx
    ```

    The result will display the current status of the Nginx service, including information about PID, runtime, and error messages if any.

4.  **Stop Nginx:**

    ```bash
    sudo systemctl stop nginx
    ```

5.  **Restart Nginx:**

    ```bash
    sudo systemctl restart nginx
    ```

6.  **Enable Nginx to start automatically when the system boots:**

    ```bash
    sudo systemctl enable nginx
    ```

7.  **Disable Nginx from starting automatically when the system boots:**

    ```bash
    sudo systemctl disable nginx
    ```

#### `service`: Traditional Service Management

The `service` command is used to manage services on systems not using systemd.

| Command           | Description                   | Example                      |
| ----------------- | ----------------------------- | ---------------------------- |
| `service start`   | Start a service               | `sudo service nginx start`   |
| `service stop`    | Stop a service                | `sudo service nginx stop`    |
| `service restart` | Restart a service             | `sudo service nginx restart` |
| `service status`  | Check the status of a service | `sudo service nginx status`  |

**Detailed examples:**

1.  **Start Nginx:**

    ```bash
    sudo service nginx start
    ```

2.  **Check the status of Nginx:**

    ```bash
    sudo service nginx status
    ```

3.  **Stop Nginx:**

    ```bash
    sudo service nginx stop
    ```

4.  **Restart Nginx:**

    ```bash
    sudo service nginx restart
    ```

### 2. Process Management

#### `ps`: Display Running Processes

The `ps` command is used to display running processes in the system.

| Command  | Description                      | Example  |
| -------- | -------------------------------- | -------- |
| `ps`     | Display current user's processes | `ps`     |
| `ps aux` | Display all running processes    | `ps aux` |
| `ps -ef` | Display processes in full format | `ps -ef` |

**Detailed examples:**

1.  **Display current user's processes:**

    ```bash
    ps
    ```

2.  **Display all running processes:**

    ```bash
    ps aux
    ```

3.  **Display processes in full format:**

    ```bash
    ps -ef
    ```

#### `top`: Real-Time Process Monitoring

The `top` command provides an interactive interface for monitoring running processes in real-time.

| Command | Description                            | Example |
| ------- | -------------------------------------- | ------- |
| `top`   | Display running processes in real-time | `top`   |

**Detailed examples:**

1.  **Run `top`:**

    ```bash
    top
    ```

    The `top` interface will display a list of running processes in real-time, including information about CPU, memory, and runtime. You can use shortcuts like `q` to exit, `k` to kill a process, and `r` to change the priority of a process.

#### `htop`: Advanced Process Monitoring Interface

The `htop` command is an advanced version of `top` with a more user-friendly interface.

| Command | Description                                                       | Example |
| ------- | ----------------------------------------------------------------- | ------- |
| `htop`  | Display running processes in real-time with an advanced interface | `htop`  |

**Detailed examples:**

1.  **Install `htop`:**

    ```bash
    sudo apt install htop
    ```

2.  **Run `htop`:**

    ```bash
    htop
    ```

    The `htop` interface will display a list of running processes with an advanced graphical interface, allowing you to easily monitor and manage processes. You can use shortcuts like `F10` to exit, `F9` to kill a process, and `F2` to configure `htop`.

#### `kill`: Stop a Process

The `kill` command is used to stop a running process.

| Command   | Description                                           | Example       |
| --------- | ----------------------------------------------------- | ------------- |
| `kill`    | Send a signal to stop a process                       | `kill PID`    |
| `kill -9` | Send the SIGKILL signal to immediately stop a process | `kill -9 PID` |

**Detailed examples:**

1.  **Display a list of running processes:**

    ```bash
    ps aux
    ```

    Find the Process ID (PID) of the process you want to stop.

2.  **Stop the process:**

    ```bash
    kill 1234
    ```

    This command will stop the process with ID `1234`.

3.  **Immediately stop the process:**

    ```bash
    kill -9 1234
    ```

    This command will send the SIGKILL signal to immediately stop the process with ID `1234`.

### Conclusion

Through this lesson, you have mastered how to manage services and processes in the Linux system. These commands are important tools to help you maintain and operate the system efficiently. I wish you successful practice and application of this knowledge in your daily work.
