curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

helm version

helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update

helm install my-nginx bitnami/nginx

kubectl get all

helm list

helm upgrade my-nginx bitnami/nginx


helm uninstall my-nginx


 commands
 helm create apache_helm

 helm package apache_helm/

helm install dev-apache apache-helm-0.1.0.tgz  --namespace apache 

 helm install dev-apache apache-helm-0.1.0.tgz  --namespace apache --create-namespace
 if namespace not exist


  helm ugrade dev-apache ./apache-helm -n apache 

  helm rollback prd-apache 1 -n dev-apache


  $ kubectl port-forward svc/dev-apache-apache-helm 8080:80 -n apache --address=0.0.0.0