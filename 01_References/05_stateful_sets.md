# 🧱 StatefulSets — Managing Stateful Applications

## 🔍 What is a StatefulSet?

* A **StatefulSet** is a Kubernetes controller for **Pods that need persistent identity and storage**.
* Ideal for **databases** and **stateful apps** (MySQL, MongoDB, Cassandra, Kafka, Redis).

## 🧩 Deployment vs StatefulSet

| Feature        | Deployment            | StatefulSet                       |
| -------------- | --------------------- | --------------------------------- |
| Pod names      | Random                | Fixed, ordered (`db-0`, `db-1`)   |
| Pod identity   | Changes after restart | **Stable** (same name, hostname)  |
| Storage        | Shared / ephemeral    | **PersistentVolumeClaim per Pod** |
| Creation order | All at once           | **Sequential** (`0 → 1 → 2`)      |
| Deletion order | Random                | **Reverse** (`2 → 1 → 0`)         |
| Use case       | Stateless apps        | **Stateful apps (databases)**     |

## 🧠 Why "Stateful"?

* Each Pod **remembers its data** even if restarted.
* Example: `mysql-0` writes to `/var/lib/mysql` → stored in PVC.
* Deleting `mysql-0` → recreated with **same name & storage**.

## 🧩 Naming & Identity

* **Stable name:** `mysql-0`, `mysql-1`
* **Stable hostname:** `mysql-0.mysql-service`
* **Dedicated PVC** per Pod → allows persistent data

## 🧱 Example — MySQL StatefulSet

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: mysql
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8
        ports:
        - containerPort: 3306
        volumeMounts:
        - name: mysql-storage
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-storage
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 5Gi
```

## 🔁 Internal Behavior

1. Creates `mysql-0` → attaches new PVC.
2. Creates `mysql-1`, `mysql-2` sequentially.
3. Deleting `mysql-1` → **recreated with same name & volume**.

✅ Data persists
✅ Name stable
✅ Ordered creation & deletion respected

## 💡 Importance

* Maintain **data consistency**
* Ensure **ordered startup/shutdown**
* Provide **stable network identities**
* Safely manage **replicated databases or clusters**

## ⚙️ Use Cases

| Application       | Purpose                       |
| ----------------- | ----------------------------- |
| MySQL/PostgreSQL  | Persistent data per instance  |
| MongoDB/Cassandra | Replica sets, distributed DBs |
| Kafka/RabbitMQ    | Queue/message persistence     |
| Elasticsearch     | Indexed data, unique node IDs |

## 🚀 Simple Summary

> StatefulSet = Deployment + Persistent Identity + Stable Storage

* Pod dies → Kubernetes recreates **exactly as before** (same name, same data).
