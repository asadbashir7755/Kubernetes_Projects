# Docker + Kubernetes Custom NGINX Project

## Project Overview

This project demonstrates the full workflow of creating a custom NGINX Docker image, deploying it to Kubernetes, and exposing it via a Service. The project highlights containerization, image management, Kubernetes deployments, services, port forwarding, rolling updates, and scaling.

---

## Project Structure

* `index.html` : Custom HTML page served by NGINX.
* `Dockerfile` : Defines the custom NGINX image.
* Kubernetes manifests:

  * `namespace.yml` : Namespace definition.
  * `deployment.yml` : Deployment manifest with custom image.
  * `service.yml` : Service manifest to expose deployment.

---

## Steps Performed

### 1. Docker Workflow

1. Created `Dockerfile` in project root:

```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

2. Built the Docker image locally:

```bash
docker build -t customnginx:latest .
```

3. Tagged the image for Docker Hub:

```bash
docker tag customnginx:latest asadbashir7755/customnginx:1.1
```

4. Pushed the image to Docker Hub:

```bash
docker push asadbashir7755/customnginx:1.1
```

5. Verified locally:

```bash
docker run -p 8080:80 customnginx:latest
# Access http://localhost:8080 to check index.html
```

### 2. Kubernetes Setup

#### Step 1: Create Namespace

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: customnginx
```

Apply:

```bash
kubectl apply -f namespace.yml
kubectl get namespace
```

#### Step 2: Create Deployment

* Deployment uses custom image from Docker Hub.
* Initial replicas = 1.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: customnginxdeployment
  namespace: customnginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: customnginx
  template:
    metadata:
      labels:
        app: customnginx
    spec:
      containers:
        - name: customnginxcontainer
          image: asadbashir7755/customnginx:1.1
          ports:
            - containerPort: 80
```

Apply:

```bash
kubectl apply -f deployment.yml
kubectl get deployment -n customnginx
kubectl get pods -n customnginx
kubectl describe pod <pod-name> -n customnginx
```

#### Step 3: Create Service

* Service exposes the deployment inside the cluster.
* ClusterIP used for internal access.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: customnginxservice
  namespace: customnginx
spec:
  selector:
    app: customnginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

Apply:

```bash
kubectl apply -f service.yml
kubectl get svc -n customnginx
```

#### Step 4: Port Forward to Access Locally

```bash
kubectl port-forward svc/customnginxservice 8080:80 -n customnginx --address=0.0.0.0
```

* Access via: `http://<EC2-public-IP>:8080`
* Custom `index.html` page is live.

### 3. Extra Practice / Advanced Kubernetes Features

#### Rolling Update (Changing Image)

```bash
kubectl set image deployment/customnginxdeployment customnginxcontainer=asadbashir7755/customnginx:1.2 -n customnginx
kubectl rollout status deployment/customnginxdeployment -n customnginx
```

* Kubernetes creates a new Pod with updated image, replaces old Pod.
* No downtime.

#### Scaling Deployment

```bash
kubectl scale deployment customnginxdeployment --replicas=3 -n customnginx
kubectl get pods -n customnginx
```

* Demonstrates multiple replicas and load distribution.

---

## Commands Used

### Docker Commands

```bash
docker ps

docker images
docker build -t customnginx:latest .
docker tag customnginx:latest asadbashir7755/customnginx:1.1
docker push asadbashir7755/customnginx:1.1
```

### Kubernetes Commands

```bash
kubectl apply -f namespace.yml
kubectl apply -f deployment.yml
kubectl apply -f service.yml

kubectl get namespace
kubectl get deployment -n customnginx
kubectl get pods -n customnginx
kubectl describe pod <pod-name> -n customnginx

kubectl scale deployment customnginxdeployment --replicas=3 -n customnginx
kubectl set image deployment/customnginxdeployment customnginxcontainer=asadbashir7755/customnginx:1.1 -n customnginx
kubectl port-forward svc/customnginxservice 8080:80 -n customnginx --address=0.0.0.0
```

---

## ✅ Summary

* Created custom HTML and NGINX Docker image.
* Pushed image to Docker Hub.
* Deployed image on Kubernetes with Deployment + Service.
* Verified live access using port-forward.
* Practiced **rolling updates** and **scaling deployments**.
* Demonstrated understanding of: namespaces, deployments, services, selectors, ports, container images, and Kubernetes commands.

