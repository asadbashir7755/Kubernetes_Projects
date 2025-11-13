# 🧠 Kubernetes Ingress and Real-World Usage

## 🔹 Ingress in Production

In real-world DevOps setups, Ingress is used to manage and route external traffic to Kubernetes services.

### Common Practices

* **NGINX Ingress Controller:** Most popular Ingress controller.
* **Cert-Manager:** Automates SSL certificate provisioning.
* **DNS + Ingress:** Expose microservices using domain names.

### Example Mapping

| URL                | Service              |
| ------------------ | -------------------- |
| frontend.myapp.com | Frontend             |
| api.myapp.com      | Backend              |
| grafana.myapp.com  | Monitoring dashboard |

All services are managed via a **single Ingress load balancer**.

### Useful Commands

* View Ingress resources:

```bash
kubectl get ingress -n nginx
```

* If port 80 conflicts, use port forwarding with a different port:

```bash
kubectl port-forward service/<service-name> <local-port>:<service-port> -n <namespace>
```
