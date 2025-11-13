# 🔹 Helm Commands — Kubernetes

## Install Helm

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm version
```

## Helm Repositories

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

## Install Chart

```bash
helm install my-nginx bitnami/nginx
```

## Check Installed Resources

```bash
kubectl get all
helm list
```

## Upgrade Chart

```bash
helm upgrade my-nginx bitnami/nginx
```

## Uninstall Chart

```bash
helm uninstall my-nginx
```

## Creating and Packaging Helm Chart

```bash
helm create apache_helm
helm package apache_helm/
```

## Install Custom Helm Chart

```bash
helm install dev-apache apache-helm-0.1.0.tgz --namespace apache
helm install dev-apache apache-helm-0.1.0.tgz --namespace apache --create-namespace  # if namespace doesn't exist
```

## Upgrade Custom Helm Chart

```bash
helm upgrade dev-apache ./apache-helm -n apache
```

## Rollback Helm Release

```bash
helm rollback prd-apache 1 -n dev-apache
```

## Port Forwarding to Access Service

```bash
kubectl port-forward svc/dev-apache-apache-helm 8080:80 -n apache --address=0.0.0.0
```
