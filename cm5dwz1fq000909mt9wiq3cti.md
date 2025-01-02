---
title: "Step-by-Step Guide: Setting Up Prometheus and Grafana Using Helm"
seoTitle: "Monitoring using Prometheus and Grafana Setup Guide with Helm"
seoDescription: "Learn how to set up Prometheus and Grafana for Kubernetes monitoring using Helm with this detailed step-by-step guide"
datePublished: Wed Jan 01 2025 13:09:24 GMT+0000 (Coordinated Universal Time)
cuid: cm5dwz1fq000909mt9wiq3cti
slug: step-by-step-guide-setting-up-prometheus-and-grafana-using-helm
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1735736851281/2036418d-f87b-4f56-9ffd-50bf71662ca5.png
tags: linux, docker, aws, kubernetes, developer, monitoring, devops, aws-lambda, helm, prometheus, grafana, docker-images, devops-articles, 90daysofdevops, 90daysofdevops-chanllenge

---

:If you're looking to monitor your Kubernetes cluster effectively, Prometheus and Grafana are your go-to tools. With Helm, you can easily set up and manage these tools. Here's a straightforward guide to get you started:

## Step 1: Install HELM

To install Helm, visit the [official Helm website](https://helm.sh) or use the following commands:

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## Step 2: Create a Namespace

Create a namespace for your monitoring tools:

```bash
kubectl create namespace monitoring
```

## Step 3: Add Prometheus Community Helm Chart

Add the Prometheus community Helm chart repository:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

Visit the [Prometheus community GitHub repository](https://github.com/prometheus-community/helm-charts) for more details.

## Step 4: Update Helm Repositories

Check the list of Helm repositories and update them:

```bash
helm repo list
helm repo update
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735386568171/80202434-b502-492d-b5bb-f14bc1e725d0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735386581991/96c839e5-3811-4f3d-adfe-9a64af6b6743.png align="center")

## Step 5: Install the Prometheus Stack

Install the Prometheus stack using Helm with the following command:

```bash
helm install prometheus-stack prometheus-community/kube-prometheus-stack \
--namespace monitoring \
--set prometheus.service.nodePort=30000 \
--set grafana.service.nodePort=31000 \
--set grafana.service.type=NodePort \
--set prometheus.service.type=NodePort
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735386535469/9c23e324-1750-4154-ae3a-93558467bfc6.png align="center")

## Step 6: Check Running Pods

Verify the pods running in the `monitoring` namespace:

```bash
kubectl get pods -n monitoring
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735386613816/929c7b53-5a68-45a2-97ad-6d7ccf9cffc8.png align="center")

## Step 7: Check Running Services

View the services in the `monitoring` namespace:

```bash
kubectl get services -n monitoring
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735386631424/3edd6eaa-70d3-4366-a883-a0c349022954.png align="center")

## Step 8: Port-Forwarding for Prometheus

Set up port-forwarding for Prometheus to access it locally:

```bash
kubectl port-forward -n monitoring svc/prometheus-stack-prometheus 9090:9090 -n monitoring
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735386702978/b2084e12-3377-4b4a-8c23-cc53d25bf829.png align="center")

## Step 9: Access Prometheus in Your Browser

Open your browser and navigate to `http://localhost:9090` to access Prometheus.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735386747577/f298b032-9130-4d8a-9e96-5dc3d6be0ebc.png align="center")

## Step 10: Port-Forwarding for Grafana

Set up port-forwarding for Grafana:

```bash
kubectl port-forward -n monitoring svc/prometheus-stack-grafana 31000:31000 -n monitoring
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735387272044/0ee525e7-3d6a-4dc3-9912-a932b0cc3a01.png align="center")

## Step 11: Access Grafana in Your Browser

Navigate to `http://localhost:31000` in your browser to access Grafana.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735387283520/1c8aa38f-cf46-451a-894c-dfbec644dcfe.png align="center")

## Step 12: Get the Grafana Admin Password

Retrieve the admin password for Grafana using this command:

```bash
kubectl get secret prometheus-stack-grafana -n monitoring -o jsonpath='{.data.admin-password}' | base64 --decode
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735387954979/6566fb1b-6ff9-48a0-83de-76b378e23f85.png align="center")

## Step 13: Log In to the Grafana Dashboard

Use the retrieved password to log in to Grafana. Once logged in, you can explore the pre-configured dashboard.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735387923263/d73a14a2-89fb-41d5-9ee4-512788c2ebba.png align="center")

## Step 14: Prometheus as Data Source

Prometheus is already set as a data source when using the Helm chart, so you can skip additional configurations.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735388106276/58844d5d-bf20-41f4-8ab1-6805c03d977c.png align="center")

## Step 15: Create a Dashboard Based on Namespace

Create a custom dashboard in Grafana to visualize metrics based on your namespace.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735388345412/6f59efc7-92b2-4f22-86f2-bc3d5fc74a21.png align="center")

## Step 16: Import Pre-Built Dashboards

Visit the Grafana Dashboards site to browse pre-built dashboards. Copy the dashboard ID, import it into Grafana, and use Prometheus as the data source.

Here's how it will look:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735388887815/e095f132-4c1d-4d70-bf4b-d31587d72238.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735390249709/e99e5e63-dee3-424f-a09b-eef72c1f5c98.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735390399087/080d1f96-f3b0-4985-a6fe-1c82b3f84260.png align="center")

---

That's it! You've successfully set up Prometheus and Grafana using Helm. Start monitoring your Kubernetes cluster efficiently and make data-driven decisions. If you found this guide helpful, share it with your network or leave a comment below!