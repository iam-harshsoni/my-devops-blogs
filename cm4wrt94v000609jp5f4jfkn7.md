---
title: "How to Launch a Kubernetes Cluster with Deployment and Deploy a ToDo App"
seoTitle: "Launch Kubernetes Cluster & Deploy ToDo App"
seoDescription: "Learn how to launch a Kubernetes cluster and deploy a ToDo app with auto-scaling and auto-healing features in this comprehensive guide"
datePublished: Fri Dec 20 2024 13:12:51 GMT+0000 (Coordinated Universal Time)
cuid: cm4wrt94v000609jp5f4jfkn7
slug: how-to-launch-a-kubernetes-cluster-with-deployment-and-deploy-a-todo-app
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734700274467/fc9d196f-4246-4778-b523-44f094e562af.jpeg
tags: cloud, docker, aws, kubernetes, developer, cloud-computing, devops, jenkins, docker-compose, ci-cd, docker-images, devops-articles, 90daysofdevops, trainwithshubham, 90daysofdevops-chanllenge

---

Kubernetes is an amazing tool for managing containerized applications. Today, we’ll dive into how to launch a Kubernetes cluster with a Deployment configuration. We'll deploy a sample ToDo application, leveraging Kubernetes’ auto-healing and auto-scaling features. By the end of this guide, you'll understand Kubernetes Deployments, deploy your own ToDo app, and use `kubectl` to manage it. Let’s get started!

---

## What Are Kubernetes Deployments?

A Deployment in Kubernetes is a configuration that manages updates to Pods and ReplicaSets. Here’s a quick breakdown:

* **Pod**: A group of one or more containers that share the same network and storage.
    
* **ReplicaSet**: Ensures a specific number of Pods are running at any time.
    
* **Deployment**: Lets you define the desired state of your application—number of replicas, image version, update strategy, and more. The Deployment Controller ensures this desired state is maintained.
    

Deployments are incredibly useful for:

* Rolling out updates or rollbacks without downtime.
    
* Scaling applications up or down seamlessly.
    
* Enabling features like auto-healing and auto-scaling.
    

---

## Step-by-Step Guide: Deploying a Sample ToDo App

We’ll deploy a Node.js-based ToDo application using a public Docker image. Here are the steps:

### 1\. Create the Deployment Configuration

Create a file named `deployment.yml` with the following content:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: todo-app
  labels:
    app: todo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: todo
  template:
    metadata:
      labels:
        app: todo
    spec:
      containers:
      - name: todo
        image: harshsoni777/todo-app
        ports:
        - containerPort: 8000
```

This file defines a Deployment that creates two replicas of the ToDo app, exposing it on port 8000.

### 2\. Apply the Deployment

Run the following command to create the Deployment:

```bash
kubectl apply -f deployment.yml
```

### 3\. Verify the Deployment

Check if the Deployment and Pods are running:

```bash
kubectl get deployments
kubectl get pods
```

You should see the `todo-app` Deployment with two running Pods.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734698936574/d196abc3-4528-43f9-a071-d5420d7bdd05.png align="center")

This shows that you have two Pods named `todo-app-<random-string>` that are running and ready.

### 4\. Expose the App as a Service

```bash
kubectl expose deployment todo-app --port=80 --type=LoadBalancer
```

This will create a Service object named `todo-app` that exposes port 80 of the Pods as a LoadBalancer service. A LoadBalancer service allocates an external IP address and routes traffic from that IP address to the Pods.

### 5\. Verify that the Service is created by running:

```bash
kubectl get services
```

You should see something like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734698842672/2a71a067-489c-40a4-b0e8-c665ca335735.png align="center")

This shows that you have two Services in your cluster: the `kubernetes` service, which is the default service for the cluster API, and the `todo-app` service, which exposes port 80 of the Pods as port 8000 on an external IP address.

### 5\. Access the App

Open your browser and visit `http://<external-ip>:8000` to see your ToDo app in action.

Congratulations! You have successfully deployed a sample ToDo app on Kubernetes using Deployment.

---

## Managing the Deployment with `kubectl`

Here are some handy `kubectl` commands to manage your Deployment:

1. **Describe the Deployment**
    
    ```bash
    kubectl describe deployment todo-app
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734699065586/cca37f03-ed71-4164-857c-343013e5c6f7.png align="center")
    
2. **Scale the Deployment**
    
    ```bash
    kubectl scale deployment todo-app --replicas=3
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734699179834/a9344b57-3fc9-484b-b538-b16c487878f0.png align="center")
    
    Adjust the number of replicas to meet demand.
    
3. **Update the App’s Image**
    
    ```bash
    kubectl set image deployment todo-app todo=<new-image>
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734699242034/497ae0e4-90be-42a9-90ce-07e4a38b9337.png align="center")
    
    Roll out a new version seamlessly.
    
4. **Rollback to a Previous Version**
    
    ```bash
    kubectl rollout undo deployment todo-app
    ```
    
    Instantly restore the app to its previous state if needed.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734699490962/44c94c0d-05c4-4ca2-8f88-532dac79fdfb.png align="center")
    

---

## Auto-Healing and Auto-Scaling

### Auto-Healing

To enable auto-healing, we don’t need to do anything special. Kubernetes already does this by default for Deployments. If a container crashes or a node fails, Kubernetes will automatically create a new Pod and assign it to a healthy node.

```bash
kubectl delete pod <pod-name>
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734699589591/3ff31bac-84a9-4fd6-82b7-cc68e7d7a73e.png align="center")

### Auto-Scaling

To enable auto-scaling, we need to create a Horizontal Pod Autoscaler (HPA) object that defines how we want to scale our Deployment based on CPU utilization or other metrics.

To create an HPA object that scales our Deployment between 1 and 5 replicas based on CPU utilization, run:

```bash
kubectl autoscale deployment todo-app --min=1 --max=5 --cpu-percent=50
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734699822161/a6401364-1779-40a5-a142-21c042db5e8d.png align="center")

To verify that the HPA is created, run:

```bash
kubectl get hpa
```

You should see something like this:

This shows that you have one HPA named `todo-app` that targets the Deployment `todo-app` with a minimum of 1 and a maximum of 5 replicas, and a target CPU utilization of 50%.

To see the current CPU utilization and the desired number of replicas, run:

```bash
kubectl get hpa -w
```

This will show you the HPA status in a watch mode, which updates every few seconds.

This shows that the current CPU utilization is 0% and the current number of replicas is 2

To test the auto-scaling feature, you can generate some load on your Pods by running:

```bash
kubectl run -it --rm load-generator --image=busybox /bin/sh
```

This will create a temporary Pod named `load-generator` that runs a shell in an interactive mode.

In the shell, run the following command:

```bash
while true; do wget -q -O- http://todo-app.default.svc.cluster.local; done
```

This will send an infinite loop of requests to the ToDo app service.

In another terminal, watch the HPA status by running:

```bash
kubectl get hpa -w
```

You should see that the CPU utilization increases and the number of replicas changes accordingly.

This shows that the HPA has scaled up the number of replicas to 5 to handle the increased load.

To stop the load generator, press `Ctrl+C` in the shell and exit.

You should see that the CPU utilization decreases and the number of replicas changes accordingly.

This shows that the HPA has scaled down the number of replicas to 1 to save resources.

Congratulations! You have successfully enabled auto-healing and auto-scaling for your Deployment.

---

## Wrapping Up

In this guide, we covered:

* Understanding Kubernetes Deployments.
    
* Deploying a sample ToDo app on Kubernetes.
    
* Using `kubectl` for inspection and management.
    
* Enabling auto-healing and auto-scaling.
    

Feel free to try this on your own Kubernetes cluster. If you found this guide helpful, share it with your network or drop a comment below. Let’s learn and grow together!

Follow me for more DevOps tips and insights on GitHub and LinkedIn.