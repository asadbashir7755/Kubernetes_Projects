# 🖥️ Kubernetes Dashboard

## What is Kubernetes Dashboard

* Web-based UI for Kubernetes clusters.
* Allows you to **view, manage, and troubleshoot** resources in your cluster.
* Useful for monitoring deployments, pods, services, and other objects.

## Why Use It

* Easy visual interface for managing resources.
* Quick access to logs, resource usage, and configuration.
* Ideal for beginners to understand cluster state.

## Dashboard Installation

### Option 1: Using Manifest File

```bash
# Apply the dashboard YAML
kubectl apply -f dashboardadminuser.yml

# Create a token for the admin user
kubectl -n kubernetes-dashboard create token adminuser

# Start kubectl proxy
kubectl proxy --address=0.0.0.0
```

* Access dashboard:

```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

### Option 2: Using Helm (Recommended for latest version)

```bash
# Add the official Helm repo for Kubernetes Dashboard
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
helm repo update

# Install the dashboard using Helm
helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard \
  --namespace kubernetes-dashboard --create-namespace
```

## Important Commands

```bash
# Create admin token
kubectl -n kubernetes-dashboard create token adminuser

# Start proxy
kubectl proxy --address=0.0.0.0

# Get access URL
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

## Notes

* Helm installation is preferred for the latest versions.
* Use the proxy command to access the dashboard from your local browser.
* Token authentication is required for secure access.
