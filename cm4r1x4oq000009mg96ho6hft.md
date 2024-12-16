---
title: "How to Build a 3-Tier App with Docker Compose: A Step-by-Step Guide"
seoTitle: "Three-Tier App with Docker Compose"
seoDescription: "Build a three-tier app using Docker Compose through a step-by-step expense-tracker project"
datePublished: Mon Dec 16 2024 13:09:11 GMT+0000 (Coordinated Universal Time)
cuid: cm4r1x4oq000009mg96ho6hft
slug: how-to-build-a-3-tier-app-with-docker-compose-a-step-by-step-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734353797852/e4684ad2-b68e-43cf-b3bd-a829a6abad27.png
tags: cloud, docker, aws, kubernetes, developer, cloud-computing, devops, jenkins, docker-compose, ci-cd, docker-images, devops-articles, 90daysofdevops, trainwithshubham, 90daysofdevops-chanllenge

---

Setting up a three-tier application (frontend, backend, and database) using Docker Compose is a great way to streamline deployment and management. In this blog, we’ll walk through building a sample expense-tracker application step by step. Let’s dive in!

---

## **Step 1: Create an EC2 Instance**

Start by creating an EC2 instance in your preferred cloud environment. Make sure you have the necessary inbound security group rules to allow HTTP (port 80) and your application port (port 8080).

---

## **Step 2: Install Dependencies**

On your EC2 instance:

* Install Docker and Docker Compose:
    
    ```bash
    sudo apt update
    sudo apt install docker.io docker-compose -y
    ```
    
* Grant Docker permissions to your user:
    
    ```bash
    sudo usermod -aG docker $USER
    ```
    
* Install NGINX (optional, for reverse proxy) and MySQL:
    
    ```bash
    sudo apt install nginx mysql-client -y
    ```
    
    ---
    
    ## **Step 3: Clone Your Project**
    
    Clone the project repository into your local directory:
    
    ```bash
    git clone <your-repo-url>
    cd <your-project-folder>
    ```
    
    ---
    
    ## **Step 4: Write a Dockerfile**
    
    Here’s a sample Dockerfile for a Java-based backend:
    
    ```bash
    # Stage 1  -  jar(JAVA APPLICATION RUNTIME) builder USING MAVEN
    
    # Maven Image
    FROM maven:3.8.3-openjdk-17 AS builder
    
    # Set working directory
    WORKDIR /ap
    
    # Copy souce code from local to container
    COPY . /app
    
    # Build the application, Create jar file and skip the tests
    RUN mvn clean install -DskipTests=true
    
    #-------------------------------------------------------------------
    
    # Stage 2 - Execute jar file from the above stage
    
    FROM openjdk:17-alpine
    
    WORKDIR /app
    # Copy build (jar file) from stage 1 (builder)
    # expenseapp.jar where (expenseapp) is the name of the application we can find in application.properties
    
    COPY --from=builder /app/target/*.jar /app/target/expenseapp.jar
    
    # Expose application port
    EXPOSE 8000
    
    # Start the application
    ENTRYPOINT ["java", "-jar", "/app/target/expenseapp.jar"]
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734354113170/45285cf3-30ed-4fd7-bfbb-53b76dca511f.png align="center")
    
    Use this to containerize your Java application.
    
    ---
    
    ## **Step 5: Build the Application**
    
    Run the following command to build your Docker image:
    
    ```bash
    docker build -t expense-app .
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734354147038/6ef8cb52-39b3-4615-a59a-26cb0c400f66.png align="center")
    
    ---
    
    ## **Step 6: Create a Docker Compose File**
    
    Now, set up a `docker-compose.yml` file to define and orchestrate your services:
    
    ```yaml
    version: "3.8"
    services:
      mysql:
        image: mysql:latest
        container_name: mysql
        environment:
          MYSQL_ROOT_PASSWORD: Test@123
          MYSQL_DATABASE: expenses_tracker
        volumes:
          - ./mysql-data:/var/lib/mysql
        networks:
          - appbridge
        healthcheck:
          test: ["CMD","mysqladmin","ping","-h","localhost","-uroot","-pTest@123"]
          interval: 10s
          timeout: 5s
          retries: 5
          start_period: 30s
        restart: always
    
      mainapp:
        image: snehcreate/expensetracker_v3
        container_name: Expensetracker
        environment:
          SPRING_DATASOURCE_USERNAME: root
          SPRING_DATASOURCE_URL: "jdbc:mysql://mysql:3306/expenses_tracker?allowPublicKeyRetrieval=true&useSSL=false"
          SPRING_DATASOURCE_PASSWORD: Test@123
        ports:
          - "8080:8080"
        networks:
          - appbridge
        depends_on:
          - mysql
        restart: always
    
    networks:
      appbridge:
    
    volumes:
      mysql-data:
    ```
    
    ---
    
    ## **Step 7: Run Docker Compose**
    
    Spin up your application with:
    
    ```bash
    docker compose up -d
    ```
    
    This command starts your MySQL database and Java backend in separate containers, all connected via the `appbridge` network.
    
    ---
    
    ## **Step 8: Configure Security Group**
    
    Update your EC2 instance’s security group to allow traffic on port 8080 (the application port).
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734354192250/7ad9ea1c-2068-48b0-87c5-6a27362983bd.png align="center")
    
    ---
    
    ## **Step 9: Access the Application**
    
    Open your browser and visit:
    
    ```bash
    http://<your-ec2-public-ip>:8080
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734354216099/3b24affa-1daf-49bd-aa8b-01f8b7c24090.png align="center")
    
    ---
    
    ## **Troubleshooting: Database Connection Issues**
    
    If your application fails to connect to MySQL, double-check the environment variable `SPRING_DATASOURCE_URL`. Here’s the correct format:
    
    ```bash
    SPRING_DATASOURCE_URL: "jdbc:mysql://mysql:3306/expenses_tracker?allowPublicKeyRetrieval=true&useSSL=false"
    ```
    
    The key addition here is `allowPublicKeyRetrieval=true&useSSL=false`, which resolves common MySQL connection issues.
    
    ---
    
    ### **Final Thoughts**
    
    Congratulations! You’ve successfully built and deployed a three-tier application using Docker Compose. This setup makes it easier to manage services and scale as needed. Feel free to customize this example for your use case.
    
    If you found this guide helpful, don’t forget to share it with your peers and leave your thoughts in the comments! 🚀