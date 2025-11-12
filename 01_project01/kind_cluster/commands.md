kubectl which is cli tool to interact with cluster created  by kind using image kindest

kubectl get nodes

kubectl get nodes --context kind-clustername
which in my case is tws-cluster
so 
kubectl get nodes --context kind-tws-cluster

 kubectl get namespace
 or 
kubectl get ns


kubectl get pods


kubectl get pods -n kube-system
instead of kube-system can use any namespace which are avaialble

kubectl run nginx --image=nginx >>>>>>>>>>>>>. to create pod

kubectl delete pod nginx 
pod name can change enter any name which are available after listing 

kubectl run nginx --image=nginx -n nginx
 to create pod inside specific namespace
kubectl get pods -n nginx 
make sure its created

kubectl delete pod nginx -n nginx
kubectl delete namespace nginx


for manifest files now commands are differnet to apply namspaces ,pods ,deployments using manifest files
kubectl apply -f namespace.yml

kubectl apply -f pod.yml 
to create pod using pod.yaml  manifest file 

 kubectl exec -it pod/nginxpod -n nginx -- bash 
 check pods

 kubectl describe pod/nginxpod -n nginx

 kubectl delete -f pod.yml 

 kubectl apply -f deployment.yml

kubectl scale deployment/nginxdeployment -n nginx --replicas=5
kubectl scale deployment/nginxdeployment -n nginx --replicas=1


 kubectl get pods -n nginx -o wide

 kubectl set image deployment/nginxdeployment -n nginx nginx=nginx:1.27.3


  kubectl get deployments

kubectl rollout status deployment/nginx-deployment

get rs -n nginx

kubectl get pods --show-labels -n nginx 

kubectl logs nginxjob-xbp7s(pod name) -n nginx

 kubectl get cronjob -n nginx


 kubectl get cronjob -n nginx

  kubectl delete -f cronjob.yml


  kubectl delete pod --all -n nginx

watch kubectl  get pods -n mysql

echo "root" | base64

echo -n "rootpassword" | base64


kubectl rollout restart deployment <deployment-name>

kubectl taint node twscluster-worker3 prod=true:NoSchedule-


 kubectl get pods -n kube-system .............for api service  resource usage

 kubectl top node

 kubectl top pod -n nginx

 curl http:apacheservice.apachenamespace.svc.cluster.local


 kubectl run loadgenerator \
  --image=busybox \
  -n apachenamespace \
  -- /bin/sh -c "while true; do :; done"



  kubectl run loadgenerator \
  --image=curlimages/curl:latest \
  -n apachenamespace \
  -- /bin/sh -c "while true; do curl -s http://apacheservice.apachenamespace.svc.cluster.local > /dev/null; done"

kubectl  auth whoami

kubectl auth can-i get pods

kubectl auth can-i get pods -n apache

kubectl auth  can-i get pods -n apachenamespace --as=apache-user