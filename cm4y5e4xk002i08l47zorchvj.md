---
title: "Secure Your Sensitive Information Using HashiCorp Vault and Terraform"
seoTitle: "HashiCorp Vault: Secure Data with Terraform"
seoDescription: "Learn to integrate HashiCorp Vault with Terraform for secure management of sensitive information, including step-by-step guidance and best practices"
datePublished: Sat Dec 21 2024 12:20:47 GMT+0000 (Coordinated Universal Time)
cuid: cm4y5e4xk002i08l47zorchvj
slug: secure-your-sensitive-information-using-hashicorp-vault-and-terraform
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734707670354/ca9f0852-da1e-4692-83ba-50cfa33e419b.png
tags: cloud, docker, aws, security, ansible, kubernetes, developer, cloud-computing, devops, terraform, devops-articles, hashicorp-vault, 90daysofdevops, trainwithshubham, 90daysofdevops-chanllenge

---

### Handling sensitive information like API keys, credentials, and tokens is one of the biggest challenges when managing cloud infrastructure with Terraform. Without proper precautions, secrets can end up in plaintext in state files or source code, exposing them to potential risks.

In this blog, we’ll explore how to integrate HashiCorp Vault with Terraform for securely managing sensitive information. We’ll walk through a step-by-step guide, provide real-world examples, and share best practices.

---

### Challenges in Managing Sensitive Information

Sensitive data in Terraform configurations, such as API keys and tokens, can inadvertently be exposed if not handled securely. Common issues include:

* **Plaintext in State Files:** Terraform state files may store sensitive data in plaintext, risking exposure if not secured.
    
* **Hard-Coded Secrets:** Including sensitive data directly in Terraform configurations makes them vulnerable to leaks.
    

**Solution:** HashiCorp Vault provides a secure and dynamic way to manage secrets. Integrating Vault with Terraform helps protect sensitive data while maintaining streamlined workflows.

---

## Setting Up HashiCorp Vault with Terraform

In this section, we will walk through the steps for securely integrating Vault with Terraform.

#### **1\. Setting Up HashiCorp Vault**

1. **Install Vault**: Follow the installation guide to install Vault on your machine.
    
2. **Start Vault Server**: Start Vault in development mode (for testing purposes):
    
    ```bash
     vault server -dev
    ```
    

3. **Set the Vault Address**: Set the Vault address:
    
    ```bash
    export VAULT_ADDR=http://0.0.0.0:8200
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734703609728/de82dccb-5edb-44e5-93ff-ec698a851527.png align="center")
    

4. Access vault in the browser using the token available in CLI
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734703563097/059d4ff8-d388-46c4-8d66-185757fae6e9.png align="center")
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734703659441/4ba668a1-fd0c-4095-9047-e14e09a6bcda.png align="center")
    
5. Create a Key-Value secret
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734703887581/3f5c554c-6189-48da-bc56-5333bdb4d342.png align="center")
    

---

### 2\. Enable AppRole Authentication

1. Run the following command to enable the AppRole authentication method:  
    
    ```bash
    vault auth enable approle
    ```
    
    This command tells Vault to enable the AppRole authentication method.
    

2. **Create an AppRole**:
    
    We need to create policy first,
    
    ```bash
    vault policy write terraform - <<EOF
    path "*" {
      capabilities = ["list", "read"]
    }
    
    path "secrets/data/*" {
      capabilities = ["create", "read", "update", "delete", "list"]
    }
    
    path "kv/data/*" {
      capabilities = ["create", "read", "update", "delete", "list"]
    }
    
    
    path "secret/data/*" {
      capabilities = ["create", "read", "update", "delete", "list"]
    }
    
    path "auth/token/create" {
    capabilities = ["create", "read", "update", "list"]
    }
    EOF
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734704574868/224a4475-a5d1-43b0-bc7a-4db0b136c2d6.png align="center")
    
    Now you'll need to create an AppRole with appropriate policies and configure its authentication settings. Here are the steps to create an AppRole:
    
    ```bash
    vault write auth/approle/role/terraform \
        secret_id_ttl=10m \
        token_num_uses=10 \
        token_ttl=20m \
        token_max_ttl=30m \
        secret_id_num_uses=40 \
        token_policies=terraform
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734704683525/a37fb823-c719-4dab-b512-1471d547ae16.png align="center")
    
3. **Generate Role ID and Secret ID**:  
    
    After creating the AppRole, you need to generate a Role ID and Secret ID pair. The Role ID is a static identifier, while the Secret ID is a dynamic credential.  
    
    **a. Generate Role ID**:
    
    You can retrieve the Role ID using the Vault CLI:
    
    ```bash
    vault read auth/approle/role/my-approle/role-id
    ```
    
    Save the Role ID for use in your Terraform configuration.  
    
    **b. Generate Secret ID**:
    
    To generate a Secret ID, you can use the following command:
    
    ```bash
    vault write -f auth/approle/role/my-approle/secret-id
    ```
    
    This command generates a Secret ID and provides it in the response. Save the Secret ID securely, as it will be used for Terraform authentication.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734704864249/e5346732-59b2-4df5-8231-f10d09186437.png align="center")
    

---

## Configuring Terraform with Vault

Next, let’s configure Terraform to authenticate to Vault and retrieve secrets.

1. **Install Terraform**: If you don’t have Terraform installed, follow the installation guide on the Terraform website.
    
2. Add the Vault Provider  
    In your Terraform configuration file (`main.tf`), define the Vault provider:
    
    ![Inject secrets into Terraform using the Vault provider | Terraform |  HashiCorp Developer](https://content.hashicorp.com/api/assets?product=tutorials&version=main&asset=public%2Fimg%2Fterraform%2Fsecrets%2Foverview.png align="left")
    
    ```bash
     provider "vault" {
       address = "http://127.0.0.1:8200"
       auth_login {
         path = "auth/approle/login"
         parameters = {
           role_id   = "<role_id>"
           secret_id = "<secret_id>"
         }
       }
     }
    ```
    
3. Fetch Secrets
    
    Use the Vault provider to fetch secrets dynamically:
    
    ```bash
     data "vault_kv_secret_v2" "example" {
       mount = "kv"
       name  = "my-secret"
     }
    ```
    
4. ### Use Secrets in Resources
    
    Integrate fetched secrets into your Terraform resources:
    
    ```bash
    resource "aws_instance" "example" {
      ami           = "ami-0a123456b7890cdef"
      instance_type = "t2.micro"
    
      tags = {
        Name   = "ExampleInstance"
        Secret = data.vault_kv_secret_v2.example.data["username"]
      }
    }
    ```
    

---

## Best Practices

* **Avoid Hard-Coded Secrets:** Always retrieve secrets dynamically from Vault.
    
* **Leverage Vault Policies:** Define granular access controls to minimize exposure.
    
* **Environment Variables:** Store sensitive configurations (e.g., Vault address) in environment variables instead of Terraform files.
    
* **Dynamic Secrets:** Use Vault’s dynamic secrets for services like databases.
    
* **Rotate Secrets Regularly:** Implement periodic rotation of Vault secrets to enhance security.
    

---

### **Terraform Commands You’ll Use**

* **Initialize Project**:
    
    ```bash
      terraform init
    ```
    
* **Validate Syntax**:
    
    ```bash
      terraform validate
    ```
    
* **Apply Changes**:
    
    ```bash
      terraform apply
    ```
    
* **Clean Up Resources**:
    
    ```bash
      terraform destroy
    ```
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734706130898/8593ebfc-d756-4b73-b45e-ef5a78d81ea5.png align="center")

---

### **Resources created:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734706232766/6aacc8f7-cca7-4881-b3aa-2065b5ce375b.png align="center")

---

### Conclusion

Integrating HashiCorp Vault with Terraform simplifies the secure management of sensitive data, reducing the risk of exposure. By following the steps and best practices outlined in this guide, you can enhance your infrastructure’s security while maintaining an efficient workflow.

**What are your thoughts on integrating Vault with Terraform?**

Let me know in the comments below! Share your experiences or tips for using Vault in Terraform workflows. Let’s learn together!