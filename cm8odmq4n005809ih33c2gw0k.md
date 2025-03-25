---
title: "Hosting a Django Application on AWS EC2: A Step-by-Step Guide"
seoTitle: "Deploy Django on AWS EC2: Easy Guide"
seoDescription: "Learn how to deploy your Django app on AWS EC2 with this comprehensive step-by-step guide for beginners"
datePublished: Tue Mar 25 2025 10:52:32 GMT+0000 (Coordinated Universal Time)
cuid: cm8odmq4n005809ih33c2gw0k
slug: hosting-a-django-application-on-aws-ec2-a-step-by-step-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1742899872981/fecff86a-cd74-444e-a87f-719fee619488.png
tags: cloud, ec2, docker, aws, python, django, web-development, kubernetes, apis, devops, rds, shubhamlondhe, trainwithshubham, s3-bucket

---

**Introduction:**  
Are you ready to launch your Django application into the world? Hosting your app on AWS EC2 is a powerful solution that ensures scalability and performance. In this guide, I'll walk you through the process of deploying your Django app on an EC2 instance, keeping it simple and beginner-friendly.

---

### **Step-by-Step Guide to Hosting Django on EC2**

**Step 1: Create an EC2 Instance**  
Head over to AWS EC2 and spin up a new instance. Choose the Ubuntu AMI, configure your instance type, security groups, and launch your virtual server.

**Step 2: SSH into the EC2 Instance**  
Access your instance securely using SSH:

```bash
ssh -i your_key.pem ubuntu@your_ec2_public_ip
```

Once connected, run:

```bash
sudo apt-get update -y && sudo apt-get upgrade -y
```

to ensure your instance is up-to-date.

**Step 3: Set Up Your Environment**

1. Clone your Django application repository:
    

```bash
git clone your_repo_url
```

2. Install Python's package manager, pip:
    

```bash
sudo apt install python3-pip
```

3. Install the virtual environment module:
    

```bash
sudo apt install python3.12-venv
```

4. Create a virtual environment:
    

```bash
python3 -m venv your_env_name
```

5. Activate your virtual environment:
    

```bash
source your_env_name/bin/activate
```

6. Install your app's dependencies:
    

```bash
pip install -r requirements.txt
```

**Step 4: Database and Migrations**  
Set up a PostgreSQL database on AWS RDS, configure inbound rules in your EC2 security group, and add the details to your `.env` file. Then, run:

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

**Step 5: Install and Configure Nginx**

1. Install Nginx:
    

```bash
sudo apt install nginx -y
```

2. Enable and start Nginx:
    

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

3. Configure Nginx by creating a new file:
    

```bash
sudo nano /etc/nginx/sites-available/your_project
```

Paste this config:

```nginx
server {
    listen 80;
    server_name your_domain_or_public_ip;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location /static/ {
        alias /path/to/static/files/;
    }

    location /media/ {
        alias /path/to/media/files/;
    }
}
```

4. Enable your configuration:
    

```bash
sudo ln -s /etc/nginx/sites-available/your_project /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

**Step 6: Serve Static Files on S3**  
Set up an S3 bucket to store your static and media files. Run this command to collect static files:

```bash
python3 manage.py collectstatic
```

**Step 7: Set Up Gunicorn**  
Install Gunicorn:

```bash
pip install gunicorn
```

Serve your app:

```bash
gunicorn --bind 0.0.0.0:8000 your_project.wsgi:application
```

**Step 8: Update EC2 Inbound Rules**  
Add your application’s port in AWS EC2 inbound rules to ensure traffic is allowed.

**Step 9: Access Your Site**  
Visit your EC2 instance's public IP in the browser, and your Django application is live!

---

**Conclusion:**  
Hosting your Django application on EC2 may seem like a daunting task at first, but with this simple guide, you’re set for success. Scale as you grow, experiment with AWS's vast offerings, and enjoy the limitless possibilities of cloud hosting.

---

If this guide helped you, share it with your friends and fellow developers! Have questions? Drop a comment below, and I'll be happy to help.

---