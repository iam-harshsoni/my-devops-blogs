---
title: "A Beginner's Guide to CI/CD with Jenkins"
seoTitle: "Getting Started with Jenkins CI/CD"
seoDescription: "Learn how to set up and use Jenkins for CI/CD with this beginner-friendly guide, covering installation and basic pipeline creation"
datePublished: Sat Nov 30 2024 12:48:26 GMT+0000 (Coordinated Universal Time)
cuid: cm4464tj5000p09ladsze81cg
slug: a-beginners-guide-to-cicd-with-jenkins
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1732970800831/b486160f-8a24-47ac-82d2-cebdd78bb47e.png
tags: cloud, docker, aws, kubernetes, cloud-computing, devops, hashnode, containers, jenkins, 2articles1week, ci-cd, 90daysofdevops, 90daysofdevops-chanllenge, tws

---

# **What is Jenkins?**

* Jenkins is an open source continuous integration-continuous delivery and deployment (CI/CD) automation software DevOps tool written in the Java programming language. It is used to implement CI/CD workflows, called pipelines.
    
* Jenkins is a tool that is used for automation, and it is an open-source server that allows all the developers to build, test and deploy software. It works or runs on java as it is written in java. By using Jenkins we can make a continuous integration of projects(jobs) or end-to-endpoint automation.
    
* Jenkins achieves Continuous Integration with the help of plugins. Plugins allow the integration of Various DevOps stages. If you want to integrate a particular tool, you need to install the plugins for that tool. For example Git, Maven 2 project, Amazon EC2, HTML publisher etc.
    

## **Steps to Create a freestyle pipeline**

![](https://miro.medium.com/v2/resize:fit:1400/1*sUxhXLGgWZCAj4ot2gElDw.png align="left")

## Setting Up Jenkins on AWS EC2

### Step 1: Launch an EC2 Instance

First, I launched an **EC2 instance** (Ubuntu) on AWS and configured it as my Jenkins Master. Don’t forget to choose a security group that allows **port 8080** so we can access Jenkins via a browser later.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732970266023/89a37892-5326-4380-9c62-64303822da38.png align="center")

---

### Step 2: Install Java

Jenkins requires Java to run. Here’s how I installed it:

```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
```

After running the commands, verify the Java version to ensure it’s installed correctly.

---

### Step 3: Install Jenkins

Next, I installed Jenkins using these commands:

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

---

### Step 4: Enable and Restart Jenkins

To ensure Jenkins starts automatically after server reboots, I ran:

```bash
sudo systemctl enable jenkins
sudo systemctl restart jenkins
```

---

### Step 5: Access Jenkins

With Jenkins installed, I accessed it in my browser using the **public IP** of my EC2 instance and **port 8080** (e.g., `http://<your-ec2-public-ip>:8080`).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732970338932/04729b77-8256-43d2-bc5a-5e7855e617a7.png align="center")

---

### Step 6: Unlock Jenkins

Jenkins prompts for an initial admin password. Retrieve it using this command:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732970380368/96cd374a-3d0e-4179-9345-c77d8e64151f.png align="center")

After entering the password, click **Install Suggested Plugins** to get started quickly.

---

### Step 7: Create Admin User

Set up your admin account by filling in the required details. And voilà, Jenkins is ready!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732970398101/a906277e-43f8-4add-a02a-849f7b1951c7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732970430300/98be625c-2095-48a8-ad4a-3757654de030.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732970446996/550be57e-2f28-41c1-980d-1f81af5f216c.png align="center")

---

## Creating a Freestyle Project in Jenkins

Once Jenkins was running, I created my first **Freestyle Project** to automate some simple tasks.

### Step 1: Create a New Item

* Navigate to the Jenkins dashboard.
    
* Click **New Item** and name the project as “Freestyle-Project1.”
    
* Select **Freestyle Project** and click **OK**.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732970472562/70fedf7e-72a3-47a6-a503-547ea78dfdbf.png align="center")
    

---

### Step 2: Configure the Build Step

* In the project configuration page, scroll down to the **Build** section.
    
* Add an **Execute Shell** step and enter these commands:
    

```bash
echo "Hello World"
date
git clone https://github.com/<your-repo>.git
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732970495277/856215ce-8ead-4e49-a85e-a205ad3a144c.png align="center")

---

### Step 3: Run the Project

* Click **Build Now** to execute the project.
    
* Navigate to the **Console Output** to see the build logs.
    

You should see:

* A "Hello World" message.
    
* The current date.
    
* Successful cloning of the repository from GitHub.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732970518380/978f5552-9e89-425f-97cb-df0eada27b4f.png align="center")

---

## Wrap-Up

That’s it! 🎉 Setting up Jenkins and running a simple Freestyle Project was an exciting start to my CI/CD journey. If you’re new to Jenkins, this guide should help you get up and running quickly.

---

**Got questions? Drop them in the comments below. I’d be happy to help! 😊**

If you found this post helpful, please **like** ❤️ and **follow** for more beginner-friendly DevOps content.

Thank you for reading! 💚