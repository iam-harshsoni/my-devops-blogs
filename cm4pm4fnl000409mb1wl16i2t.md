---
title: "Easy Steps to Build a Two-Tier Flask Application with Docker"
seoTitle: "Build Flask App with Docker: Easy Two-Tier Guide"
seoDescription: "Learn to build a two-tier Flask app with a MySQL database using Docker, featuring Docker Network and Volume for communication and persistence"
datePublished: Sun Dec 15 2024 12:59:12 GMT+0000 (Coordinated Universal Time)
cuid: cm4pm4fnl000409mb1wl16i2t
slug: easy-steps-to-build-a-two-tier-flask-application-with-docker
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734267434885/1181e0f0-e9fb-4cd7-901a-8838d8c33b63.png
tags: cloud, linux, docker, aws, javascript, python, kubernetes, developer, cloud-computing, devops, hashnode, jenkins, 2articles1week, ci-cd, docker-images

---

In this blog, we’ll walk through building a two-tier Flask app with a MySQL database using Docker. The project demonstrates two key Docker concepts: **Docker Network** and **Docker Volume**. These concepts are essential when dealing with multi-container applications, enabling communication and data persistence. Let’s dive in!

---

### What Are Docker Volume and Docker Network?

**Docker Volume**  
Volumes provide persistent storage for Docker containers. Think of them as a "data-safe zone" where files survive container deletions. For instance, you can store your database files in a volume so the data won’t disappear even if the database container stops or is removed.

**Docker Network**  
Networks allow Docker containers to talk to each other securely. For example, in this project, the Flask app and the MySQL database will use a shared network to communicate seamlessly.

---

### Setting Up the Two-Tier Flask App

Follow these steps to deploy the app:

#### **Step 1: Create an EC2 Instance**

* Launch an EC2 instance to serve as the environment for our Docker setup.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733298437915/5dba7be0-e8d7-4e4d-8298-1514c9a1eda8.png align="center")
    

#### **Step 2: Install Docker**

Run the following commands to install Docker on the EC2 instance:

```bash
sudo apt update
sudo apt install docker.io -y
```

#### **Step 3: Pull the MySQL Docker Image**

Fetch the MySQL image from the Docker Hub public registry and verify it:

```bash
docker pull mysql
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733296680882/a4975151-2b5c-4538-82b2-60eaf8024090.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733297056086/d2834440-0cb4-4a9c-8237-5190ca038cd5.png align="center")

#### **Step 4: Grant User Permissions**

Allow your user to access Docker without using `sudo` every time:

```bash
sudo usermod -aG docker $USER && newgrp docker
```

#### **Step 5: Reboot the System**

To apply the changes, restart your instance:

```bash
sudo reboot
```

#### **Step 6: Clone the Project**

Download the Flask app project files from a public repository.

#### **Step 7: Create a Dockerfile**

Set up a `Dockerfile` to containerize the Flask app.

```bash
# Use an official Python runtime as the base image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# install required packages for system
RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y gcc default-libmysqlclient-dev pkg-config \
    && rm -rf /var/lib/apt/lists/*

# Copy the requirements file into the container
COPY requirements.txt .

# Install app dependencies
RUN pip install mysqlclient
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
COPY . .

# Specify the command to run your application
CMD ["python", "app.py"]
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733296476804/d1de23b6-8e85-4d80-9388-702154315ee6.png align="center")

#### **Step 8: Build the Flask App Container**

Build the container with this command:

```bash
docker build -t two-tier-backend:latest .
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733297653810/8fa0d668-7de1-4dac-8818-97581d1a164e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733297671332/a296f810-b177-4a5c-9a46-3529d2a8fc2f.png align="center")

#### **Step 9: Create a Docker Network**

Create a bridge network for the app and database to communicate:

```bash
docker network create two-tier-nw
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733297390853/2de76e35-74c9-4a02-b105-0c1c637036bc.png align="center")

#### **Step 10: Run the Containers**

Run both containers (Flask and MySQL) on the same network.

1. **Run MySQL**  
    Start the MySQL container with environment variables:
    
    ```bash
    docker run -d --name mysql --network two-tier-nw \
    -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733297745462/706d23a6-e4c8-4335-96e1-48dbd5065973.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733297894250/a1597cf6-39e2-4187-b7c7-2160f75821a1.png align="center")
    
2. **Run Flask App**  
    Start the Flask app container with the required environment variables:
    
    ```bash
    docker run -d -p 5000:5000 --network two-tier-nw \
    -e MYSQL_HOST=mysql -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=root -e MYSQL_DB=devops two-tier-backend:latest
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733298091214/230f2cf4-1c03-4b91-86a1-35dad100756e.png align="center")
    

#### **Step 11: Update Security Group**

Allow inbound traffic on port **5000** in your EC2 security group.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733298340518/d2baf75b-ebcb-4e4a-9ccd-18c4d6dd082d.png align="center")

#### **Step 12: Access the App**

Open your browser and access the Flask app using the public IP of your EC2 instance:

```bash
http://<EC2_IP>:5000
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733298676213/19f5413b-5b04-45d7-bb0e-dfa2b004ad66.png align="center")

---

### The Data Loss Problem

If the MySQL container is stopped or deleted, the database data will be lost. This is because, by default, container storage is ephemeral—it doesn’t persist after the container is gone.

#### **Reproducing the Issue**

1. Stop and remove the MySQL container:
    
    ```bash
    docker stop mysql && docker rm mysql
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733298966960/11227a3e-0b28-4c4e-b475-51581a88e46b.png align="center")
    
2. Re-run the containers and check the app. You’ll notice that the database is empty again.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733299303927/bdb6ec93-eeb3-4b26-b521-0fbe52560ad0.png align="center")
    
    Access the site again - Data will get lost
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733299415001/4b8da486-1167-4552-a871-4a4f49b9909d.png align="center")
    

---

### The Solution: Docker Volumes

To ensure your data persists even after container crashes, bind a **Docker Volume** to the MySQL container. Volumes store data outside the container, on the host system.

#### **Step 1: Create a Volume**

Create a new Docker volume:

```bash
docker volume create mysql-data-backup
```

#### **Step 2: Bind the Volume to the MySQL Container**

Re-run the MySQL container, binding the volume to `/var/lib/mysql` (where MySQL stores its data):

```bash
docker run -d --name mysql --network two-tier-nw \
-v mysql-data-backup:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733300010887/a28f153e-7560-4614-970d-633f041cd4ba.png align="center")

#### **Step 3: Verify Data Persistence**

Now, even if the MySQL container is removed, the data will remain intact in the volume.

---

### Conclusion

This project showcases how Docker Volumes and Networks simplify multi-container application management. You learned how to:

1. Build and deploy a two-tier Flask app with MySQL.
    
2. Set up container communication using Docker Network.
    
3. Persist data with Docker Volume.
    

These concepts are foundational for anyone working with Docker in real-world applications. If you have any questions, feel free to ask in the comments!