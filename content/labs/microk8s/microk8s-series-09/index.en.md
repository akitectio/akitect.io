---
categories:
  - microk8s
date: 2023-01-15T08:00:00+08:00
description: To detect and prevent errors, it will be very convenient if there is a good monitoring tool. Monitoring systems are responsible for monitoring the technology that a company uses (hardware, network and communication, operating system or application, among others) to analyze its performance, and detect and alert about potential errors. A good monitoring system is capable of monitoring devices, infrastructure, applications, services, and even business processes.
draft: false
featuredImage: /series/microk8s/lesson-9-using-the-monitoring-system-integration-elasticsearch-fluentd-and-kibana-grafana-zipkin-of-microk8s.webp
images:
  - /series/microk8s/lesson-9-using-the-monitoring-system-integration-elasticsearch-fluentd-and-kibana-grafana-zipkin-of-microk8s.webp
  - /lesson-9-using-the-monitoring-system-integration-elasticsearch-fluentd-and-kibana-grafana-zipkin-of-microk8s/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - microk8s-series
tags:
  - microk8s
  - Kubernetes
  - k8s
  - ubuntu
  - virtualbox
  - virtualbox 7
  - virtualbox 7 on ubuntu 22.04
  - microservices pattern
  - kong api gateway
  - grafana
  - Zabbix
  - Prometheus
  - alertmanager
title: Lesson 9 - Using the Monitoring System integration (Elasticsearch, Fluentd and Kibana, Grafana, Zipkin) of Microk8s
url: /lesson-9-using-the-monitoring-system-integration-elasticsearch-fluentd-and-kibana-grafana-zipkin-of-microk8s
weight: 9
---

# Activation of Monitoring System Toolset

## Activating Observability Add-ons

To activate a monitoring system, it is important to first enable observability add-ons. These add-ons are responsible for monitoring the technology that a company uses, such as hardware, network and communication, operating systems or applications, among other things, to analyze their performance and detect and alert about potential errors.

A good monitoring system has the ability to monitor devices, infrastructure, applications, services, and even business processes. By doing so, it can improve hardware usage, prevent incidents, detect issues early, and ultimately enhance the company's image and productivity.

To activate observability add-ons, follow these steps:

1. Open the Visual Studio Code IDE.
2. Open the active document that contains the code for the technology being used.
3. Navigate to the extensions tab on the left-hand side of the IDE.
4. Search for and install the observability add-ons that are compatible with the technology being used.
5. Configure the add-ons to monitor the desired aspects of the technology.
6. Run the code and monitor the output in the integrated output pane and terminal.

By following these steps, a monitoring system can be activated and observability add-ons can be used to monitor the technology being used, detect and prevent errors, and ultimately improve the company's productivity and image.

### Step 1: First, we connect to the vm **microk8s-master-01**

```java
ssh ubuntu@192.168.56.2
```

After successfully ssh-ing, we activate **observability addons** using the command

```markdown
microk8s enable observability
```

{{< figure src="./c23304c3-1579-4e8e-a7e2-41aaf7484a45.webp" >}}

When the application is successful, there will be a default username and password.

### Step 2: Port-forward port 3000

To access the service, we use port-forwarding with the following command:

```shell
microk8s kubectl port-forward -n observability service/kube-prom-stack-grafana --address 0.0.0.0 3000:80
```

{{< figure src="./8160b5a3-5d57-4e09-a5ec-fb3228897909.webp" >}}

This command forwards port 3000 on the local machine to port 80 on the Kubernetes service named `kube-prom-stack-grafana` in the `observability` namespace. This allows us to access the service through a web browser on the local machine by navigating to `http://localhost:3000`.

### Bước 3: Login vào admin của Grafana

### Step 3: Login to Grafana admin

Access the following URL: http://192.168.56.2:3000

{{< figure src="./bb154f4c-473b-4814-87b1-7bd7244e72ea.webp" >}}

username: **admin**

password: **prom-operator**

After successful login, go to the dashboard section: http://192.168.56.2:3000/dashboards

{{< figure src="./849f7781-13ce-4729-8a03-ebe841a3f5a9.webp" >}}

Here, there is a list of pre-designed dashboard templates that can be used or customized as per the user's needs.

{{< figure src="./974abdf2-3bbf-4b30-b95f-5f7e0b3179b7.webp" >}}

### Monitoring System with Zabbix 6.2

Please refer to the series that I have shared before:

https://akitect.io/series/zabbix

### Activate community addons fluentd (Elasticsearch, Fluentd and Kibana)

First, enable the community using the command:

```
 microk8s enable community
```

{{< figure src="./8797a148-841f-4f82-9c8c-1886ccd25e2e.webp" >}}

After successfully enabling the community, activate the **fluentd addons** using the command:

```
microk8s enable fluentd
```

{{< figure src="./580d8f45-21d8-4d47-99f8-7f1532d5524a.webp" >}}

After successful application, wait for about 1 minute for the services to be up. Then, use port forwarding to access the Kibana service:

```
microk8s kubectl port-forward -n kube-system service/kibana-logging --address 0.0.0.0 8181:5601
```

{{< figure src="./1c216157-8a40-40fa-9f12-c8fa399173fb.webp" >}}

Access the URL: http://192.168.56.2:8181 to access Kibana.

{{< figure src="./0317007a-870a-4911-9ecb-9229ba2574da.webp" >}}

Thus, we have successfully activated the **fluentd addons**.

## Installing Zipkin

Note that we must activate the **fluentd community addons** before installing **Zipkin** using the following command:

```
microk8s kubectl apply -f https://gist.githubusercontent.com/tdduydev/e979e6d36c7b03d6b41160b470ec70fa/raw/e8e71b14cbb013204262409aaa46a899a29ab64e/zipkin-all-in-one.yaml
```

{{< figure src="./36336ccc-8c97-4790-b5fe-4c976b8a6fb3.webp" >}}

Content of the file **zipkin-all-in-one.yaml**

{@embed: https://gist.github.com/tdduydev/e979e6d36c7b03d6b41160b470ec70fa}

After successful activation, use port forwarding to access the service:

```
 microk8s kubectl port-forward -n default service/zipkin --address 0.0.0.0 9411:9411
```

Access the URL: http://192.168.56.2:9411 to access Zipkin.

{{< figure src="./70fcd036-8b16-4149-9f42-5e3fa6396668.webp" >}}

Thus, you have successfully installed **Zipkin**.

To use Zipkin and Kong, please refer to [Lesson 8].

## Conclusion

Thus, I have successfully installed the Monitoring System in less than 15 minutes. The advantage is that I can control the entire system and be proactive when errors occur, without receiving complaints from customers.
