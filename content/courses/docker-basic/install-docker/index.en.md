---
draft: false
featuredImage: /courses/docker-basic/install-docker/featured-image.webp
images:
  - /courses/docker-basic/install-docker/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
  - apple-m1-silicon
  - mac-m1
title: Lesson 2 - Installing Docker on Mac M1
description: To install and use Docker on Mac M1 efficiently, you need to carefully follow these steps in detail. Mac M1 uses ARM architecture, so there are a few things to note to ensure Docker operates smoothly.
date: 2024-02-15T10:23:30-09:00
weight: 2
---

To install and use Docker on Mac M1 efficiently, you need to carefully follow these steps in detail. Mac M1 uses ARM architecture, so there are a few things to note to ensure Docker operates smoothly.

### Installing Docker on Mac M1:

#### Step 1: Download Docker Desktop for Mac
- Visit the official Docker website at [Docker Desktop for Mac](https://docs.docker.com/desktop/install/mac-install/) and download the version for Apple Silicon.

- Ensure you have selected the correct version for Mac M1, as there are separate versions for Intel chips and Apple Silicon.

#### Step 2: Install Docker Desktop
- Open the downloaded `.dmg` file and drag the Docker icon into the Applications folder.
- Open Docker from the Applications folder. The first time you open Docker, you may need to go through some macOS security steps to allow the app to run.

#### Step 3: Start and Configure Docker Desktop
- After installation, open Docker Desktop. The app will automatically start and run in the background.
- You may need to adjust the resource configurations such as CPU, memory, and storage in Preferences to optimize performance.

#### Step 4: Verify Installation
- Open Terminal and type `docker --version` to check the Docker version, ensuring Docker is installed correctly.

### Using Docker:

#### Step 1: Pull a Docker Image
- Open Terminal and use the command `docker pull hello-world` to pull a simple Docker image to test.
- This command will download the `hello-world` image from Docker Hub to your machine.

#### Step 2: Run a Container
- Use the command `docker run hello-world` to create and run a container from the `hello-world` image.
- This will start the container, and you should see a welcome message from Docker, indicating the container is working.

#### Step 3: Manage Containers and Images
- Use `docker logs <container-id>` to view the logs of a specific container.
- Stop running containers with `docker stop <container-name>` and remove them with `docker rm <container-name>`.

### Important Notes:
- For Mac M1, you may need to use Docker images optimized for ARM architecture. Check the image information on Docker Hub to ensure compatibility.
- If you encounter errors related to ARM architecture, you may need to look for alternative images or use solutions like QEMU to emulate the x86 architecture.

### Conclusion:
Installing and using Docker on Mac M1 might require some minor adjustments due to hardware architecture differences, but the overall process is quite straightforward. Docker is a powerful tool that allows you to package, distribute, and run your applications consistently across any environment, simplifying the application development and deployment process.