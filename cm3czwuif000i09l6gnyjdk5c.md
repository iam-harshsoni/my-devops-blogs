---
title: "Easy Steps to Launch Your Node.js Application on AWS"
seoTitle: "Launch Node.js on AWS: Easy Guide"
seoDescription: "Learn how to deploy your Node.js app on AWS EC2 with our step-by-step guide. Perfect for beginners"
datePublished: Mon Nov 11 2024 12:24:30 GMT+0000 (Coordinated Universal Time)
cuid: cm3czwuif000i09l6gnyjdk5c
slug: deploy-nodejs-application-on-aws
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1731328126745/c9b57929-e183-4334-ae0a-3317340be14c.png
tags: aws, nodejs, aws-certified-solutions-architect-associate, step-by-step-guide-creating-and-configuring-an-auto-scaling-group-with-application-load-balancer-on-aws, step-by-step-guide, nodejs-on-aws, aws-ec2-nodejs, step-by-step-guide-to-deploying-nodejs-on-aws-ec2

---

### Testing on a Local System

Step 1: Clone the application to your local system

Step 2: Create an environment variables file named `.env` and specify the variables

Step 3: Install NPM packages

`npm install`

Step 4: Run the application on your local system

`npm run start`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730783503452/cea522be-4628-42b1-abde-68605672ac56.png align="center")

### Deploying on an AWS EC2 Instance

Now that the application is working well on your local system, it's time to deploy it on an AWS EC2 instance.

Step 1: Create an IAM user and assign a specific role to that user.

*Important: We are using a simple password here, but when working in an organization, it's highly recommended to use a secure password.*

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730783797693/5890ed18-2521-44e7-acfa-4cfaec3577b9.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730783991515/2dd9e676-0917-434f-8d7c-e7a4814c90c5.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730784072622/c61b5537-71b2-40b5-a4e0-edb3c57207b1.png align="center")

Step 2: Login as an IAM User using the credentials provided while creating the user.

Step 3: Create EC2 instance

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730785160010/16849fa3-64e4-4e81-a64c-f652aa14fc95.png align="center")

Step 4: Connect to this EC2 instance.

Step 5: Updating the outdated packaged and dependencies

`sudo apt-get update`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730785745131/5a75e4ea-1b5f-4ab7-a291-3fc5de9835b2.png align="center")

Step 6: install git on you install, to check where its already installed or not, type this command

`git --version`

if the git is installed on your ec2 instance then it will show the version of the git.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730785867709/a402b606-74ae-476e-8664-7f5d8e20db9d.png align="center")

Step 7: Configure Node.js and `npm`\- [Guide by DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-22-04)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730786366652/bec6f312-8065-44ec-8601-5c7bdefbad11.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730786386100/17864a4f-d5eb-4bbc-8ae7-e71621c2cde4.png align="center")

Step 8: Install npm

`sudo apt install npm`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730787709518/760fcae4-a14c-41b7-93a5-c2d12c87d820.png align="center")

Check npm —version, to see if its installed or not.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730787812603/d5bf8b01-4ede-40b3-a8d5-c000c4e03b2f.png align="center")

Step 9: Clone your application from git to the server using git clone &lt;url&gt; command. Cloned application on the server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730787944421/7ff9f9af-18a1-4d5d-a7e1-2a847b7fcac9.png align="center")

Step 10: Setting up Environment Variables file, what I have done on local machine

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730788275545/d525cab7-9089-4000-bac6-6cba0e69b340.png align="center")

Step 11: Install NPM

`npm install`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730788479738/ebe48dca-5ecb-4535-acea-eef0224b5da8.png align="center")

Step 12: Run the application on local system

`npm run start`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730788498719/a4e7ced3-2910-467a-b7d0-519df42c4085.png align="center")

Step 13: Update the Security Group of the EC2 instance to open port 3000, allowing access to the application running on that port.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730788626408/36c04dfb-437f-4077-a05d-f18baed37439.png align="center")

Step 14: Copy the public IP address of the EC2 instance and the port number where the application is running. Then, try to access the application.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1730788712218/c32fe936-e130-42d6-8f33-0c20a00a4238.png align="center")