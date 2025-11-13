# 🧠 Kubernetes Core Concepts — Simplified Revision Notes

## 🟩 Why We Create a Cluster

A **cluster** is where all Kubernetes resources run — like a company:

* **Cluster** → Company building 🏢
* **Control Plane** → Management 👨‍💼
* **Worker Nodes** → Employees 👷‍♂️
* **Pods/Deployments** → Projects inside

Without the cluster, there’s no place to run Pods or Deployments.

### ⚙️ Cluster Components

* **Control Plane (Master)** → Manages scheduling, scaling, health.
* **Worker Nodes** → Run Pods, managed by control plane.
* Services on each node: `kubelet`, `kube-proxy`.

### 🧩 Why Multiple Workers?

| Setup     | Use                  |
| --------- | -------------------- |
| 1 Node    | Local testing        |
| 3 Nodes   | Simulated production |
| 10+ Nodes | Real production      |

✅ 3 nodes show load balancing, replication, and failover.

### 🏗️ Logical Flow

1. Cluster → foundation
2. Namespace → logical separation
3. Deployment → define app
4. Pod → actual container(s)

---

## 🧱 Pods — Smallest Unit

A **Pod** wraps one or more containers.
🏠 Pod = house, 🧑‍🍳 Containers = people inside.

Shared features:

* Same network (IP/ports)
* Same storage (volumes)
* Same node (host)

| Feature         | Managed by Pod |
| --------------- | -------------- |
| Networking      | ✅              |
| Storage         | ✅              |
| Health checks   | ✅              |
| Resource limits | ✅              |
| Scaling         | ✅              |

---

## 🧩 Namespaces

Used to separate environments:

| Env        | Namespace | Use            |
| ---------- | --------- | -------------- |
| 🧑‍💻 Dev  | `dev`     | For developers |
| 🧪 Staging | `staging` | Pre-production |
| 🚀 Prod    | `prod`    | Live system    |

---

## 🔄 Rolling Update

When you update a Deployment image:

1. New **ReplicaSet** created.
2. New Pods launch, old ones removed.
3. Update happens gradually — **no downtime**.

---

## ⚙️ ReplicaSet vs Deployment

| Feature         | ReplicaSet | Deployment |
| --------------- | ---------- | ---------- |
| Keep Pods alive | ✅          | ✅          |
| Rolling update  | ❌          | ✅          |
| Rollback        | ❌          | ✅          |
| Production use  | ❌          | ✅          |

* **ReplicaSet:** Keeps fixed pod count.
* **Deployment:** Manages ReplicaSets + updates.

---

## 🧱 DaemonSet

Runs **one Pod per node** (e.g., for logging or monitoring).

| Feature   | Deployment        | DaemonSet           |
| --------- | ----------------- | ------------------- |
| Run count | Multiple replicas | One per node        |
| Example   | Web app           | Fluentd, kube-proxy |

✅ Used for system agents that must exist on every node.

---

## ⚙️ Pod States

* **Running** → Active
* **Completed** → Finished job
* **Terminating** → Shutting down
* **Error / ImagePullBackOff** → Failed image or config

---

## 🧩 Jobs

Run **one-time tasks** like:

* 🧹 DB migration
* 🧪 Test automation
* 📤 Backup to S3
* 📦 Batch data jobs
* 🔄 Cleanup scripts

**Why Jobs?** Portable, isolated, schedulable.

**BusyBox** = tiny Linux image with essential tools.

---

## ⏰ CronJobs

Used for **scheduled tasks** (like daily backups).

Example:

```
00 12 1 3 5 → At 12:00 on March 1 (Friday)
```

Old job Pods auto-cleaned to save space.

---

## 🏷️ Labels & Selectors

`selector.matchLabels` → Filters Pods.
`template.metadata.labels` → Tags Pods.

🔗 Links: Deployment → ReplicaSet → Pod (via label `app=nginx`).

---

✅ **Summary:**

* Cluster = Base
* Pod = Smallest unit
* Deployment = Manages versions
* DaemonSet = One Pod per node
* Job/CronJob = One-time or scheduled tasks
* Namespace = Isolation
* Labels = Connection mechanism
