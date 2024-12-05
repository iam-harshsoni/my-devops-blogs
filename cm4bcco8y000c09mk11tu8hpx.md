---
title: "Easy Guide to Multi-Agent Jenkins CI/CD Pipelines: Set Up with Docker"
seoTitle: "Jenkins CI/CD: Multi-Agent Pipelines with Docker"
seoDescription: "Set up scalable Jenkins CI/CD pipelines using Docker with this easy multi-agent and multi-stage guide for efficient resource utilization"
datePublished: Thu Dec 05 2024 13:16:54 GMT+0000 (Coordinated Universal Time)
cuid: cm4bcco8y000c09mk11tu8hpx
slug: easy-guide-to-multi-agent-jenkins-cicd-pipelines-set-up-with-docker
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733404510307/b0e7c4bb-a188-4fab-94ac-0b3cea54450f.png
tags: cloud, docker, aws, kubernetes, developer, cloud-computing, devops, hashnode, terraform, jenkins, 2articles1week, ci-cd, 90daysofdevops, 90daysofdevops-chanllenge, tws

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733402954698/1a650274-ef8d-4ec6-ba11-94283d06fbfe.png align="center")

## **Why Multi-Stage and Multi-Agent Pipelines?**

Multi-stage pipelines allow you to break down the CI/CD process into logical stages like building, testing, and deploying. Adding multi-agent functionality makes it scalable by assigning specific tasks to different environments (agents), ensuring better resource utilization and faster execution.

---

## **Step 1: Setting Up Jenkins**

### **1\. Install Java**

Jenkins requires Java to run. I installed OpenJDK 17 using the following commands:

```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
```

Ensure Java is installed by running `java -version`.

### **2\. Install Jenkins**

Download and install Jenkins with these commands:

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

### **3\. Enable Jenkins Service**

To make Jenkins run automatically after a system reboot, execute:

```bash
sudo systemctl enable jenkins
```

---

## **Step 2: Installing Docker and Configuring Permissions**

Docker is essential to enable containerized agents for Jenkins pipelines. Install Docker using:

```bash
sudo apt install docker.io -y
```

Grant your user permissions to run Docker commands without `sudo`:

```bash
sudo usermod -aG docker $USER && newgrp docker
```

### **Restart Jenkins and Docker Services**

To apply changes, restart both services:

```bash
sudo systemctl restart docker
sudo systemctl restart jenkins
```

---

## **Step 3: Open Port 8080 on EC2**

If you’re running Jenkins on an EC2 instance, ensure port `8080` is open in the security group’s inbound rules. This allows you to access Jenkins via your browser.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733403447340/cc143294-e3f0-4387-9806-732058bd32e4.png align="center")

---

## **Step 4: Configure Jenkins**

Access Jenkins at `http://<EC2_Public_IP>:8080`. During setup, install the required plugins.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733403467534/045baa71-f007-4d71-881a-cde830d218a4.png align="center")

### **Install Docker Pipeline Plugin**

This plugin is crucial to enable Jenkins pipelines to run inside Docker containers.

1. Navigate to **Manage Jenkins &gt; Manage Plugins**.
    
2. Search for **Docker Pipeline** and install it.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733403486396/763aecb8-1f3c-462d-8702-b7abc1ab184d.png align="center")

Restart Jenkins to apply the changes.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733403510438/e7c91ddf-a729-4638-a1e5-f75e4bda08e4.png align="center")

---

## **Step 5: Write the Multi-Stage, Multi-Agent Pipeline**

Here’s the Jenkinsfile I used for my project. It demonstrates a pipeline with multiple stages (Build, Test, Deploy) and leverages Docker as an agent:

```bash
pipeline {
  agent none
  stages {
    stage('Back-end') {
      agent {
        docker { image 'maven:3.8.1-adoptopenjdk-11' }
      }
      steps {
        script {
          // Create a simple Java Hello World application
          writeFile file: 'HelloWorld.java', text: '''
          public class HelloWorld {
              public static void main(String[] args) {
                  System.out.println("Hello, World from Java!");
              }
          }
          '''
          
          // Compile and run the Java application
          sh 'javac HelloWorld.java'
          sh 'java HelloWorld'
        }
      }
    }
    stage('Front-end') {
      agent {
        docker { image 'node:16-alpine' }
      }
      steps {
        script {
          // Create a simple Node.js Hello World application
          writeFile file: 'app.js', text: '''
          console.log("Hello, World from Node.js!");
          '''
          
          // Execute the Node.js application
          sh 'node app.js'
        }
      }
    }
  }
}
```

## **Key Benefits of This Pipeline**

1. **Isolation**: Each stage runs in a dedicated Docker container, ensuring clean and reproducible environments.
    
2. **Scalability**: Multi-agent design distributes tasks across different environments.
    
3. **Modularity**: Breaking processes into stages improves maintainability.
    

---

## **Step 6: Execute the Pipeline**

Save the `Jenkinsfile` in your project’s repository and create a new pipeline job in Jenkins. Configure the repository URL, and trigger the pipeline. Watch as Jenkins executes each stage using the Docker agents!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733403553657/b415613e-8480-4263-9fe8-432e82a08fef.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733403579835/9928e442-b18f-4677-be22-620c33ae3898.png align="center")

---

## **Conclusion**

By combining Jenkins and Docker, you can create powerful, modular CI/CD pipelines tailored to your project’s needs. This hands-on guide aimed to simplify the process and get you started with multi-stage, multi-agent pipelines quickly.

Feel free to share your experience or ask questions in the comments below! 🚀

Ready to dive deeper? Follow me for more practical DevOps tips!