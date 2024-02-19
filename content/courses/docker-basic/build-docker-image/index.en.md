---
draft: false
featuredImage: /courses/docker-basic/build-docker-image/featured-image.webp
images:
  - /courses/docker-basic/build-docker-image/featured-image.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - docker
  - apple-m1-silicon
title: Lesson 3 - Building Docker Image
description: To efficiently build Docker Images, understanding each step and applying optimization techniques is crucial. Here is a more detailed guide with additional examples on how to create Dockerfile, build Images, optimize, and share Docker Images.
date: 2024-02-15T10:23:30-09:00
weight: 3
---

To efficiently build Docker Images, understanding each step and applying optimization techniques is crucial. Here is a more detailed guide with additional examples on how to create `Dockerfile`, build Images, optimize, and share Docker Images.

### Creating Dockerfile:

`Dockerfile` is a text file without any extension format, used by Docker to automate the process of building Docker images. It contains a set of instructions that Docker uses to set up and configure an image. When you run the docker build command, Docker reads the instructions from the Dockerfile and executes them one by one to create a new Docker image.

#### 1. Initializing Dockerfile:
- Create a new directory on your computer and name it after your project, for example, `my-docker-app`.
- In the `my-docker-app` directory, create a new file and name it `Dockerfile`. This will be the file where you define how to build your Docker Image.

#### 2. Defining Base Image:
- Open `Dockerfile` with a text editor and start with the `FROM` instruction to choose a base Image. For example, if you want to build a web application using Python, you could start with:
  ```docker
  FROM python:3.8-alpine
  ```
  This will use Python 3.8 on the lightweight Alpine Linux Image as the basis of your Docker Image.

#### 3. Copying Files and Installing Dependencies:
- Next, copy the necessary files from your computer into the Image and install any dependencies. For a Python application, you might have:
  ```docker
  WORKDIR /app
  COPY . /app
  RUN pip install -r requirements.txt
  ```
  This sets `/app` as the working directory, copies all files from the current directory into `/app` in the Image, and then runs `pip install` to install the necessary packages from `requirements.txt`.

#### 4. Defining Startup Command:
- Finally, define the command that Docker will execute when the container starts:
  ```docker
  CMD ["python", "app.py"]
  ```
  This specifies that Docker should run `app.py` with the Python interpreter.

### Building Docker Image:

#### 1. Building the Image:
- Open Terminal or Command Prompt, navigate to the directory containing your `Dockerfile`, and run:
  ```bash
  docker build -t my-python-app .
  ```
  This command will build a Docker Image from your `Dockerfile` with the tag `my-python-app`.

#### 2. Checking the New Image:
- Check your new Image with the command:
  ```bash
  docker images
  ```

### Optimizing Docker Image:

#### 1. Using Alpine Image:
- The lightweight Alpine Linux Image is a good choice to reduce the overall Image size, as mentioned above.

#### 2. Combining `RUN` Commands:
- Reduce the number of layers by combining `RUN` commands, for example:
  ```docker
  RUN apk add --no-cache python py-pip && \
      pip install -r requirements.txt
  ```

#### 3. Removing Cache and Temporary Files:
- Clean up after installations to remove cache and unnecessary files:
  ```docker
  RUN apk add --no-cache python py-pip && \
      pip install --no-cache-dir -r requirements.txt
  ```

### Storing and Sharing Image:

#### 1. Pushing to Docker Hub:
- Log in to Docker Hub from the Terminal with `docker login`.
- Push your Image to Docker Hub:
  ```bash
  docker push my-username/my-python-app
  ```

#### 2. Using a Private Repository:
- Configure a private repository using services like Docker Registry or cloud-based storage solutions.

By adhering to these steps and optimization techniques, you can efficiently build Docker Images and share them with the community or within your organization.