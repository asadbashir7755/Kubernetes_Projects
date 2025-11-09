k8s service 
all concepts about it


service flow

Browser (port 8080)
   ↓
kubectl client (your laptop)
   ↓
Kubernetes API server
   ↓
Service: nginxservice (port 80)
   ↓
Pod(s) with label app=nginx



