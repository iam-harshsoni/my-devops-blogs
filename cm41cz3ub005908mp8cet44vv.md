---
title: "Step-by-Step Guide to Migrating Resource Configurations to Terraform"
seoTitle: "Terraform Migration: A Step-by-Step Resource Guide"
seoDescription: "Migrate cloud resources to Terraform for consistent, scalable, version-controlled infrastructure management with this step-by-step guide"
datePublished: Thu Nov 28 2024 13:36:38 GMT+0000 (Coordinated Universal Time)
cuid: cm41cz3ub005908mp8cet44vv
slug: step-by-step-guide-to-migrating-resource-configurations-to-terraform
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1732801448961/fd681953-4833-43e0-a51b-b1385a72ee69.png
tags: cloud, ec2, docker, aws, github, kubernetes, cloud-computing, hashnode, terraform, jenkins, 2articles1week, ci-cd, 90daysofdevops, 90daysofdevops-chanllenge, tws

---

Hi there! 🌟

As I dive deeper into learning Terraform, I came across an interesting challenge: **migrating existing cloud resources into Terraform** for better management. I recently worked on this scenario and want to share what I learned and how I performed this hands-on task.

Here’s a step-by-step guide to what I did to make this happen.

---

### Why Manage Existing Resources with Terraform?

While manually created resources are fine for small setups, managing them at scale can become messy, especially if they were created using CFT and you or your organization is now switching to Terraform. That's where Terraform excels; it lets you manage everything as code! Migrating resources to Terraform helps with:

* Keeping configurations consistent.
    
* Automating updates and scaling.
    
* Tracking changes in a version-controlled way.
    

This sounded like a no-brainer to me, so I decided to give it a try.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732799622519/3273443b-d191-4ffb-9da5-bd4927c15a6f.png align="center")

---

### Steps to Migrate an Existing Resource

#### **Step 1: Create a Basic *main.tf*** File

Start by writing a ***main.tf*** file with the following structure

```bash
provider "aws" {  
  region = "ap-south-1"  
}  

import {  
  id = "i-0270a1d74d30244fd"  
  to = aws_instance.example  
}  
```

Here’s what’s happening:

* The **AWS provider** is configured with the desired region.
    
* The ***import*** block specifies the resource ID (***id***) of the existing EC2 instance and the Terraform resource block (***id***) where it will be imported.
    

---

#### **Step 2: Generate Resource Configuration**

Next, initialize Terraform and generate a configuration file for the existing resource using the following commands:

```bash
terraform init  
terraform plan -generate-config-out=generate_resources.tf  
```

This creates a ***generate\_***[***resources.tf***](http://resources.tf) file containing the full configuration of the existing resource.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732800594095/604a66d0-d5e6-4b10-a9f3-9b715a5c1e1b.png align="center")

---

#### **Step 3: Copy the Resource Block**

1. Open the ***generate\_***[***resources.tf***](http://resources.tf) file to view the generated configuration.
    
2. Copy the relevant resource block (for example, the EC2 instance details) and paste it into your ***main.tf*** file.
    
3. Once done, delete the ***generate\_***[***resources.tf***](http://resources.tf) file as it has served its purpose.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732800667839/3579cbe7-a7c5-43b8-8ec5-ae28f3a9c89d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732800678743/4591055a-362c-4696-9c4b-17b1f37b11d8.png align="center")

---

#### **Step 4: Handle the State File**

At this stage, if you run:

```bash
terraform plan  
```

Terraform will try to create a new resource instead of recognizing the existing one. This happens because there’s no **state file** to inform Terraform about the resource’s current state.

To fix this, run the following command to import the resource and create a state file:

```bash
terraform import aws_instance.example <instance_id> 
```

Here:

* `aws_instance.example` is the resource block defined in [`main.tf`](http://main.tf).
    
* `i-0270a1d74d30244fd` is the actual instance ID of the existing EC2 resource.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732800746524/4dd034f0-0708-4ea9-ba39-cbe2801fd43f.png align="center")

---

#### **Step 5: Verify the State**

Once the state file (`terraform.tfstate`) is generated, re-run:

```bash
terraform plan  
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1732800780414/e1d6bb75-efb1-4706-93a9-e62475d8d49e.png align="center")

Now, Terraform will show **“no changes”**, confirming that the existing resource is fully managed by Terraform.

---

### What I Learned

1. **Resource Import**: Terraform’s `import` command bridges the gap between manually created resources and IaC management.
    
2. **State Management**: The state file is crucial for Terraform to track resources. Without it, Terraform cannot recognize existing infrastructure.
    
3. **Configuration Generation**: Using `-generate-config-out` simplifies the process of fetching resource details, saving time when dealing with complex setups.
    

---

### Closing Thoughts

Migrating existing resources to Terraform was a rewarding experience! It made me realize how powerful Terraform can be for managing infrastructure consistently. If you’re exploring Terraform, I highly recommend trying this scenario—it’s a great way to practice importing and managing resources.

If you have any questions or thoughts, feel free to drop them in the comments. I’d love to hear about your Terraform journey too! 😊

Happy Terraforming! 🌍