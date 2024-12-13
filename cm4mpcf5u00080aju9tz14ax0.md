---
title: "Beginner’s Docker Tutorial: How to Deploy on AWS EC2"
seoTitle: "Deploy Docker on AWS EC2: Beginner's Guide"
seoDescription: "Beginner's tutorial on deploying Docker containers on AWS EC2, including installation and essential Docker commands for easy container management"
datePublished: Fri Dec 13 2024 12:06:05 GMT+0000 (Coordinated Universal Time)
cuid: cm4mpcf5u00080aju9tz14ax0
slug: beginners-docker-tutorial-how-to-deploy-on-aws-ec2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734091483720/1342025e-83a9-4ee8-a5b5-0ee00c4508d7.png
tags: docker, aws, kubernetes, developer, devops, hashnode, containers, jenkins, 2articles1week, ci-cd, 90daysofdevops, 90daysofdevops-chanllenge, tws

---

### **What is Docker?**

Docker is a powerful software platform that enables developers to package applications into standardized units called **containers**. A container includes everything an application needs to run—code, runtime, libraries, and system tools—ensuring that your application runs seamlessly in any environment. With Docker, you can:

* **Quickly Deploy and Scale**: Run applications efficiently across multiple platforms.
    
* **Ensure Reliability**: Guarantee that your code works identically in development, testing, and production environments.
    

---

Here's a quick command to get started:

```bash
bashCopy codedocker run hello-world
```

This command starts a new container and lets you interact with it through the command line, showcasing the simplicity of Docker.

---

### **Installing Docker on an AWS EC2 Instance**

Installing Docker on an AWS EC2 instance is straightforward. Follow these steps to get started:

#### **Step 1: Create an EC2 Instance**

* Launch an EC2 instance using AWS Management Console.
    
* Choose an Ubuntu-based AMI for simplicity.
    

#### **Step 2: Connect to the EC2 Instance**

* Use SSH to connect to your EC2 instance:
    

```bash
bashCopy codessh -i your-key.pem ubuntu@your-ec2-instance-ip
```

#### **Step 3: Install Docker**

Run the following commands to install Docker:

```bash
bashCopy codesudo apt-get update
sudo apt install docker.io -y
```

#### **Step 4: Grant Docker Daemon Permissions**

Add your user to the Docker group to enable non-root access:

```bash
bashCopy codesudo usermod -aG docker ubuntu
```

#### **Step 5: Restart the Instance**

Reboot the instance to apply permission changes:

```bash
bashCopy codesudo reboot
```

#### **Step 6: Verify Docker Installation**

After rebooting, check if Docker is installed and running:

```bash
bashCopy codedocker run hello-world
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733290648467/5a48cde7-4bf1-4994-8c1a-2d7022661abb.png align="center")

---

### **Key Docker Commands for Managing Containers and Images**

Mastering Docker involves understanding its versatile command-line tools. Here are some essential commands to help you manage containers and images:

#### **1\. Inspect Containers and Images**

Use the `docker inspect` command to view detailed information about a container or image:

```bash
docker inspect container_name_or_id
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733290896695/36c5f433-8dcd-4830-8208-3161996f5bef.png align="center")

#### **2\. List Port Mappings**

To check the port mappings for a container, use:

```bash
docker port container_name_or_id
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733290896695/36c5f433-8dcd-4830-8208-3161996f5bef.png align="center")

For example:

```bash
docker run -p 8181:82 container_name
docker port container_name
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733291076606/64bb34ac-c6b2-429b-a742-2dffa4541b46.png align="center")

#### **3\. Monitor Resource Usage**

Track real-time resource usage of running containers:

```bash
docker stats
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733291205028/04c90d74-a149-4614-8956-2cc5eddc3d3d.png align="center")

#### **4\. View Running Processes**

To see the processes inside a specific container:

```bash
docker top my_container2
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733291342235/d4912f60-b74c-4587-aaa9-ba8478740465.png align="center")

#### **5\. Save and Load Images**

* Save an image to a tar archive:
    

```bash
docker save -o my_image.tar nginx
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733292161684/143a4073-4e60-44e3-8fea-e77381f2bb70.png align="center")

* Load an image from a tar archive:
    

```bash
docker load -i my_image.tar
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733292244138/fde6da9b-c3bc-4810-ba16-6321f3a61c32.png align="center")

---

### **Why Learn Docker?**

Docker has become a cornerstone of modern application development and deployment. Its portability, scalability, and ease of use make it a favorite for developers and DevOps professionals alike. Whether you're deploying applications to the cloud or optimizing local development workflows, Docker empowers you to deliver reliable and efficient solutions.

---

### **Conclusion**

Docker is more than just a tool—it's a gateway to modernizing application development and deployment. By installing Docker on an AWS EC2 instance and mastering essential commands, you take the first steps toward becoming proficient in containerization and cloud-based deployments. Start your Docker journey today, and unlock a world of possibilities in software engineering!

**Ready to dive deeper?** Share your experience with Docker or ask questions in the comments below. Let’s build a vibrant community of learners and innovators!