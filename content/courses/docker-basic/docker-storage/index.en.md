---
draft: false
featuredImage: /courses/docker-basic/docker-storage/featured-image.webp
images:
  - /courses/docker-basic/docker-storage/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
title: Lesson 8 - Docker Storage
description:  Docker Storage is an important part of Docker, helping manage data and storage within containers. In this article, we will explore the types of storage in Docker, how to manage and use storage within containers.
date: 2024-02-15T10:23:30-09:00
weight: 8
---

Managing data in Docker requires an understanding of the storage mechanisms Docker provides. Each mechanism has its own advantages and disadvantages, suitable for specific use cases. Below is a more detailed look at how to use and manage data in Docker through Volumes, Bind Mounts, and tmpfs Mounts.

### Using Volumes in Docker

Volumes are the preferred choice for persistent data storage as they are managed by Docker and detached from the lifecycle of a container.

**Creating and managing Volumes:**

1. **Create a new Volume:**

   ```bash
   docker volume create my_volume
   ```

2. **List existing Volumes:**

   ```bash
   docker volume ls
   ```

3. **Run a container and attach a Volume:**

   ```bash
   docker run -v my_volume:/app/data <image_name>
   ```
   
   Here, `my_volume` is the name of the volume, and `/app/data` is the mount point inside the container.

4. **Inspect details about a Volume:**

   ```bash
   docker volume inspect my_volume
   ```

### Using Bind Mounts

Bind Mounts allow you to mount any directory on the host machine into a container. The data is stored directly on the host's filesystem and can be easily accessed from both the host and the container.

**How to use Bind Mounts:**

```bash
docker run -v /path/on/host:/path/in/container <image_name>
```

For example, to mount the `/src` directory on the host to the `/app` directory in the container:

```bash
docker run -v /src:/app <image_name>
```

Note: Bind Mounts can pose security risks if not used carefully, as they allow containers direct access to the host's filesystem.

### Using tmpfs Mounts

Tmpfs Mounts create a temporary memory partition in the host's RAM, providing a way to store temporary data without writing to disk.

**Launching a container with tmpfs Mounts:**

```bash
docker run --tmpfs /path/in/container <image_name>
```

For example, to create a tmpfs mount at `/app/cache` inside the container:

```bash
docker run --tmpfs /app/cache <image_name>
```

Data in tmpfs mounts is deleted when the container stops or is removed, making them an ideal choice for data that requires temporary storage or high security.

### Choosing the Right Storage Method

When deciding how to store data in Docker, consider the following:

- **Does the data need to persist across container restarts?** Use Volumes.
- **Does the data need to be managed outside of Docker and easily accessible from the host?** Consider using Bind Mounts.
- **Is the data only temporary and not needed after the container stops?** Tmpfs Mounts are a good choice.

Remember, each storage method has its own pros and cons, and the choice of the right method depends on the specific requirements of your application and your deployment environment.