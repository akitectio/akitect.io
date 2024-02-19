---
draft: false
featuredImage: /courses/docker-basic/introduction-to-docker/featured-image.webp
images:
  - /courses/docker-basic/introduction-to-docker/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
title: Lesson 1 - Introduction to Docker
description: Docker is one of the most popular tools for containerizing applications. It offers efficiency and operational cost reduction, enabling any developer in any development environment to build stable and reliable applications.
date: 2024-02-15T10:23:30-09:00
weight: 1
---

Docker is one of the most popular tools for containerizing applications. It offers efficiency and operational cost reduction, enabling any developer in any development environment to build stable and reliable applications.

# Modern Application Development

Modern application development faces many challenges, particularly in managing the technical and technological aspects of applications across various cloud and development environments. DevOps teams often need to ensure applications run smoothly regardless of where they are deployed.

Meanwhile, development teams want to quickly update new features, but sometimes inadvertently affect application stability due to environment-dependent errors.

To mitigate this issue, many organizations choose to package applications into containers. This approach packages the entire code, libraries, and configurations needed to run an application into a standalone package, ensuring stable operation without adding complexity, security risks, or management errors.

Initially, container technology wasn't widely adopted due to its complexity, but Docker changed this, making containers widely popular.

# What is Docker?

Docker is an open-source containerization platform based on Linux that developers use to build, run, and package applications for deployment using containers. Unlike virtual machines, Docker containers offer:

- Operating system-level abstraction with optimized resource use
- Compatibility across systems
- Efficient build and testing processes
- Faster application execution

Essentially, Docker containers break down an application's functionality into multiple components, allowing for independent deployment, testing, or scaling as needed.

For example:

- A web application might be divided into 3 containers: one for the web server, one for the database server, and one for the Redis server, each running a specific service.
- A microservice application might be divided into multiple containers, each containing a specific microservice.

# Docker Architecture Components

Docker has 4 main components:

- **Images:** Images are like blueprints containing instructions to create a Docker container. They define what's needed for the application to run, such as dependencies and processes when the application starts. You can download images from `DockerHub` or create your own images by writing instructions in a file called `Dockerfile`.

- **Containers:** A container is a running instance of an image, where the application or its independent modules run. Using an object-oriented programming analogy, if an image is a class, then a container is an instance of that class. This enhances operational efficiency by allowing multiple containers to be created from one image.

- **Registries:** A Docker registry is like a repository of images. Docker Hub is the default public registry, containing public and official images for various languages and platforms. You can also have private registries and configure them as the default source for your custom requirements.

- **Docker Engine:** Docker Engine is a core component of Docker architecture, where applications run. You can consider Docker Engine as an application installed on the system to manage containers, images, and builds. Docker Engine uses a client-server architecture and includes the following sub-components:

  - **Docker Daemon:** The server running on the host, responsible for building and managing Docker images.
  - **Docker Client:** The command-line interface (CLI) to send commands to the Docker Daemon using specific Docker commands. Although the client can run on the host, it uses the Docker Engine's REST API to connect remotely to the daemon.
  - **REST API:** Supports interaction between the client and daemon.

# Benefits of Docker in the Software Development Lifecycle

Docker offers numerous benefits to application architecture at different stages of the software development lifecycle:

- **Building.** Docker saves time, effort, and money for development teams by packaging their applications into one or more modules. By investing initial effort in creating a suitable image for the application, the build process can avoid the repetitive challenges of multiple version dependencies that could cause issues in the production environment.

- **Testing.** Docker allows independent testing of containerized applications (or their components) without affecting other application components. This also creates a secure framework by eliminating tight dependencies and allowing better fault tolerance.

- **Deployment & Maintenance.** Docker reduces friction between teams by ensuring consistent use of library versions and packages at each development stage. Moreover, deploying a pre-tested container eliminates the introduction of errors into the build process, thus allowing efficient movement to production.

- **Production Environment.** Docker reduces the time and effort needed to deploy applications, as well as minimizes deployment risks. Using containers ensures that your application will run as expected in the production environment.

# Comparing Docker with Virtual Machines (VMs)

Virtual machines and containers both create isolated environments for running applications, but they have significant differences:

- **Virtual Machines:** VMs run on a virtualized operating system (hypervisor) and require a full operating system to run. Each VM needs a significant amount of resources, including memory, CPU, and storage. VMs also require time to start up and shut down.

- **Containers:** Containers run on a shared operating system (host OS) and share the OS kernel with other containers. Each container shares the operating system and requires fewer resources than VMs. Containers also start up and shut down faster than VMs.