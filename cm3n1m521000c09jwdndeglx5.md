---
title: "Create Resources on AWS Using Ansible"
seoTitle: "AWS Resource Automation with Ansible"
seoDescription: "Automate AWS EC2 instances with Ansible: learn prerequisites, set up roles, and execute playbooks in this beginner guide"
datePublished: Mon Nov 18 2024 13:09:51 GMT+0000 (Coordinated Universal Time)
cuid: cm3n1m521000c09jwdndeglx5
slug: create-resources-on-aws-using-ansible
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1731935188924/bae39844-d3d6-4da9-98ac-7d9d3a7cc0a9.png
tags: cloud, ec2, docker, aws, python, ansible, kubernetes, cloud-computing, automation, devops, hashnode, 2articles1week, shubhamlondhe, trainwithshubham, tws

---

If you're starting with cloud automation using Ansible, setting up AWS resources is a great project to try. In this blog, I'll show you how I created an AWS EC2 instance with Ansible. This guide is easy to follow and meant for anyone who knows the basics of Ansible and AWS.

## Prerequisites

Before starting, ensure you have the following:

1. An AWS account.
    
2. Ansible installed on your local machine.
    
3. Python installed, along with the ***boto3*** module.
    

## Step 1: Install the Required Collections and Libraries

Ansible uses collections to support specific platforms or tools. For AWS, you'll need the [`amazon.aws`](http://amazon.aws) collection and the Python ***boto3*** module.

### Install AWS Collection:

```bash
ansible-galaxy collection install amazon.aws  
```

### Install Boto3 Module:

```bash
pip install boto3  
```

## Step 2: Create a Role for EC2 Management

Roles in Ansible help organize tasks, making your playbooks modular and reusable. Start by creating a role for managing EC2 instances.

```bash
ansible-galaxy role init ec2  
```

This command creates a directory structure where you can add your tasks, variables, and other configurations.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731064798837/2883078c-15c9-4bf4-ab14-293712386b79.png align="center")

## Step 3: Set Up AWS Credentials

To access AWS APIs, you'll need to use an **Access Key** and **Secret Key**. However, storing these keys securely is critical. Here's how you can handle it safely using Ansible Vault.

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
    

## Step 4: Create the Playbook

Now, let’s write the playbook to create an EC2 instance. Below is an example of a playbook file, ***ec2\_create.yaml***:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731064816536/b7a3fb28-e043-4459-840d-4d3ba222af2f.png align="center")

## Step 5: Execute the Playbook

With everything set up, it's time to run the playbook and create your EC2 instance!

```bash
ansible-playbook ec2_create.yaml --vault-password-file vault.pass  
```

Once executed, Ansible will launch the EC2 instance, and you’ll see the instance ID in the output.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731064597010/78e2f3e7-e29e-4793-921c-85a9f02ad10d.png align="center")

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731064559083/d8ad44ce-2263-4877-bce4-f6e3176cb0bd.png align="center")

## Success!

Congratulations, you've successfully created an EC2 instance on AWS using Ansible. 🎉

This simple example can be a starting point for automating more complex infrastructure setups. With Ansible's power and AWS's scalability, the possibilities are endless.

Let me know in the comments if you tried this or if you have any questions!

---

Happy automating! 🚀