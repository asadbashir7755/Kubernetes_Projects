In Real-World DevOps

In production, you almost always:

Use NGINX Ingress Controller (most popular)

Combine it with Cert-Manager for automatic SSL certificates

Use Ingress + DNS (domain names) to expose microservices properly

For example:

URL	Service
frontend.myapp.com	Frontend
api.myapp.com	Backend
grafana.myapp.com	Monitoring dashboard

All managed through one ingress load balancer.



 kubectl get ingress -n nginx

 use anyother port if 80 not work with port forward