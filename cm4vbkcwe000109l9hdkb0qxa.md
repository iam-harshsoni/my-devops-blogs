---
title: "Easy Steps to Run Minikube and Kubernetes on AWS EC2 for Beginners"
seoTitle: "Run Minikube on AWS EC2: A Beginner's Guide"
seoDescription: "Learn to set up Minikube and Kubernetes on AWS EC2 with this beginner-friendly guide. Start running local clusters with ease!"
datePublished: Thu Dec 19 2024 12:50:16 GMT+0000 (Coordinated Universal Time)
cuid: cm4vbkcwe000109l9hdkb0qxa
slug: easy-steps-to-run-minikube-and-kubernetes-on-aws-ec2-for-beginners
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734612547155/63bffcec-bde5-4e80-9e06-21a1fccbc8f7.png
tags: ec2, docker, aws, development, kubernetes, developer, devops, jenkins, ci-cd, devops-articles, 90daysofdevops, trainwithshubham, 90daysofdevops-chanllenge

---

# **What is minikube?**

Minikube is a lightweight tool that spins up a local Kubernetes cluster within a virtual machine (VM). Think of it as your Kubernetes sandbox for learning and testing.

**Why Minikube?**  
Here are some of its top benefits:

* **Easy to Install**: A straightforward setup process for quick onboarding.
    
* **Kubernetes Features**: Supports most Kubernetes functionalities and add-ons.
    
* **Cross-Platform**: Works with various operating systems and hypervisors.
    
* **Flexibility**: Switch between different Kubernetes versions and configurations seamlessly.
    

---

# Installing Minikube on AWS EC2

Here’s a step-by-step guide to get Minikube up and running on an AWS EC2 instance.

1\. Launch an EC2 instance with the Ubuntu 22.04 image, ensuring it has at least 2 CPUs, 2GB of free memory, and 20GB of free disk space. You can choose any instance type that meets these requirements, such as t2.medium, which is not included in the free tier.

2\. Connect to the instance via SSH and update the packages with the command:

```bash
sudo apt update && sudo apt upgrade -y
```

3\. Install Docker, which is the container runtime that `minikube` uses by default. You can install it using the official Docker script with the following command:

```bash
sudo apt install docker.io
```

4. Add your user to the Docker group so you can run Docker commands without using sudo. You will need to log out and log back in for this change to take effect. Use the following command:
    

```bash
sudo usermod -aG docker $USER
```

5\. Install `kubectl`, which is the command-line tool for interacting with Kubernetes clusters.

```bash
sudo snap install kubectl --classic
```

6\. Install `minikube`, which is the main tool for creating and managing local Kubernetes clusters. You can download the latest version from the GitHub releases page and make it executable with the commands:

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

7\. Start `minikube` with the command:

```bash
minikube start
```

This will create a VM and install a Kubernetes cluster inside it. It may take a few minutes to complete.

Verify that `minikube` is running and that you can connect to the cluster with `kubectl` with the command:

```bash
minikube status && kubectl get nodes
```

You should see something like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734611068106/a2fb79ba-6030-4c64-b503-5412719d34b5.png align="center")

Congratulations! You have successfully installed `minikube` on AWS EC2 machines and launched a local Kubernetes cluster.

---

# **What is a pod?**

### **What is a Pod?**

A pod is the smallest deployable unit in Kubernetes. It’s a wrapper for one or more containers, sharing the same network and storage resources.

**Key Characteristics**:

* **Ephemeral**: Pods can be created and destroyed dynamically.
    
* **Unique IPs**: Assigned within the cluster for seamless communication.
    
* **Volumes**: Attach persistent storage for your applications.
    
* **Lifecycle Hooks**: Customize pod behavior during creation, updates, or deletion.
    

### **Create Your First Pod on Kubernetes Using Minikube**

To create a pod on Kubernetes using `minikube`, you need to follow these steps:

1. Create a YAML file that defines the pod specification. For example, you can create a file named **nginx-pod.yaml** with the following content:
    
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx
      label:
        name: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
    ```
    
    This file defines a pod named nginx with the label name=nginx. It has one container named nginx that uses the nginx image from Docker Hub and exposes port 80.
    
2. Apply the YAML file to the cluster with the command:
    
    ```bash
    kubectl apply -f my-pod.yaml
    ```
    
3. This will create the pod and pull the image from Docker Hub if it is not already cached.
    
4. Check the status of the pod with the command:
    
    ```bash
    kubectl get pod nginx
    ```
    
5. You should see something like this:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734611587496/0c1e488a-2f89-4c4b-9015-f7d60223e6e9.png align="center")
    
    This means that the pod is running and ready.
    

6\. To access the NGINX web server running in your Pod, you need to expose it as a service. You can use the following command to create a NodePort service:

```bash
kubectl expose pod nginx --type=NodePort --port=80
```

7\. Access the pod from your browser by using the `minikube` service command, which creates a temporary URL for the pod. You can do this with the command:

```bash
minikube service nginx --url
```

8\. Copy and paste this URL into your browser and you should see the default Nginx welcome page.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734612252536/2834ba6e-45be-47c9-b4e5-9c2ec702ea48.png align="center")

Congratulations! You have successfully created your first pod on Kubernetes using `minikube`.

---

### **Wrapping Up**

In this guide, you learned:

* What Minikube is and why it’s a game-changer for Kubernetes beginners.
    
* How to set up Minikube on an AWS EC2 instance.
    
* The basics of pods and how to deploy one using Minikube.
    

Minikube simplifies your Kubernetes learning journey and brings the power of container orchestration to your fingertips.

If you enjoyed this guide and want more hands-on content, connect with me on GitHub or [LinkedIn](https://www.linkedin.com/in/harsh-soni-007hs/).

Let’s build something amazing together! 🚀