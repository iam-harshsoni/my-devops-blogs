---
title: "Step-by-Step Tutorial for Self-Hosted Runners in GitHub Actions"
seoTitle: "Self-Hosted Runners in GitHub Actions Guide"
seoDescription: "Learn to set up a self-hosted runner on AWS EC2 for customized, efficient GitHub Actions workflows with this step-by-step tutorial"
datePublished: Thu Dec 12 2024 12:47:55 GMT+0000 (Coordinated Universal Time)
cuid: cm4lbedst000f09kv0v7n8jto
slug: step-by-step-tutorial-for-self-hosted-runners-in-github-actions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734007345963/fca3bd41-c1a1-4cec-83c5-daee638ec800.png
tags: cloud, ec2, aws, github, kubernetes, developer, cloud-computing, devops, hashnode, jenkins, 2articles1week, github-actions-1, 90daysofdevops, 90daysofdevops-chanllenge, tws

---

## What Are Self-Hosted Runners?

A self-hosted runner is a server that you manage and configure to execute GitHub Actions workflows. Unlike GitHub-hosted runners that operate in the cloud, self-hosted runners give you complete control over the environment, allowing customization and optimization according to your specific needs.

### Key Benefits:

1. **Customization:** Install specific tools, dependencies, and configurations unique to your project.
    
2. **Performance:** Leverage dedicated resources for faster execution times.
    
3. **Cost Efficiency:** Use your existing infrastructure or cloud resources to save on costs.
    
4. **Flexibility:** Handle workloads that require unique hardware or software setups, such as GPU-enabled tasks.
    

---

## Why Use Self-Hosted Runners?

While GitHub’s hosted runners work well for many use cases, self-hosted runners shine in the following scenarios:

* **High Demand:** When workflows demand more CPU, memory, or disk space than the shared runners can provide.
    
* **Specific Tools:** When you need pre-installed tools, libraries, or dependencies that aren’t available in default runners.
    
* **Security & Compliance:** For workflows requiring private environments, such as on-premise servers.
    
* **Cost Control:** When running workflows extensively, hosting your runners can be more economical.
    

---

## Practical Setup: Self-Hosted Runner on AWS EC2

Here’s a step-by-step guide to setting up a self-hosted runner on an AWS EC2 instance.

### **Step 1: Create an EC2 Instance**

1. **Launch EC2 Instance:**
    
    * Choose an instance type (e.g., t2.medium for general use).
        
    * Select an appropriate AMI (Amazon Machine Image) like Ubuntu Server.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734006610783/391449c3-a39d-434c-8c5a-cc201bddb406.png align="center")
        
2. **Configure Inbound and Outbound Rules:**
    
    * Allow SSH access (port 22) to connect to the instance.
        
    * Open necessary ports for your workflow (e.g., port 443 for HTTPS).
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734006623164/04b41679-c7c8-4ddf-9dc4-85b0ff5cb314.png align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734006636297/66b331a2-556c-499e-a9b3-9660c7bfea09.png align="center")
        
3. **Connect to Your Instance:**
    
    * Use SSH to log in to your instance.
        

---

### **Step 2: Set Up the Self-Hosted Runner**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734006716152/bf216d9b-44d9-451c-8742-effef7a9c626.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734006729592/e0bfb920-91f4-4a9c-98f1-eb0698d246aa.png align="center")

Run all the commands one by one in your ec2 instance

---

### **Step 3: Update Your Workflow File**

Modify your workflow YAML file to use the self-hosted runner:

```yaml
name: CI Workflow

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: self-hosted
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Run Tests
        run: npm test
```

---

### **Step 4: Run and Verify**

1. Commit and push your changes to trigger the workflow.
    
2. Monitor the workflow execution in the GitHub Actions tab.
    
3. Confirm that the job is executed successfully using the self-hosted runner.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734006806641/be830024-b133-477d-957b-a6a3906ef4ec.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734006818212/b0debd56-8147-4b24-acad-4f1834fa2e15.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734006827495/68b53a94-4f29-4f01-a6a6-ab0f42765f77.png align="center")

---

## Wrapping Up

Self-hosted runners offer a powerful way to customize and optimize your GitHub Actions workflows. With complete control over the environment, you can run tasks that demand unique configurations or high performance. Setting up a self-hosted runner on an AWS EC2 instance is straightforward and provides flexibility for modern DevOps needs.

If you’ve set up your own self-hosted runner or have any questions, drop a comment below. Let’s discuss! 🚀