---
draft: false
featuredImage: /courses/docker-basic/docker-networking/featured-image.webp
images:
  - /courses/docker-basic/docker-networking/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
title: Lesson 7 - Docker Networking
description: Docker Networking is an important topic that everyone using Docker should be concerned about. In this lesson, we will explore issues related to Networking in Docker and how to solve them.
date: 2024-02-15T10:23:30-09:00
weight: 7
---

Docker Networking is an indispensable part of deploying containerized applications, facilitating the creation of interconnected and communicative environments between containers. To gain a deeper understanding of Docker Networking, let's explore the steps to configure networks and some real-world scenarios you might encounter.

### Docker Network Types

Docker provides several basic network types:

- **bridge**: The default network bridge for containers. Containers within the same bridge network can communicate with each other through IP addresses.
- **host**: Removes the network isolation between the container and the Docker host, allowing the container to use the host's network stack.
- **overlay**: Creates a network across multiple Docker daemons, suitable for distributed applications across several hosts.
- **macvlan**: Allows assigning a MAC address to a container, making it appear as a physical device on the network.

### Step 1: Configuring a Custom Bridge Network

1. **Create a Bridge Network:**

   ```bash
   docker network create --driver bridge my_custom_network
   ```

   This command creates a new bridge network named `my_custom_network`.

2. **Run Container and Connect to Custom Network:**

   ```bash
   docker run -d --name my_container --network my_custom_network nginx
   ```

   The `my_container` will run and automatically connect to `my_custom_network`.

### Step 2: Container Communication

- **Linking Containers:**

  ```bash
  docker run -d --name db_container --network my_custom_network mysql
  docker run -d --name app_container --network my_custom_network --link db_container:db my_app
  ```

  `app_container` will link to `db_container` using the alias `db`, allowing `app_container` to communicate with `db_container` using this alias.

### Step 3: Exposing and Port Mapping

- **Expose Container Externally:**

  ```bash
  docker run -d --name web_container --network my_custom_network -p 8080:80 nginx
  ```

  This command maps port 80 of `web_container` to port 8080 on the Docker host, allowing external access to `web_container` through the host's port 8080.

### Step 4: Using Docker Compose

Docker Compose makes it easier to manage complex containers and networks. Here is an example of a `docker-compose.yml` file to configure networks and services:

```yaml
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
    networks:
      - my_app_network

  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
    networks:
      - my_app_network

networks:
  my_app_network:
    driver: bridge
```

This file defines a bridge network `my_app_network` and two services `web` and `db` connected to this network.

### Conclusion

Docker Networking provides flexibility and power in how containers communicate and are managed. Understanding the different types of networks and knowing how to use them in specific scenarios is key to building and deploying efficient, secure applications in a containerized environment. Don't forget to leverage tools like Docker Compose for easier and more efficient Docker project management.