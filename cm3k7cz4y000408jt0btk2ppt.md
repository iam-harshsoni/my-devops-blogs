---
title: "Learn Ansible Roles: An Easy Guide for Beginners with Real Examples"
seoTitle: "Beginner's Guide to Ansible Roles with Examples"
seoDescription: "Learn Ansible Roles: Structure, create, and execute roles for efficient automation with real examples in this beginner-friendly guide"
datePublished: Sat Nov 16 2024 13:27:23 GMT+0000 (Coordinated Universal Time)
cuid: cm3k7cz4y000408jt0btk2ppt
slug: learn-ansible-roles-an-easy-guide-for-beginners-with-real-examples
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1731763295754/7713df64-7259-401d-a9f2-67e4cf25edec.png
tags: cloud, docker, aws, ansible, kubernetes, cloud-computing, devops, hashnode, 2articles1week, devops-articles, devops-journey, 90daysofdevops, trainwithshubham, tws, devopscommunity

---

Ansible Roles provide a structured way to organize playbooks into reusable and maintainable units. Roles allow you to separate configurations, tasks, variables, files, and templates into distinct directories, making complex automation workflows more manageable.

In this blog, I’ll share how I created and executed Ansible Roles during my hands-on learning experience.

### 1\. **Creating Roles**

Roles can be created easily using the `ansible-galaxy` command:

```bash
ansible-galaxy role init httpd
```

This command generates a directory structure for the role `httpd`, which includes subdirectories like `tasks`, `vars`, `files`, `templates`, and more.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731044189541/80c160cb-9c19-4e54-8fbd-6f9bc353b65e.png align="center")

### 2\. **Adding Tasks**

Once the role is created, the core logic resides in the `tasks/main.yaml` file. For example, to deploy a simple web server:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731045421838/07777a4d-c57b-4b26-978c-3a4a5e7be584.png align="center")

Here, the tasks include installing Apache, starting the service, and deploying a sample `index.html` file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731046201497/0dc82155-017d-408a-a0d8-160fecde5b26.png align="center")

### 3\. **Using the Role in a Playbook**

After defining the role, it’s time to integrate it into an Ansible playbook. Add the role to the `roles` section of the playbook:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731045361718/e94d9056-c231-49eb-855f-dff63cee984f.png align="center")

### 4\. **All-in-One Approach**

By using roles, you encapsulate everything needed for specific automation into reusable components. This modularity is especially useful for managing configurations across multiple environments.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731045496283/8a4a22db-fb30-46ee-8c96-b07932a90599.png align="center")

### 5\. **Executing the Playbook**

Finally, execute the playbook using the `ansible-playbook` command:

```bash
ansible-playbook -i inventory.ini first-playbook.yaml
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731045624173/81e2f7eb-02ac-4c45-afea-9ffa572a7719.png align="center")

Successful 🎉

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731046155287/f877d236-9c9b-442a-99ac-1c87a974dadf.png align="center")

### Key Takeaways:

* **Structured Organization**: Roles make it easy to maintain and scale your Ansible projects.
    
* **Reusability**: Once written, a role can be reused across multiple projects and playbooks.
    
* **Modularity**: Encapsulation of logic and configurations ensures a cleaner playbook structure.
    

I hope this blog helps you understand the basics of Ansible Roles. Feel free to share your thoughts or ask questions in the comments!

---

*Join the community of learners and be a part of the conversation! Your feedback is valuable to us, so please share your thoughts in the comments section. Help us make this blog even better for everyone. And if you found this post helpful, spread the word! Share it with those who could benefit from the information. And don't forget to follow along and subscribe to our newsletter for instant updates on our latest content. Thank you for taking the time to read and engage with us*