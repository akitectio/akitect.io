---
draft: false
featuredImage: /courses/docker-basic/run-docker-container/featured-image.webp
images:
  - /courses/docker-basic/run-docker-container/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
  - apple-m1-silicon
title: Lesson 4 - Running a Docker Container
description: A detailed guide on how to run a Docker container. You will learn how to choose a Docker image, use the `docker run` command, and common options like `-it`, `--rm`, `-p`, and `-v`.
date: 2024-02-15T10:23:30-09:00
weight: 4
---

Running a Docker container is a core process in using Docker, allowing you to deploy and manage your applications flexibly. Below is a detailed guide on how to run a Docker container.

### Running a Docker Container

#### 1. **Choose a Docker Image:**

First, you need to select the Docker image you want to use to run your container. You can choose an image from [Docker Hub](https://hub.docker.com/) or use an image you have previously built.

#### 2. **Use the `docker run` Command:**

The `docker run` command is the basic command to create and start a new container from a Docker image.

The basic syntax of this command is:

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

Where:
- `[OPTIONS]` are various options that can be applied, such as port mapping or volume mounting.
- `IMAGE` is the name of the Docker image you want to run.
- `[COMMAND]` is the command you want to execute inside the container when it starts.
- `[ARG...]` is a list of arguments for the command.

#### 3. **Commonly Used Options:**

- **Interactive (`-it`):** Use `-it` to run the container in interactive mode, allowing you to interact with the container's terminal. `i` stands for `--interactive` to keep STDIN open even if not attached, and `t` stands for `--tty` to allocate a pseudo-TTY.

  ```bash
  docker run -it ubuntu bash
  ```

- **Automatic Removal (`--rm`):** Use `--rm` to have Docker automatically remove the container when it stops, keeping your system clean.

  ```bash
  docker run --rm nginx
  ```

- **Port Mapping (`-p`):** Use `-p` to map a port from the container to the host. This is useful for web applications or services that need to be accessed from outside.

  ```bash
  docker run -p 80:80 nginx
  ```

  In this example, port 80 on the host is mapped to port 80 in the container.

- **Volume Mounting (`-v`):** Use `-v` to mount a directory from your host into the container, allowing data sharing.

  ```bash
  docker run -v /my/host/folder:/my/container/folder nginx
  ```

  Where `/my/host/folder` is the path on the host and `/my/container/folder` is the path inside the container.

### Conclusion

Running a Docker container is straightforward yet powerful, allowing you to deploy applications and services quickly. By using options like `-it`, `--rm`, `-p`, and `-v`, you can precisely customize how your container runs and interacts with your system.