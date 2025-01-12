---
title: "Step-by-Step Guide to Creating an EKS Cluster with Terraform"
seoTitle: "Create EKS Cluster Using Terraform Guide"
seoDescription: "Learn to provision an EKS cluster with Terraform using this step-by-step guide, focusing on modularity, scalability, and industry best practices"
datePublished: Sun Jan 12 2025 15:20:13 GMT+0000 (Coordinated Universal Time)
cuid: cm5trhmnc000009me3z4eagk8
slug: step-by-step-guide-to-creating-an-eks-cluster-with-terraform
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736695080494/cff957a7-43b8-478a-9dcd-1d87cfdf711e.png
tags: cloud, ec2, docker, aws, kubernetes, developer, cloud-computing, devops, terraform, eks, devops-articles, 90daysofdevops, trainwithshubham, 90daysofdevops-chanllenge, tws

---

Infrastructure as Code (IaC) is a game-changer for automating and managing cloud resources. Recently, I worked on a Terraform project to provision an Amazon Elastic Kubernetes Service (EKS) cluster following industry standards. Here's a quick walkthrough of the project, its structure, and how you can apply the same to your own cloud environments.

---

## Why Terraform and EKS?

Managing cloud resources manually is time-consuming and error-prone. With Terraform, you can write reusable and modular configurations that scale effortlessly. Amazon EKS is a managed Kubernetes service that simplifies running Kubernetes workloads in the cloud. Together, they create a powerful combination for deploying and managing infrastructure with ease.

---

## Project Highlights

### 💡 Key Features

* **Modular Design**: The project uses Terraform modules for better reusability and maintainability.
    
* **Scalable Infrastructure**: Designed to support multiple node groups, public and private subnets, and secure NAT gateways.
    
* **Best Practices**: Adheres to industry standards, ensuring a well-architected solution.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736695096281/1894e702-cb2c-4034-9497-dc809d80525d.png align="center")

---

## Project Structure

The project is organized as follows:

```plaintext
Project_Terraform_eks/
├── .terraform/                     # Terraform state and configuration directory
├── modules/                        # Directory for Terraform modules
│   ├── aws_eks/                    # Module for creating the EKS cluster
│   ├── aws_eks_node_group/         # Module for managing EKS Node Groups
│   ├── aws_elastic_ip/             # Module for allocating Elastic IPs
│   ├── aws_internetGW/             # Module for creating the Internet Gateway
│   ├── aws_natGW/                  # Module for creating NAT Gateways
│   ├── aws_route_table/            # Module for configuring Route Tables
│   ├── aws_route_table_association/ # Module for associating Route Tables
│   ├── aws_subnets/                # Module for creating public and private subnets
│   ├── aws_vpc/                    # Module for creating the VPC
├── main.tf                         # Main Terraform configuration file
├── provider.tf                     # Provider configuration (e.g., AWS)
├── variables.tf                    # Input variables for the project
├── outputs.tf                      # Output values for the infrastructure
├── terraform.tfvars                # Variable values specific to this deployment
└── README.md                       # Documentation for the project
```

Each module focuses on a specific resource, making it easy to manage and scale the infrastructure.

---

## Resources Created

Here’s an overview of the resources provisioned in this project:

* **VPC** with public and private subnets.
    
* **Elastic IPs** for NAT Gateways.
    
* **NAT Gateways** to enable secure internet access for private subnets.
    
* **Route Tables** for network routing.
    
* **EKS Cluster** with multiple node groups to host workloads.
    

---

## Why Modularity Matters

Using modules ensures that each part of the infrastructure is reusable, easy to maintain, and scalable. For example:

* You can reuse the **VPC module** across multiple projects.
    
* Adding a new **node group** is as simple as updating a few variables in the EKS module.
    

---

## GitHub Repository

The entire codebase is available on my [GitHub Repository](https://github.com/iam-harshsoni/DevOps-Project/). Feel free to explore, fork, and contribute!

---

## Conclusion

This project highlights how Terraform can simplify provisioning complex cloud infrastructures like EKS. By adhering to best practices and modular principles, you can build scalable and maintainable solutions that save time and reduce errors.

If you’re diving into Terraform or Kubernetes, this project is a great starting point to explore best practices and practical implementations.

---

### 🌟 Share Your Thoughts

Have feedback or questions? Let me know in the comments below, or reach out to me on [LinkedIn](https://linkedin.com/in/harsh-soni-007hs). Let’s learn and grow together!