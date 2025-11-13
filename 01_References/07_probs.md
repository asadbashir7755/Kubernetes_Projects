# 🧠 Kubernetes Advanced Concepts — Resource Quotas, Probes, and Taints

## 🟩 Resource Quota

A **ResourceQuota** in Kubernetes limits the **CPU, memory, and object usage** in a namespace.

* Ensures fair usage among teams or applications.
* Example:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev
spec:
  hard:
    requests.cpu: "2"
    requests.memory: 4Gi
    limits.cpu: "4"
    limits.memory: 8Gi
    pods: "10"
```

* Pods exceeding the quota cannot be scheduled.

---

## 🔹 Probes — Ensure Pod Health

Probes monitor if Pods are **working correctly**.

### Types of Probes

| Probe               | Purpose                                                                                             |
| ------------------- | --------------------------------------------------------------------------------------------------- |
| **Liveness Probe**  | Checks if the container is alive. Restarts if it fails.                                             |
| **Readiness Probe** | Checks if the container is ready to serve traffic. Unready Pods are removed from Service endpoints. |
| **Startup Probe**   | Checks startup process for slow containers. Useful for containers with long initialization.         |

* Probes help **self-healing** and stable app deployment.

### Example Probe:

```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10
```

---

## 🧩 Taints and Tolerations

**Taints** prevent Pods from being scheduled on certain nodes unless the Pod **tolerates** the taint.

### Commands:

* Add taint:

```bash
kubectl taint node twscluster-worker prod=true:NoSchedule
```

* Remove taint:

```bash
kubectl taint node twscluster-worker prod=true:NoSchedule-
```

### Concepts:

* **Taint** = Node restriction.
* **Toleration** = Pod permission to run on tainted nodes.

Example:

```yaml
tolerations:
- key: "prod"
  operator: "Equal"
  value: "true"
  effect: "NoSchedule"
```

* Ensures only specific Pods run on tainted nodes.

---

✅ Summary:

* **ResourceQuota** → Limit resources per namespace.
* **Probes** → Monitor Pod health and readiness.
* **Taints/Tolerations** → Control Pod scheduling on specific nodes.
