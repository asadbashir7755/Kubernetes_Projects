# 🟢 Kubernetes Commands Reference — Complete Sequence

## 🔹 Cluster and Node Commands

```bash
kubectl get nodes
kubectl get nodes --context kind-tws-cluster
```

## 🔹 Namespace Commands

```bash
kubectl get namespace
kubectl get ns
kubectl apply -f namespace.yml  # create namespace via manifest
kubectl delete namespace nginx
```

## 🔹 Pod Commands

```bash
kubectl get pods
kubectl get pods -n kube-system  # or any namespace
kubectl run nginx --image=nginx  # create pod
kubectl run nginx --image=nginx -n nginx  # pod in specific namespace
kubectl get pods -n nginx
kubectl delete pod nginx
kubectl delete pod nginx -n nginx
kubectl apply -f pod.yml  # pod via manifest
kubectl delete -f pod.yml
kubectl exec -it pod/nginxpod -n nginx -- bash  # access container
kubectl describe pod/nginxpod -n nginx
kubectl get pods -n nginx -o wide
kubectl logs nginxjob-xbp7s -n nginx
kubectl delete pod --all -n nginx
watch kubectl get pods -n mysql
```

## 🔹 Deployment Commands

```bash
kubectl apply -f deployment.yml
kubectl get deployments
kubectl scale deployment/nginxdeployment -n nginx --replicas=5
kubectl scale deployment/nginxdeployment -n nginx --replicas=1
kubectl set image deployment/nginxdeployment -n nginx nginx=nginx:1.27.3
kubectl rollout status deployment/nginxdeployment
kubectl rollout restart deployment <deployment-name>
```

## 🔹 ReplicaSet & Labels

```bash
kubectl get rs -n nginx
kubectl get pods --show-labels -n nginx
```

## 🔹 CronJob Commands

```bash
kubectl get cronjob -n nginx
kubectl delete -f cronjob.yml
```

## 🔹 Resource Commands

```bash
kubectl top node
kubectl top pod -n nginx
kubectl get pods -n kube-system  # for API service resource usage
```

## 🔹 Taints and Tolerations

```bash
kubectl taint node twscluster-worker prod=true:NoSchedule
kubectl taint node twscluster-worker3 prod=true:NoSchedule-
```

## 🔹 Service Testing and Port Forward

```bash
curl http://apacheservice.apachenamespace.svc.cluster.local
kubectl port-forward svc/dev-apache-apache-helm 8080:80 -n apache --address=0.0.0.0
```

## 🔹 Load Generator Examples

```bash
kubectl run loadgenerator \
  --image=busybox \
  -n apachenamespace \
  -- /bin/sh -c "while true; do :; done"

kubectl run loadgenerator \
  --image=curlimages/curl:latest \
  -n apachenamespace \
  -- /bin/sh -c "while true; do curl -s http://apacheservice.apachenamespace.svc.cluster.local > /dev/null; done"
```

## 🔹 Authentication Checks

```bash
kubectl auth whoami
kubectl auth can-i get pods
kubectl auth can-i get pods -n apache
kubectl auth can-i get pods -n apachenamespace --as=apache-user
```

## 🔹 Helm Commands

```bash
helm list --all
```
