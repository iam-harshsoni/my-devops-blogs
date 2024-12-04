---
title: "Step-by-Step Guide to Setting Up Jenkins with Docker Agent"
seoTitle: "Jenkins & Docker Agent Setup Guide"
seoDescription: "Learn how to set up Jenkins with a Docker agent for CI/CD, from EC2 instance launch to creating your first pipeline project"
datePublished: Wed Dec 04 2024 11:52:48 GMT+0000 (Coordinated Universal Time)
cuid: cm49twop0000b0albbom5d4zy
slug: step-by-step-guide-to-setting-up-jenkins-with-docker-agent
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733310106787/35d842c9-a7f2-47cf-936f-c1af510bf60a.png
tags: cloud, docker, aws, kubernetes, developer, cloud-computing, devops, hashnode, jenkins, 2articles1week, ci-cd, devops-articles, 90daysofdevops, 90daysofdevops-chanllenge, tws

---

Today, I dived deep into Continuous Integration and Continuous Deployment (CI/CD) and Jenkins Declarative Pipelines using a Docker agent. To make things more exciting, I created my first project and documented every step so that you can follow along and set it up yourself. Let’s get started!

---

## **Step-by-Step Guide to Setting Up Jenkins with Docker Agent**

Here’s a simple walkthrough of everything I did:

### **1\. Launch an AWS EC2 Instance**

I created an **Ubuntu-based EC2 instance** to serve as my Jenkins master node. Ensure you have port **8080** open in the **security group’s inbound rules** to access Jenkins from your browser.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733312118288/e9c2b0e4-c116-427c-b1b7-3186e94ac71b.png align="center")

---

### **2\. Install Java**

Jenkins requires Java, so I installed **OpenJDK 17** using these commands:

```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
```

Verify the Java version to ensure it’s installed correctly.

---

### **3\. Install Jenkins**

Next, I installed Jenkins by adding its repository and installing the package:

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

Restart Jenkins for good measure:

```bash
sudo systemctl restart jenkins
sudo systemctl enable jenkins
```

---

### **4\. Install Docker and Grant Permissions**

To use Docker as an agent in Jenkins, I installed Docker and granted the necessary permissions:

```bash
sudo apt install docker.io -y
sudo usermod -aG docker $USER && newgrp docker
```

Restart Docker and Jenkins:

```bash
sudo systemctl restart docker
sudo systemctl restart jenkins
```

---

### **5\. Access Jenkins and Complete Setup**

Access Jenkins in your browser using `http://<your-ec2-public-ip>:8080`. Follow the setup wizard and install the recommended plugins.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733312147176/9b2aac30-f665-45f5-a19d-00a54274508c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733312162046/72abdeea-6ab7-4a1b-9afd-011efd020cd9.png align="center")

---

### **6\. Install Docker Pipeline Plugin**

In Jenkins, navigate to **Manage Jenkins → Plugins → Available Plugins** and install the **Docker Pipeline Plugin**.

This plugin is crucial as it allows Jenkins to run pipelines in a Docker agent specified in the `Jenkinsfile`.

Restart Jenkins again to apply changes.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733312175625/4a6f7b66-6a0b-4fa0-baaf-3eb4581766c9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733312185808/b4279900-482d-474b-90fd-f17f9b9488ee.png align="center")

---

## **Creating My First Declarative Pipeline Project**

Here’s where the magic begins:

1. **Create a New Pipeline Project:** Go to Jenkins and create a pipeline project.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733312260420/0eddc12f-cf12-4fc5-932e-9e2443d8b99b.png align="center")

2. **Add a** `Jenkinsfile`: Define your pipeline in a declarative format. The key part is using the **Docker agent**, so the pipeline runs inside a container.  
    
    ```bash
    pipeline {
      agent {
        docker { image 'node:16-alpine' }
      }
      stages {
        stage('Test') {
          steps {
            sh 'node --version'
          }
        }
      }
    }
    ```
    
3. **Build and Execute:** When you run the pipeline, Jenkins will:
    
    * Create a Docker container.
        
    * Execute the pipeline steps inside the container.
        
    * Delete the container after execution.
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733312291365/0d0cd61c-976c-433b-9f7c-8956d4bd0035.png align="center")

It creates docker container, executed pipeline and delete the docker container as well

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733312306117/e3459d29-f636-4c67-b315-3858ad70f017.png align="center")

---

## **Why This Setup is Awesome**

* **Scalable:** Each build runs in an isolated container, ensuring consistency.
    
* **Resource-Efficient:** Containers are lightweight and ephemeral.
    
* **Beginner-Friendly:** Jenkins’ GUI simplifies pipeline management.
    

---

### **My Results**

Here’s what I achieved:

* Jenkins successfully pulled and spun up a Docker container.
    
* The pipeline executed flawlessly within the container.
    
* The container was deleted after the build, keeping my setup clean.
    

(*Visuals for each step included in the images!*)

---

## **Key Takeaways**

Learning about CI/CD, Jenkins, and Docker agents might seem overwhelming initially, but breaking it into steps makes it manageable. This setup is the foundation for advanced pipelines and workflows.

If you're starting your DevOps journey, this is a great project to try out. Don’t forget to share your results in the comments or tag me on LinkedIn! 🚀