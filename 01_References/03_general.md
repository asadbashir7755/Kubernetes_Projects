# 🧠 Kubernetes Complete Beginner Notes (By Asad)

## 🟩 1. What is Kubernetes?

**Kubernetes (K8s)** is an open-source container orchestration platform used to **automate deployment, scaling, and management of containerized applications**.

### Why It Was Needed

Before Kubernetes, Google used an internal tool called **Borg** to manage thousands of containers.
To make this technology public and standardized, Google released **Kubernetes in 2014**, and later donated it to the **Cloud Native Computing Foundation (CNCF)**.

### Relation with Docker

* **Docker** helps you **create and run containers**.
* **Kubernetes** helps you **manage and scale** those containers across many servers.

So:
🔑 **Docker = creates containers**
🔑 **Kubernetes = manages containers at scale**

### Who Owns It Now

Kubernetes is maintained by the **CNCF (Cloud Native Computing Foundation)** — a part of the **Linux Foundation**, which promotes cloud-native tools like:

* Kubernetes
* Prometheus
* Helm
* Envoy
* etc.

---

## 🟩 2. What is Orchestration?

**Orchestration** means automatically handling the **deployment, networking, scaling, and lifecycle** of containers.

### Example

Before Kubernetes, apps were built as **monolithic** (all in one block).
Now, apps are divided into **microservices** (e.g., user, payment, order services).
Managing these hundreds of microservices manually is hard, so we use **Kubernetes** to orchestrate them.

🔑 **Industry Trend:**
Microservices architecture is now in demand, and Kubernetes is essential to manage it.

---

## 🟩 3. Kubernetes Certifications

* **CKA** – Certified Kubernetes Administrator
* **CKAD** – Certified Kubernetes Application Developer
* **CKS** – Certified Kubernetes Security Specialist

---

## 🟩 4. Kubernetes Architecture

### Master (Control Plane) Node Components

* **API Server:** Communication gateway between master and worker nodes.
* **Scheduler:** Decides which node should run a Pod.
* **Controller Manager:** Ensures the cluster state matches the desired configuration.
* **etcd:** Key-value database storing cluster state (e.g., nodes, pods, configs).

### Worker Node Components

* **Kubelet:** Ensures containers are running properly on the node.
* **Kube Proxy (Service Proxy):** Manages networking and load-balancing for Pods.
* **Pods:** The smallest deployable unit in Kubernetes (contains one or more containers).
* **CNI (Container Network Interface):** Enables communication between Pods and nodes.

---

## 🟩 5. Pods, Deployments, and Services

### Pods

A **Pod** is the smallest unit in Kubernetes — it wraps one or more containers that share the same resources.

### Deployments

A **Deployment** manages Pods and their replicas, ensuring desired state (like version, count, or image).

### ReplicaSet / StatefulSet

* **ReplicaSet:** Maintains a stable number of replica Pods.
* **StatefulSet:** Used for stateful apps (like databases) where identity/order matters.

### Rolling Update

When you update a Deployment, Kubernetes gradually replaces old Pods with new ones — this is a **rolling update**.

---

## 🟩 6. kubectl

**kubectl** is the command-line tool to interact with the Kubernetes cluster.

Examples:

```bash
kubectl get pods
kubectl get nodes
kubectl apply -f deployment.yaml
```

---

## 🟩 7. Kubernetes Cluster Types

| Type         | Description                                                   | Usage                        |
| ------------ | ------------------------------------------------------------- | ---------------------------- |
| **Kind**     | Kubernetes IN Docker (runs clusters inside Docker containers) | Local testing & CI/CD        |
| **Minikube** | Runs single-node Kubernetes cluster locally                   | Beginners & learners         |
| **kubeadm**  | Official tool to install real multi-node clusters             | Production or on-prem setups |
| **EKS**      | Elastic Kubernetes Service (AWS)                              | Managed production on AWS    |
| **AKS**      | Azure Kubernetes Service                                      | Managed production on Azure  |
| **GKE**      | Google Kubernetes Engine                                      | Managed production on GCP    |

---

## 🟩 8. Installing Kubernetes with Kind

### Prerequisites

Install **Docker**, **Kind**, and **kubectl**.

```bash
# Install Docker
sudo dnf install docker.io -y
sudo systemctl enable docker --now

# Install Kind (Kubernetes in Docker)
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Verify installation
kind version

# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

### Create a Cluster

```bash
kind create cluster --name mycluster --image kindest/node:v1.31.2
```

---

## 🟩 9. Kubernetes YAML Basics

Each configuration file (YAML) is called a **manifest file**.

> "The power of the manifest file: whatever we want, Kubernetes will do!"

### Common Fields

| Field          | Meaning                        | Example                  |
| -------------- | ------------------------------ | ------------------------ |
| **kind**       | Defines the type of object     | Pod, Deployment, Service |
| **apiVersion** | Defines API schema version     | apps/v1, v1              |
| **metadata**   | Object name, labels, namespace | name: mypod              |
| **spec**       | Desired configuration          | replicas, image, ports   |

### Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

---

## 🟩 10. Namespace in Kubernetes

A **Namespace** is used to logically divide cluster resources.
Think of it as folders in a computer.

Example:

* `dev` namespace for developers
* `prod` namespace for production

```bash
kubectl create namespace dev
```

---

## 🟩 11. Flow in Kubernetes

**Container → Pod → Deployment → Service**

### Example Flow

1. **Container**: Your application (e.g., `nginx`)
2. **Pod**: Runs the container
3. **Deployment**: Ensures desired number of Pods
4. **Service**: Exposes the Pods to the network

---

## 🟩 12. Summary

* Kubernetes = Container Orchestrator
* Built by Google, inspired by Borg
* Owned by CNCF
* Works with Docker
* Core Components: API Server, etcd, Scheduler, Kubelet, Proxy
* Tools: Kind, Minikube, Kubeadm, EKS/AKS/GKE
* YAML = Manifest File
* Namespace = Logical Separation
* kubectl = Control Tool

> 📢 **Master Kubernetes by understanding architecture first, then practice YAMLs daily!**
