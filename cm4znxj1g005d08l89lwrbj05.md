---
title: "Kubernetes Made Easy: Understanding Deployments, ConfigMaps, and Secrets"
seoTitle: "Deployments, ConfigMaps & Secrets Basics"
seoDescription: "Learn Kubernetes with an easy guide on Deployments, ConfigMaps, and Secrets to manage and secure your applications effectively"
datePublished: Sun Dec 22 2024 13:47:31 GMT+0000 (Coordinated Universal Time)
cuid: cm4znxj1g005d08l89lwrbj05
slug: kubernetes-made-easy-understanding-deployments-configmaps-and-secrets
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734874930775/692643eb-0d34-4d61-9ff8-2075a5af4b39.png
tags: cloud, docker, aws, kubernetes, developer, cloud-computing, devops, jenkins, ci-cd, devops-articles, 90daysofdevops, trainwithshubham, 90daysofdevops-chanllenge, tws

---

Kubernetes makes managing containerized applications easier. In this blog, we'll walk you through deploying a sample application, configuring it using ConfigMaps, and securing sensitive data with Secrets. Let’s get started!

---

### Step 1: Clone the Project and Build the Docker Image

Begin by cloning the project repository and building the Docker image:

```bash
docker build -t sample-note-app:v1 .
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734067052414/55f49110-9341-4ed3-8232-3e3d4d08d9ae.png align="center")

---

### Step 2: Create the Deployment File

Here’s a basic `deployment.yml` file for deploying your app:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-note-app
  labels:
    app: sample-note-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: sample-note-app
  template:
    metadata:
      labels:
        app: sample-note-app
    spec:
      containers:
      - name: note-app
        image: sample-note-app:v1
        ports:
        - containerPort: 8000
```

---

### Step 3: Apply the Deployment File

Run the following command to deploy the application:

```bash
kubectl apply -f deployment.yml
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734067279169/85dc25a1-4184-43a7-8d5e-e050a00ffa14.png align="center")

Check if your deployment and pods are running:

```bash
kubectl get pods -o wide
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734067537313/a4b02e32-c999-465c-aebf-a9629e9bd99c.png align="center")

---

### Step 4: Configure a ConfigMap

Create a ConfigMap file (`cm.yml`) to store configuration data:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: test-cm
data:
  db-port: "3306"
```

Apply it:

```bash
kubectl apply -f cm.yml
```

Verify its creation:

```bash
kubectl get configmap test-cm
kubectl describe configmap test-cm
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734067476380/fedcae3c-73bb-4976-bf3f-5d5125778500.png align="center")

---

### Step 5: Use ConfigMap in Deployment

Update your `deployment.yml` to use the ConfigMap as an environment variable:

```bash
 kubectl edit deployment sample-note-app
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734067791865/501e9b04-67ff-4a64-a1fa-a13c6cd6e8e8.png align="center")

Notice how Kubernetes terminates old pods and creates new ones.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734067768592/135efc44-3425-419a-83aa-015eca3604c0.png align="center")

Check inside the pods to confirm the environment variable is set.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734067996995/2f92d16f-231b-46e5-9e4a-3529510e4765.png align="center")

---

### Problem: ConfigMap Changes Don’t Reflect in Running Pods

If the `db-port` value changes in the ConfigMap, it won’t update in running pods without restarting them. This can disrupt live traffic, which isn’t ideal in production.

---

### Solution: Use VolumeMount

Instead of environment variables, use ConfigMap as a mounted file. Update your `deployment.yml` like this:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-note-app
  labels:
    app: sample-note-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: sample-note-app
  template:
    metadata:
      labels:
        app: sample-note-app
    spec:
      containers:
      - name: note-app
        image: sample-note-app:v1
        volumeMounts:
          - name: db-connection
            mountPath: /opt
        ports:
        - containerPort: 8000
      volumes:
        - name: db-connection
          configMap:
            name: test-cm
```

Apply the changes and check inside the pods. You’ll see a file named `db-port` in `/opt` with the value `3306`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734069610008/1559f313-fb81-4cfe-9c14-c63afb792eec.png align="center")

Now, if you update the ConfigMap, the file value updates automatically without restarting the pods.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734069772256/d15d5a6d-dcab-4f55-be9f-fa6a27b3bf2c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734069961305/bf05b78a-c59b-45a5-ba33-7f971bb3d40d.png align="center")

---

### Secrets: Secure Sensitive Data

To store sensitive data like passwords, use Secrets.

1. Create a secret:
    

```bash
kubectl create secret generic test-secret --from-literal=db-port="3306"
```

2. View the secret:
    

```bash
kubectl get secrets test-secret
kubectl describe secret test-secret
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734070287581/fc05ae4a-29f2-47b7-a56e-980fd04454c7.png align="center")

3. Note: Kubernetes Secrets are only base64-encoded. For stronger encryption, use tools like **HashiCorp Vault** or **Sealed Secrets**.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734070552713/e322625d-28c3-407c-95db-0907b1dd9fb8.png align="center")
    

To learn more, check out **"How to Encrypt etcd for Secrets"**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734070750411/566c0b60-5397-41b3-ab2a-a3123521eea7.png align="center")

---

### Final Thoughts

Kubernetes offers flexibility and power, but understanding its nuances, like using ConfigMaps effectively or securing sensitive data, is critical. By following this guide, you’ll be better prepared to handle real-world challenges.

Got feedback or questions? Drop them in the comments below! 👇

Happy coding! 🚀