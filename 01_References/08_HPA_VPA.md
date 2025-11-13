# 🧠 Kubernetes Autoscaling Concepts — HPA, VPA, and KEDA

## 🔹 Horizontal Pod Autoscaler (HPA)

* **Purpose:** Automatically scales the **number of Pods** based on observed metrics (CPU, memory, custom metrics).
* **Use Case:** When load increases, add more Pods; when load decreases, reduce Pods.
* Example:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: webapp-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: webapp
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

## 🔹 Vertical Pod Autoscaler (VPA)

* **Purpose:** Automatically adjusts **CPU and memory resources** of Pods instead of increasing replicas.
* **Use Case:** Commonly used with **StatefulSets** where pod identity matters.
* Requires installation from the **GitHub repo:** [https://github.com/kubernetes/autoscaler.git](https://github.com/kubernetes/autoscaler.git)
* VPA increases Pod specifications rather than number of Pods.

## 🔹 KEDA (Kubernetes Event-driven Autoscaling)

* **Purpose:** Automatically scales workloads based on **events or custom metrics**.
* Integrates **HPA or VPA** based on external events (e.g., queue length, Kafka messages).
* Useful for **event-driven architectures** and serverless workloads.

### ⚡ Summary

| Autoscaler | Scales                       | Use Case                                     |
| ---------- | ---------------------------- | -------------------------------------------- |
| HPA        | Number of Pods               | Stateless workloads, horizontal scaling      |
| VPA        | Pod CPU/Memory               | Stateful workloads, vertical scaling         |
| KEDA       | Pods based on events/metrics | Event-driven, serverless, or custom triggers |
