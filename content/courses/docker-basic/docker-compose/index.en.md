---
draft: false
featuredImage: /courses/docker-basic/docker-compose/featured-image.webp
images:
  - /courses/docker-basic/docker-compose/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
title: Lesson 5 - Docker Compose
description: Docker Compose is a powerful tool that helps you define and run multiple Docker containers easily and efficiently. You will learn how to use Docker Compose to manage your application with a simple YAML configuration file.
date: 2024-02-15T10:23:30-09:00
weight: 5
---

Docker Compose is a Docker project management tool that allows you to run and manage multiple Docker containers as a single application. With Docker Compose, you can easily configure, initialize, and manage all the services related to your application through a simple YAML configuration file.

### Step 1: Installing Docker Compose

- On **Docker Desktop for Windows and Mac**: Docker Compose is integrated by default, no additional installation required.
- On **Linux**: You need to install Docker Compose by following the commands provided on Docker's official website.

### Step 2: Creating docker-compose.yml File

The `docker-compose.yml` file is the heart of every Docker Compose project. This file contains definitions of how your project's containers are built, how they interconnect, and any other necessary configurations.

**Example:**

```yaml
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  database:
    image: postgres:latest
    environment:
      POSTGRES_DB: exampledb
      POSTGRES_USER: exampleuser
      POSTGRES_PASSWORD: examplepass
```

In the example above, we have 2 services: `web` using the `nginx:latest` image and `database` using the `postgres:latest` image. The web service is configured to map port 8080 on the host to port 80 in the container, while the database service is provided with some environment variables to configure the database.

### Step 3: Running Docker Compose

To launch your project, you simply need to open a terminal, navigate to the directory containing the `docker-compose.yml` file, and run the command:

```bash
docker-compose up
```

This command reads the `docker-compose.yml` file, builds (if necessary) and starts all defined services. Use `-d` to run the services in detached mode.

### Step 4: Managing Services

- **List running services:**

```bash
docker-compose ps
```

- **View logs of a service:**

```bash
docker-compose logs [service_name]
```

- **Stop all services:**

```bash
docker-compose down
```

### Advanced Features

- **Building Custom Images:** If you want Docker Compose to build a custom image for your service instead of using an image from Docker Hub, you can use the `build` section in the configuration file.

```yaml
services:
  myservice:
    build: ./my_service_directory
```

- **Mounting Volumes:** To store or share data between your container and host, you can use volumes.

```yaml
services:
  web:
    image: nginx
    volumes:
      - ./my_local_directory:/usr/share/nginx/html
```

- **Custom Networks:** Docker Compose allows you to define and use custom networks to create complex network relationships between containers.

```yaml
services:
  myservice:
    networks:
      - mycustomnetwork

networks:
  mycustomnetwork:
```

### Conclusion

Docker Compose is an indispensable tool in the containerized application development workflow, making it simple and efficient to manage and deploy related services. Ensure that you always refer to the official Docker Compose documentation for a deeper understanding of its features and how to use them optimally.