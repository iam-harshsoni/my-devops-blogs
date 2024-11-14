---
title: "Implementing Passwordless Authentication with Ansible"
seoTitle: "Passwordless Authentication via Ansible"
seoDescription: "Learn to set up SSH key and SSH-copy-id passwordless authentication using Ansible for secure, efficient access to remote systems"
datePublished: Thu Nov 14 2024 13:49:14 GMT+0000 (Coordinated Universal Time)
cuid: cm3hd9dq3000m09jkbzrs79u3
slug: implementing-passwordless-authentication-with-ansible
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1731588772964/19a68a66-8ba9-4cd3-a767-81b3109ce1e6.png
tags: cloud, authentication, docker, aws, security, ansible, kubernetes, cloud-computing, devops, terraform, jenkins, ci-cd, 90daysofdevops, trainwithshubham, tws

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731588820996/a4643d64-eedd-41dd-b049-c3772d7fb91b.png align="center")

In this blog, I explore three common methods of setting up passwordless authentication: using **SSH keys**, **SSH-copy-id**, and **password-less** configurations. Passwordless authentication is a secure, efficient way to access remote systems without entering passwords each time, enhancing both convenience and security. I’ll walk through each method, breaking down the setup process for seamless, password-free logins.

I have created 3 EC2 instances on AWS and I will use my local machine as a control node to perform password-less authentication on all the instances

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731589355218/39432b50-adef-4b25-b01b-9061e7e621c6.png align="center")

# Using SSH Keys

Lets try to perform passwordless authentication on ‘manage-node-1’ using ssh-keys.

I have created private and public keys using this below command

```bash
ssh-keygen
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731589547849/b9e420da-7dd2-4faa-ad52-85792c0f62f2.png align="center")

it will create .ssh folder and that folder contains private and public key

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731589656370/d0c7242f-ae64-4ab1-b016-ad2ff23dc476.png align="center")

Connect to ***manage-node-1*** from the AWS console. Copy the control node's public key and paste it into the ***authorized\_keys*** file on ***manage-node-1***.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731589889521/4f4dc0b2-6e7d-47d0-8e5a-d96117bd025e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731590183136/e860bff7-d1bf-4fbb-a559-867c7562059e.png align="center")

Try to log in to ***manage-node-1*** without using a password or any keys.

```bash
ssh ubuntu@<public ip address>
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731590348385/da568786-67c3-48b9-8437-be3fbfb9f803.png align="center")

Login Successful 🎉

# Using SSH-copy-id

Copy the key pair to a safe directory. We will use this key pair to perform passwordless authentication on manage-node-2.

Note: This keypair.pem should be the same one used during the creation of manage-node-2.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731590410854/c9a2ea82-1b3c-40bb-8cce-43bb0588095b.png align="center")

```bash
ssh-copy-id -f "-o Identityfile <path_of_keypair.pem>" [user@]hostname
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731590754172/9508bf54-b6ea-44c3-a87e-04eb3431aea0.png align="center")

Try to log in to ***manage-node-2*** without using a password or any keys.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731590799994/b16df458-5a32-41a1-8d3e-810ce6a27aac.png align="center")

Login Successful 🎉

# Using Password Configuration

Connect to ***manage-node-3 using* AWS Console.** Edit this file ‘60-cloudimg-settings.conf” and update ‘PasswordAuthentication yes’

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731591045848/2dae3230-0a68-423d-b732-2a79b22b5109.png align="center")

Restart ssh process

```bash
sude systemctl restart ssh
```

Create a password for current user

```bash
sudo passwd ubuntu
New Pass:
Retype Pass:
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731591255588/25b9331f-d299-4c64-9124-ccd71d9f37af.png align="center")

Now, try to connect ***manage-node-3*** from control-node ( laptop ) using ssh-copy-id &lt;username@public ip&gt;

You need to enter the password the first time you connect to manage-node-3. After that, when you try to connect again, it won't ask for a password.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731591652628/12bf3a07-1f70-43b7-abfe-2edc0bcb8d9e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731591686858/90fd74c8-d188-4e60-a1fd-140204e2af5c.png align="center")

Login Successful 🎉

# Conclusion

Setting up passwordless authentication between control and managed nodes enhances both security and efficiency. This method simplifies node management, allowing you to automate tasks securely and focus on more complex DevOps operations.

---

*Join the community of learners and be a part of the conversation! Your feedback is valuable to us, so please share your thoughts in the comments section. Help us make this blog even better for everyone. And if you found this post helpful, spread the word! Share it with those who could benefit from the information. And don't forget to follow along and subscribe to our newsletter for instant updates on our latest content. Thank you for taking the time to read and engage with us!*