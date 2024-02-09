---
categories:
  - microk8s
date: 2023-01-14T08:00:00+08:00
description: Hello everyone, Continuing the microk8s series, this is the longest Reseach part of mine and how to deploy a microservices architecture
draft: false
featuredImage: /series/microk8s/lesson-8-using-kong-gateway-to-deploy-api-gateway-for-microservices-system-on-microk8s.webp
images:
  - /series/microk8s/lesson-8-using-kong-gateway-to-deploy-api-gateway-for-microservices-system-on-microk8s.webp
  - /lesson-8-using-kong-gateway-to-deploy-api-gateway-for-microservices-system-on-microk8s/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - Kubernetes
  - k8s
  - ubuntu
  - virtualbox
  - kong-api-gateway
title: Lesson 7 - Using Kong Gateway to deploy API Gateway for Microservices system on Microk8s
url: /lesson-8-using-kong-gateway-to-deploy-api-gateway-for-microservices-system-on-microk8s
weight: 8
---

## What is KONG?

- Kong is a Microservice API Gateway. Kong provides a flexible layer to manage secure communication between clients and microservices through APIs. Also known as an API gateway, API middleware, or in some cases a service mesh. It was made available as an open-source project in 2015, with its core values being high performance and scalability.

- Kong is a Lua application running within bash and implemented thanks to the lua-bash-module.

## Why do we use KONG?

If you are building for the Web, Mobile or IoT (Internet of Things), you may need a common functionality to run your real-world software. Kong can help by acting as a gateway (or sidecar) for microservice requests while providing load balancing, logging, authentication, rate limiting, transformation, and more through plugins.

{{< figure src="./images/7e5f7ff4-3e27029b-d779-48ad-b196-07bfe027881f.webp" >}}

But surely, Kong is a template tool that will help speed up development time and it supports configurable plugins. And the development community makes it stable.

- Many authentication plugins

  1. JWT
  2. LDAP (most commonly used)
  3. OAuth2

- Security plugins:

  1. CL
  2. CORS
  3. Dynamic SSL
  4. IP Restriction

- Traffic control plugin is very useful for cost limitation such as rate limiting, request size limiting, response rate limiting, and others.
- Analytics and monitoring plugins visualize, test, and monitor API traffic such as Prometheus, Data Dog, and RunScope.
- Transformation plugin that transform request and responses on the fly such as Request Transformer, Response Transformer.
- Logging plugin that log request and response data using the best transport for your infrastructure: TCP, UDP, HTTP, StatsD, Syslog and others.

I have briefly introduced Kong API Gateway, you can refer to more information at https://konghq.com/

Now I will configure as follows:

In the previous article, I instructed you to build 2 images service01 and service02.

[Lesson 7 - Configuring Jenkins on Ubuntu 22.04 and writing Pipeline Build Service](/lesson-7-configuring-jenkins-on-ubuntu-2204-and-writing-pipeline-build-service)

| IP           | Hostname          | vCPU   | RAM | DISK     |
| ------------ | ----------------- | ------ | --- | -------- |
| 127.0.0.1    | Host              | 8 core | 32G | SSD 500G |
| 192.168.56.2 | microk8s-master-1 | 1 core | 2G  | 50G      |
| 192.168.56.3 | microk8s-master-2 | 1 core | 2G  | 50G      |
| 192.168.56.4 | microk8s-master-3 | 1 core | 2G  | 50G      |
| 192.168.56.5 | microk8s-worker-1 | 1 core | 2G  | 50G      |
| 192.168.56.6 | microk8s-worker-2 | 1 core | 2G  | 50G      |
| 192.168.56.7 | microk8s-worker-3 | 1 core | 2G  | 50G      |
| 192.168.56.8 | microk8s-worker-4 | 1 core | 2G  | 50G      |

Currently does not support installation using databases (DB-mode) or not using databases (DB-less-mode)

{{< figure src="./images/7e5f7ff4-3e27029b-d779-48ad-b196-07bfe027881f.webp" >}}

Here I use DB-less-mode to save Kong configuration

{{< youtube "hYHbbyWyVTQ" >}}

### Step 1: Configure Kong Controller Ingress

```bash
ssh ubuntu@192.168.56.2
```

We proceed to apply Kong Controller Ingress, here I have researched many articles and provided a standard file below, you can run it directly or customize it according to your requirements. To perform, we use the following command.

```bash
microk8s kubectl apply -f https://gist.githubusercontent.com/akitectio/2b875c579a2ef53f68053e4e5af5b90b/raw/deef2792d33db7fb62c784421c447731f1d5808d/all-in-one-dbless.yaml
```

{{< figure src="./images/866cebdd-e19c-4b8a-a4a4-89e9908983a9.webp" >}}

File contents **all-in-one-dbless.yaml**

{{< gist akitectio 2b875c579a2ef53f68053e4e5af5b90b >}}

Continue applying kong ingress configuration

```bash
microk8s kubectl apply -f https://gist.githubusercontent.com/akitectio/1d8c8f270e28d022cd6cdfb06b6a4484/raw/02ba2bdc5c5c6ae0996a40340e6f3a0e6bc965c2/kong-ingress.yaml
```

{{< figure src="./images/63fdc794-1f9b-4308-8029-45d2e40f23a5.webp" >}}

File contents **kong-ingress.yaml**

{{< gist akitectio 1d8c8f270e28d022cd6cdfb06b6a4484 >}}

### Step 2: Configure service01 and service02

**Service01**

```bash
microk8s kubectl apply -f  https://gist.githubusercontent.com/akitectio/e1e50da183eff226f7b2fc5e39781d8c/raw/97eb61e0debbd76a9c42f61e01e47054653334c9/service01-deployment.yaml
```

{{< figure src="./images/bd8a95fc-8a48-4a39-bc12-2f0440091feb.webp" >}}

File contents **service01-deployment.yaml**

{{< gist akitectio e1e50da183eff226f7b2fc5e39781d8c >}}

**Service02**

```bash
microk8s kubectl apply -f  https://gist.githubusercontent.com/akitectio/e3b20b53d33302d654cd1b058a283562/raw/44553ddf0f142431fc1c394a25e89c8a4e513255/service02-deployment.yaml
```

{{< figure src="./images/2f06a6d8-48a5-45bc-8ad3-5b7f36d85bdb.webp" >}}

File contents **service02-deployment.yaml**

{{< gist akitectio e3b20b53d33302d654cd1b058a283562 >}}

### Step 3: Configure Kong ingress for Service01 and Service02

**Service01**

```bash
 microk8s kubectl apply -f https://gist.githubusercontent.com/akitectio/0f43b50aeed7d1a2cd51cc6dcf307ace/raw/f70bab1ac04d98280777e21821ca530690403285/kong-ingress-service01.yaml
```

{{< figure src="./images/78eb7dfc-2bc8-4221-ba95-843ae7495c93.webp" >}}

File contents **kong-ingress-service01.yaml**

{{< gist akitectio 0f43b50aeed7d1a2cd51cc6dcf307ace >}}

**Service02**

```bash
 microk8s kubectl apply -f https://gist.githubusercontent.com/akitectio/6e366ab62b4d01e84352f3e616627201/raw/a49523cc901736875e7af33e0b54d633fc2c927c/kong-ingress-service02.yaml
```

{{< figure src="./images/db441b0a-844e-409c-94b5-c6bcab88ea45.webp" >}}

File contents **kong-ingress-service02.yaml**

{{< gist akitectio 6e366ab62b4d01e84352f3e616627201 >}}

After applying the configuration successfully, we check whether the configuration is correct or not by going to the link:

1. http://192.168.56.5/service01

{{< figure src="./images/bbaa70ab-5f01-475d-8603-b5a6cc92f298.webp" >}}

3. http://192.168.56.5/service02

{{< figure src="./images/f31d0d66-d48a-4e24-b256-79689c8a39c7.webp" >}}

### Custom Config Kong ingress configuration

Here I increase the set `connect_timeout`, `read_timeout`, `write_timeout` so you can use the command

```bash
microk8s kubectl apply -f https://gist.githubusercontent.com/akitectio/61b2dce63558ccfed18b6095abc06ed6/raw/6cca022d014c919153ef66a8bd8eb5634c4cec28/timeout-kong-ingress.yaml
```

{{< figure src="./images/25b93eaa-60f9-455a-ba7e-792f9e8d18e4.webp" >}}

File contents **timeout-kong-ingress.yaml**

{{< gist akitectio 61b2dce63558ccfed18b6095abc06ed6 >}}

So I have successfully increased the timeout of ingress

You can learn more at the link: https://docs.konghq.com/kubernetes-ingress-controller/latest/references/custom-resources/

### Configure Kubernetes Ingress Controller annotations

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: service02
  namespace: kong
  annotations: # <- Add here
    konghq.com/strip-path: "true" #
    konghq.com/preserve-host: "false
spec:
    ingressClassName: kong
    rules:
      - http:
          paths:
          - path: /service02
            pathType: Prefix
            backend:
              service:
                name: service02
                port:
                  number: 5000
```

You can find the necessary service wall configurations here: https://docs.konghq.com/kubernetes-ingress-controller/latest/references/custom-resources/

### Configure the zipkin distributed tracing system plugin

Note that you need to install the zipkin server first, I have provided instructions in [article 7].

We apply the Kong configuration with the following command:

```bash
 microk8s kubectl apply -f https://gist.githubusercontent.com/akitectio/c5af0e748628ca778740451da9e1d783/raw/62ca7641ebe3d939a93c53a91cceb734d8893c3e/kong-plugin-zipkin.yaml
```

{{< figure src="./images/6adc3546-8021-4d63-ba6d-4d033832ec36.webp" >}}

The content of the file **kong-plugin-zipkin.yaml**

{{< gist akitectio c5af0e748628ca778740451da9e1d783 >}}

So you have successfully configured the Kong zipkin plugin, now you can track all requests to the API through Kong.

{{< figure src="./images/6adc3546-8021-4d63-ba6d-4d033832ec36.webp" >}}

So you have successfully installed Kong API Gateway.
