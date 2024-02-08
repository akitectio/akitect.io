---
categories:
  - keycloak
date: 2023-10-01T08:00:00+08:00
description: Deploy the latest version of Keycloak on Kubernetes
draft: false
featuredImage: /series/keycloak-k8s.png
images:
  - /keycloak-deploy-version-2204-on-microk8s/images/index.en.png
  - /series/keycloak-k8s.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
lightgallery: true
tags:
  - keycloak
  - kubernetes
  - microk8s
title: Deploy Keycloak version 22.0.4 on Microk8s
url: /keycloak-deploy-version-2204-on-microk8s
---

Here are the steps to deploy Keycloak on Kubernetes:

### Step 1: Install Microk8s

Refer to the article: [Microk8s](//courses/microk8s)

### Step 2: Create the `keycloak-service.yaml` file

We need to create a service file to expose the Keycloak application. By default, Keycloak runs on port 8080, with the previous version, we can use `ClusterIP` directly and can have replicas of the groups. But with the latest version of Keycloak, we will need to create a `headless` service, which will help us have replicas, i.e., have `HA keycloak` on Kubernetes.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: keycloak
  labels:
    app: keycloak
spec:
  ports:
    - name: http
      port: 8080
      targetPort: 8080
  selector:
    app: keycloak
  type: ClusterIP
  clusterIP: None
```

After the service yaml is created, we need to apply the deployment using the command below.

```bash
microk8s kubectl apply -f service.yaml -n keycloak
```

To check if the service has been created or not, run the command below.

```bash
microk8s kubectl get svc -n keycloak
```

### Step 3: Create the `keycloak-deployment.yaml` file

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: keycloak
  labels:
    app: keycloak
spec:
  replicas: 2
  selector:
    matchLabels:
      app: keycloak
  template:
    metadata:
      labels:
        app: keycloak
    spec:
      containers:
        - name: keycloak
          image: quay.io/keycloak/keycloak:22.0.4
          args: ["start", "--cache-stack=kubernetes"]
          env:
            - name: KEYCLOAK_ADMIN
              value: "admin"
            - name: KEYCLOAK_ADMIN_PASSWORD
              value: "admin"
            - name: KC_PROXY
              value: "edge"
            - name: jgroups.dns.query
              value: "keycloak"
            - name: KC_HOSTNAME_STRICT
              value: "false"
            - name: KC_HEALTH_ENABLED
              value: "true"
            - name: KC_METRICS_ENABLED
              value: "true"
            - name: KC_HTTP_ENABLED
              value: "true"
            - name: KC_DB
              value: "postgres"
            - name: KC_DB_PASSWORD
              value: PassKhongChilaPasss
            - name: KC_DB_USERNAME
              value: postgres
            - name: KC_DB_URL_PORT
              value: "5432"
            - name: KC_DB_URL_HOST
              value: 10.86.224.19
            - name: CACHE_OWNERS_COUNT
              value: "2"
            - name: CACHE_OWNERS_AUTH_SESSIONS_COUNT
              value: "2"
          ports:
            - name: jgroups
              containerPort: 7600
            - name: https
              containerPort: 8443
            - name: http
              containerPort: 8080
          readinessProbe:
            httpGet:
              scheme: HTTP
              path: /health/ready
              port: 8080
            initialDelaySeconds: 60
            periodSeconds: 1
```

If you have multiple replicas, i.e., more than `1` group for Keycloak and if you face issues accessing the application, add the env variables below. Update the value according to the number of groups, i.e., if you have two groups then the value will be `2`, if you have four groups then the value will be `4`.

```yaml
- name: CACHE_OWNERS_COUNT
  value: "2"
- name: CACHE_OWNERS_AUTH_SESSIONS_COUNT
  value: "2"
```

After the deployment yaml is created, we need to apply the deployment using the command below.

```bash
microk8s kubectl apply -f keycloak-deployment.yaml -n keycloak
```

To check if the deployment has been created or not, run the command below.

```bash
microk8s kubectl get deployments -n keycloak
```

### Step 4: Create the `keycloak-ingress.yaml` file

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: keycloak-ingress
  annotations:
    nginx.ingress.kubernetes.io/proxy-body-size: 2500m
    nginx.ingress.kubernetes.io/proxy-buffer-size: 12k
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  ingressClassName: nginx
  rules:
    - host: keycloak.localhost
      http:
        paths:
          - backend:
              service:
                name: keycloak
                port:
                  number: 8080
            path: /(.*)
            pathType: Prefix
```

After the ingress yaml is created, we need to apply the deployment using the command below.

```bash
microk8s kubectl apply -f keycloak-ingress.yaml -n keycloak
```

To check if the ingress has been created or not, run the command below.

```bash
microk8s kubectl get ingress -n keycloak
```

After the deployment, service, and ingress are created, you can check the status of the group to see if they are running or not. Run the command below to check the status of the group.

```bash
microk8s kubectl get pods -n keycloak
```
