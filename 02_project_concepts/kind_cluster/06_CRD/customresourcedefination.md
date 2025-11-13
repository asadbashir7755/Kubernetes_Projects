# 🧠 Kubernetes CRD (Custom Resource Definition) Notes

## ✅ What is a CRD?

A **Custom Resource Definition (CRD)** allows you to **extend Kubernetes with your own resource types**.

* Kubernetes comes with built-in resources like Pods, Services, Deployments.
* Sometimes, your application needs a custom object type that Kubernetes doesn’t provide.
* CRD lets you define this custom object, e.g., `MyApp`, `DatabaseCluster`, `BackupJob`.

Once defined, these behave like native Kubernetes resources.

---

## 🛠️ Why Use CRDs

* Extend Kubernetes functionality without modifying its core.
* Define domain-specific resources for your application.
* Automate management using custom controllers/operators.
* Standardize deployment and configuration for teams.

Example:

* You create `DatabaseCluster` CRD. Now you can `kubectl get databasecluster` like you would `kubectl get pods`.

---

## ⚡ CRD Concepts

| Concept                    | Explanation                                                                               |
| -------------------------- | ----------------------------------------------------------------------------------------- |
| Custom Resource            | The instance of your CRD (like a Pod for Deployment). Example: `mydb-cluster1`            |
| Custom Resource Definition | Schema/definition for your custom resource, tells K8s the fields, validation, and version |
| API Version                | Like built-in objects, e.g., `mygroup.example.com/v1`                                     |
| Kind                       | Name of your resource type, e.g., `DatabaseCluster`                                       |
| Spec                       | Desired state defined by user                                                             |
| Status                     | Current state reported by Kubernetes or your controller                                   |

---

## 🔄 Flow

1. Create CRD YAML
2. Apply CRD with `kubectl apply -f crd.yaml`
3. Create Custom Resource (instance) YAML
4. Apply custom resource with `kubectl apply -f mydb-instance.yaml`
5. Kubernetes (with custom controller/operator) reconciles desired vs current state

---

## 📝 CRD YAML Example

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: databaseclusters.mygroup.example.com
spec:
  group: mygroup.example.com
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                replicas:
                  type: integer
                version:
                  type: string
  scope: Namespaced
  names:
    plural: databaseclusters
    singular: databasecluster
    kind: DatabaseCluster
    shortNames:
    - dbc
```

Once applied:

```bash
kubectl apply -f databasecluster-crd.yaml
kubectl get crd
kubectl get databaseclusters
```

---

## ⚙️ Real-World Use Cases

* Custom application operators: RedisCluster, KafkaCluster, ElasticsearchCluster
* Backup jobs specific to an app
* Custom monitoring resources
* Multi-tenant resource management

---

### 💡 Notes

* CRDs + Operators = powerful pattern in Kubernetes for building application-specific automation.
* You can have multiple versions (`v1`, `v2`) in CRD for backward compatibility.
* CRDs can be **Namespaced** or **Cluster-scoped** depending on usage.

---

This covers **all fundamental concepts of CRD in Kubernetes**.
