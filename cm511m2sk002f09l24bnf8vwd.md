---
title: "Step-by-Step Guide: Monitoring with Prometheus and Grafana"
seoTitle: "Monitor Systems Using Prometheus and Grafana"
seoDescription: "Learn how to monitor Kubernetes clusters using Prometheus and Grafana with this comprehensive step-by-step setup guide"
datePublished: Mon Dec 23 2024 12:58:17 GMT+0000 (Coordinated Universal Time)
cuid: cm511m2sk002f09l24bnf8vwd
slug: step-by-step-guide-monitoring-with-prometheus-and-grafana
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734958277298/bd1e21ee-ff93-41fa-947d-b90ed7156532.png
tags: cloud, ec2, docker, aws, kubernetes, developer, monitoring, devops, prometheus, grafana, 90daysofdevops, trainwithshubham, 90daysofdevops-chanllenge, tws, monitoring-using-prometheus-and-grafana-on-aws-ec2

---

Monitoring your Kubernetes cluster is crucial for maintaining a healthy and performant environment. This guide will walk you through the setup of **Prometheus** and **Grafana** for monitoring, ensuring that you can visualize metrics effectively.

---

## **Step 1: Install Kubectl**

Kubectl is the command-line tool for Kubernetes. Install it by running:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

---

## **Step 2: Install Minikube**

Minikube lets you run Kubernetes locally. Install it with:

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

---

## **Step 3: Create a Kubernetes Cluster with Minikube**

Start your Kubernetes cluster:

```bash
minikube start
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734157491152/5251166b-5dcc-44dc-8fe7-6143f5a7d131.png align="center")

---

## **Step 4: Install Helm**

Helm simplifies Kubernetes application management. Install it with:

```bash
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734157682839/fb8420c3-b5ea-4e43-8397-8982b607d488.png align="center")

---

## **Step 5: Install Prometheus**

Prometheus is a powerful monitoring system. Install it using Helm:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734157768971/f866ab45-7f9d-43e0-97cc-80b7c1e0e907.png align="center")

Verify the Prometheus services:

```bash
kubectl get svc
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734157811954/7d6d0688-b500-4987-85fe-006934ef8b7a.png align="center")

To access Prometheus in your browser:

1. Expose the Prometheus service:
    
    ```bash
    kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext --port=9090
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734157965246/347306aa-75b1-4e64-892e-5351c591674d.png align="center")
    
2. Forward the port:
    
    ```bash
    kubectl port-forward service/prometheus-server-ext 9090:9090 --address=0.0.0.0 &
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734158163708/5268d46f-a137-473c-98d8-217becd78931.png align="center")
    
3. Update your security group’s inbound rules to allow traffic on port 9090.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734158106307/b8032922-2d43-4b17-84fd-74a412615c2a.png align="center")
    

---

## **Step 6: Install Grafana**

Grafana provides visualization for your metrics. Install it using Helm:

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install grafana grafana/grafana
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734158197076/7ef97290-1d0c-4e51-9467-442488e0585f.png align="center")

Retrieve the Grafana admin password:

```bash
# Run the command shown in your CLI output after Grafana installation.
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160097725/f0dd87e3-3262-45eb-ab50-518eba85c468.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160110308/71e4b82c-f91c-42ad-b31a-835cdca757ac.png align="center")

Expose the Grafana service:

```bash
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext --port=3000
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160247768/034e9ec9-b2fd-4a37-a992-04c468e80160.png align="center")

Forward the port:

```bash
kubectl port-forward service/grafana-ext 3000:3000 --address=0.0.0.0 &
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160301662/8d32ab53-a241-456a-8aad-045de308275a.png align="center")

Update your security group’s inbound rules to allow traffic on port 3000.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160368937/331973be-5ce9-4fce-9712-64745f7dc6a8.png align="center")

---

## **Step 7: Integrate Prometheus with Grafana**  

1. In Grafana, navigate to **Data Sources** and add Prometheus.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160463591/a8353b39-d03f-4d18-ac02-705bc1a9f9b1.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160478406/53db23da-3887-4540-aae6-f3181ec9b7ba.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160550946/ebadc72e-951e-4f9b-8012-2153acc9400b.png align="center")
    
2. Enter the Prometheus server URL.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160588399/bf70d847-822b-42be-a80e-ab033d2d5044.png align="center")
    
3. Select **Performance Type** as ‘Prometheus’
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160646622/0fb5be08-007e-4292-80f7-cb37ebaa81c4.png align="center")
    

---

## **Step 8: Create a Grafana Dashboard**

1. Go to **Dashboard** and click **New** &gt; **Import**.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160702845/64380eb7-f9b8-4b34-8185-e7d4c2076c6f.png align="center")
    
2. Use the default dashboard ID: **3662**.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160756922/2b040a44-c9b2-4000-ab51-31879a15bc36.png align="center")
    
3. Select the Prometheus data source created earlier and click **Import**.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160847216/1a9a582f-0a39-4e00-8520-776124586e00.png align="center")
    

### Congratulations 🎉

Your Grafana dashboard with Prometheus as the data source is ready! You can now monitor your Kubernetes environment efficiently.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160895930/ac012b81-81e8-411c-9efe-79627e814678.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734160906229/364ab3a4-0d7e-4353-bd41-47b3239200bb.png align="center")

---

### **Conclusion**

Setting up Prometheus and Grafana for monitoring might seem challenging initially, but with this step-by-step guide, it becomes a straightforward process. By integrating Prometheus for data collection and Grafana for visualization, you ensure that your Kubernetes cluster is well-monitored and its performance is easy to analyze. As you grow more comfortable with these tools, you can explore advanced features like creating custom alerts and dashboards tailored to your specific needs.

Happy monitoring!