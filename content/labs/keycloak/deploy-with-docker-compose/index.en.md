---
categories: [keycloak]
date: 2023-09-04T00:00:00.000Z
description: Deploy Keycloak version 22.0.4 with docker compose
draft: false
featuredImage: /series/keycloak-docker.png
images: [/deploy-keycloak-version-22-0-4-with-docker-compose/images/index.en.png, /series/keycloak-docker.png]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
lightgallery: false
tags: [keycloak, docker, docker-compose]
title: Deploy Keycloak version 22.0.4 with docker compose
---

## Step 1: Edit the file on Linux and Mac `/etc/hosts`

```shell
127.0.0.1 keycloak.localhost
```

On Windows, the file path is usually:

```shell
c:\Windows\System32\Drivers\etc\hosts
```

## Step 2: Clone the code

```shell
git clone https://github.com/akitectio/keycloak-compose
```

```yaml
version: "3.9"
services:
  postgres:
    container_name: db
    image: "postgres:14.4"
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "postgres", "-U", "root"]
      timeout: 45s
      interval: 10s
      retries: 10
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./sql:/docker-entrypoint-initdb.d/:ro # turn it on, if you need run init DB
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: keycloak
      POSTGRES_HOST: postgres
    networks:
      - local
    ports:
      - "5432:5432"

  pgadmin:
    container_name: pgadmin
    image: "dpage/pgadmin4:5.1"
    environment:
      PGADMIN_DEFAULT_EMAIL: postgres@domain.local
      PGADMIN_DEFAULT_PASSWORD: postgres
    ports:
      - "5050:80"
    networks:
      - local

  keycloak:
    container_name: keycloak
    build:
      context: .
      args:
        KEYCLOAK_VERSION: 22.0.4
    command: ["start", "--optimized"]
    environment:
      JAVA_OPTS_APPEND: -Dkeycloak.profile.feature.upload_scripts=enabled
      KC_DB_PASSWORD: postgres
      KC_DB_URL: jdbc:postgresql://postgres/keycloak
      KC_DB_USERNAME: postgres
      KC_HEALTH_ENABLED: "true"
      KC_HTTP_ENABLED: "true"
      KC_METRICS_ENABLED: "true"
      # KC_HOSTNAME: keycloak.localhost
      # KC_HOSTNAME_PORT: 8180
      KC_HOSTNAME_URL: http://keycloak.localhost:8180
      KC_PROXY: reencrypt
      KEYCLOAK_ADMIN: admin
      KEYCLOAK_ADMIN_PASSWORD: admin
    ports:
      - "8180:8080"
      - "8787:8787" # debug port
    networks:
      - local

networks:
  local:
    name: local
    driver: bridge

volumes:
  postgres_data:
```

## Step 3: Run the docker compose command

```shell
docker compose build --no-cache keycloak

docker compose up -d
```

## Step 4: Check the running containers

```shell
docker compose ps
```

Once it starts successfully, go to <http://keycloak.localhost:8180> to test.

Log in to the admin console with the admin username and password.

<b> Username: </b> admin

<b> Password: </b>admin

By following these steps, you can run Keycloak with an external PostgreSQL database using Docker.

<b> References </b>

-   <https://medium.com/@ozbillwang/run-keycloak-locally-with-docker-compose-db9a9f2fb437>
