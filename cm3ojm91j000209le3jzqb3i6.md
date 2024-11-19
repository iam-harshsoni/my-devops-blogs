---
title: "Ansible Real-Time Project: Hands-On Implementation"
seoTitle: "Hands-On Ansible Real-Time Project"
seoDescription: "Learn Ansible by automating AWS EC2 tasks with loops, idempotency, and conditionals in this practical, hands-on project tutorial"
datePublished: Tue Nov 19 2024 14:21:36 GMT+0000 (Coordinated Universal Time)
cuid: cm3ojm91j000209le3jzqb3i6
slug: ansible-real-time-project-hands-on-implementation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1732025953560/59bf770c-8aa4-4cc6-b5fc-990848be5570.png
tags: cloud, docker, ansible, kubernetes, cloud-computing, devops, hashnode, jenkins, 2articles1week, ci-cd, 90daysofdevops, trainwithshubham, 90daysofdevops-chanllenge, tws

---

In this blog, I'll share a step-by-step guide for a real-time project I recently worked on using **Ansible**. The project involves automating tasks on AWS EC2 instances. If you're new to Ansible, this is an excellent way to understand key concepts like **loops**, **idempotency**, and **conditionals** in a practical scenario.

Let’s dive into the tasks!

---

## **Project Overview**

### **Tasks:**

1. **Create EC2 Instances**
    
    * Launch 3 EC2 instances on AWS using Ansible:
        
        * 2 with Ubuntu
            
        * 1 with CentOS  
            *(Hint: Use* `connection: local` on the control node)
            
2. **Set Up Passwordless Authentication**
    
    * Configure passwordless authentication between the Ansible control node and the newly created instances.
        
3. **Automate Shutdown**
    
    * Automatically shut down **Ubuntu instances only** using Ansible conditionals.
        

---

# **Task 1: Create EC2 Instances**

### **Step 1: Set Up AWS Credentials**

To interact with AWS APIs, Ansible needs access credentials. Instead of hardcoding sensitive information, use **Ansible Vault** to secure them.

### Secure AWS Credentials with Ansible Vault

1. **Generate a Vault Password:**  
    Use the following command to create a secure vault password file.
    
    ```bash
    openssl rand -base64 2048 > vault.pass
    ```
    
2. **Create a Vault File for AWS Credentials:**  
    Store your AWS Access Key and Secret Key in a YAML file. Use Ansible Vault to encrypt it.
    
    ```bash
    ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
    ```
    
    Inside ***pass.yml***, add your AWS credentials:
    
    ```bash
    aws_access_key: YOUR_ACCESS_KEY  
    aws_secret_key: YOUR_SECRET_KEY
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731063454943/9154b8f6-617b-4dea-b093-d60abfc7b54c.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731064923773/344fcbe9-ad00-4903-9475-378ce657918c.png align="center")
    

## Step 2: Write a Playbook to Create EC2 Instances

Using Ansible's **loop** feature, we can define multiple instances with different attributes (e.g., Ubuntu and CentOS).

#### **Key Concept: Idempotency**

Ansible ensures that tasks are only executed when necessary. If the desired state is already present, Ansible will skip execution. This is achieved by specifying properties like `image` and `name` in the loop.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732024369233/1c8e12b3-ef03-4135-9908-b363d1356172.png align="center")

Run the playbook with:

```bash
ansible-playbook ec2_create.yaml --vault-password-file vault.pass
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732024707361/16a925dc-565e-42ab-8fcc-17715fde3ac9.png align="center")

**Task 1: Done!** ✅

---

# Task 2: Set Up Passwordless Authentication

To enable seamless communication between the Ansible control node and the newly created instances:

* Set up SSH keys for passwordless authentication.
    
* Use one of the methods (e.g., `ssh-keygen`, `ssh-copy-id`) to configure it.
    

Once configured, you’ll be able to manage these instances without entering a password for every task.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732024668433/1dccb5ed-2ead-45a3-a6f2-4ce735094557.png align="center")

**Task 2: Done!** ✅

---

# **Task 3: Automate Shutdown of Ubuntu Instances**

### **Step 1: Update Inventory**

Add the public IPs of your newly created EC2 instances to the `inventory.ini` file. This file will act as your inventory source for Ansible.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732024773030/3fdcb7d7-5fb3-403f-88c4-9c5f4a02c15c.png align="center")

### **Step 2: Write the Playbook**

Use **conditionals** to target only Ubuntu instances. For example, leverage `ansible_facts` to filter by distribution type (`Ubuntu`).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732024824322/b4f5d9f0-1313-40f4-8e44-fc6386e6891e.png align="center")

Run the playbook with:

```bash
ansible-playbook -i inventory.ini ec2_stop.yaml --vault-password-file vault.pass
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732025025692/3659cdde-3402-4dc5-b79a-3429b4eeff2b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732024986604/6209ccdd-0347-4c09-a02a-3a71b87beff6.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732025042251/39d2f444-3167-45ee-abc2-9642de142a42.png align="center")

**Task 3: Done!** ✅

---

## **What I Learned**

1. **Idempotency:** Ansible ensures tasks aren’t repeated unnecessarily, making automation reliable.
    
2. **Loops in Playbooks:** Simplifies repetitive tasks by iterating over defined parameters.
    
3. **Conditionals in Ansible:** Filters actions based on specific criteria, enhancing task precision.
    
4. **Securing Credentials with Ansible Vault:** A crucial practice for production environments.
    

---

This project was an exciting way to put theory into practice, and I hope it inspires you to try automating your own tasks with Ansible. If you have questions or need help, feel free to reach out or comment below!

Happy Automating! 🚀