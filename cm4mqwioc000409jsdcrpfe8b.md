---
title: "A Beginner’s Guide to Dockerfile: Building and Sharing Your First Docker Container"
seoTitle: "Dockerfile Basics: Build and Share Containers"
seoDescription: "Learn to build and share a Docker container with a step-by-step guide. Perfect for beginners creating and deploying Python web apps"
datePublished: Fri Dec 13 2024 12:49:42 GMT+0000 (Coordinated Universal Time)
cuid: cm4mqwioc000409jsdcrpfe8b
slug: a-beginners-guide-to-dockerfile-building-and-sharing-your-first-docker-container
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734094110825/376ed061-fb46-48e0-8ab3-4f634d1d736b.png
tags: cloud, docker, aws, kubernetes, cloud-computing, devops, hashnode, containers, jenkins, 2articles1week, ci-cd, docker-images, 90daysofdevops, 90daysofdevops-chanllenge, tws

---

## **What is a Dockerfile?**

A **Dockerfile** is a text file containing a series of commands and instructions used to build a Docker image. It serves as a blueprint for creating a container. With a Dockerfile, you can:

* Specify a base image (like Python or Node.js).
    
* Copy application files into the container.
    
* Install dependencies.
    
* Define what happens when the container starts (e.g., run a web server).
    

For example, if you’re creating a container for a Python application, the Dockerfile might tell Docker to use a Python base image, copy your application files, install required libraries, and start the application.

---

## **Task: Building and Sharing a Simple Python App with Docker**

Follow these steps to create, run, and share a Docker container for a Python app:

### Step 1: Create a Dockerfile for a simple application

* This is the simple python ‘Hello world’ application for which we are going to create Dockerfile
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733292384541/8c8a95ef-e760-4b20-8d85-b667ffb5c810.png align="center")
    
    Dockerfile
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733292410903/21886b64-a085-4697-bb99-007250633b9c.png align="center")
    
    ---
    
    ### **Step 2: Build the Docker Image**
    
    Use the Dockerfile to build a Docker image:
    
    ```bash
    docker build -t harshsoni777/my-first-docker-image
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733292431915/a8286207-f16f-4a72-a625-6734b9bdd506.png align="center")
    
    ---
    
    ### **Step 3: Run the Docker Container**
    
    Run the container using the newly built image:
    
    ```bash
    docker run -it harshsoni777/my-first-docker-image:latest
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733292455068/ca814c1d-cf1d-4faa-8461-a64f615a8e16.png align="center")
    
    ---
    
    ### **Step 5: Share the Image on Docker Hub**
    
    #### **Log in to Docker Hub**
    
    Before sharing the image, log in to your Docker Hub account:
    
    ```bash
    docker login
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733292471840/5e80a652-f701-4038-af4c-45afa6cb01d4.png align="center")
    
    #### **Push the Image**
    
    Push the image to Docker Hub:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733292494056/2812680f-6b44-40a6-8ce3-b4646987ff7c.png align="center")
    
    Your image is now available on Docker Hub and can be pulled by anyone using:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733292507443/91804cdc-1e04-485b-bdcb-75ff54ac7c23.png align="center")
    
    ---
    
    ## **Why Use a Dockerfile?**
    
    * **Consistency**: Ensure your app runs the same across all environments.
        
    * **Portability**: Share and deploy your app effortlessly.
        
    * **Efficiency**: Automate application setup and deployment.
        
    
    ---
    
    ## **Conclusion**
    
    With Docker and a simple Dockerfile, you can create, run, and share a fully functional containerized application. Whether you're developing a small web app or a large-scale system, Docker makes your work scalable, efficient, and collaborative.
    
    **Ready to try it yourself?** Build your first Docker image, push it to Docker Hub, and share your learning journey!