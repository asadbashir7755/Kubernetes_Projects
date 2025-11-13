# Base Understanding of Kubernetes Storage

## 🧱 Setup Overview

Your setup structure:

```
Host Machine (Physical)
│
└── Virtual Machines (VMs)
     ├── Control-plane node (master)
     ├── Worker node 1
     ├── Worker node 2
     └── Worker node 3
```

Each worker node runs its own Linux OS with **kubelet**, a **container runtime** (Docker or containerd), and pods.

When a **Pod** writes to `/mnt/data` (via a `hostPath` volume), it writes inside that node’s local filesystem — **not** your physical host.

### 💾 Where is `/mnt/data` stored?

Data path:

```
Pod → /mnt/data (container)
│
└── hostPath /mnt/data (worker node)
     │
     └── virtual disk (e.g., .qcow2, .vmdk)
          │
          └── stored inside host machine as a VM image file
```

So the data ends up on your host **inside the VM’s disk image**, not in a visible folder.

### 🧰 Accessing `/mnt/data`

You cannot access it directly from your host; instead, use one of these:

```bash
kubectl debug node/<node-name> --image=busybox
# OR
ssh <node-vm>
# OR
virsh console <node-name>
```

---

## ⚙️ Problem Scenario

You have **1 control-plane + 5 worker nodes**. Some pods store data in **PersistentVolumes (PVs)** using `hostPath`. When scaling down (removing worker nodes), you fear losing data.

### 🧨 Issue — hostPath is Node-Bound

A `hostPath` volume keeps data **only on that specific node’s disk**:

* If node is removed from cluster → data remains but pods can’t reach it.
* If VM or node is deleted → data is **lost permanently**.

So `hostPath` is suitable **only for testing**, not production.

---

## 🧭 Production Storage Solutions

Use storage independent of any node.

| Storage Type                  | Example                            | Description                   |
| ----------------------------- | ---------------------------------- | ----------------------------- |
| **NFS (Network File System)** | Shared drive                       | Accessible from all nodes     |
| **Cloud Block Storage**       | AWS EBS, GCP Disk, Azure Disk      | Auto-attaches to new node     |
| **Distributed Storage**       | Ceph, GlusterFS, Longhorn, OpenEBS | Replicated & fault-tolerant   |
| **Dynamic StorageClass**      | NFS/EBS provisioners               | Creates volumes automatically |

---

## 🧠 Safe Node Removal Options

### **Option 1: Move Data to Shared Storage**

1. Create a PV using NFS or another shared backend.
2. Bind a PVC to it.
3. Copy old data from hostPath PV to new PV (via `rsync` or a temporary pod).
4. Update deployments to use the new PVC.
5. Safely delete the old nodes.

### **Option 2: Backup Before Deletion**

If still using `hostPath`:

```bash
kubectl drain <node-name> --ignore-daemonsets
ssh <node>
tar czvf /tmp/backup.tar.gz /mnt/data
```

Then copy the backup elsewhere before removing the node.

---

## 🧰 Summary

| Case                  | Data Location           | If Node Deleted | Recommended Fix             |
| --------------------- | ----------------------- | --------------- | --------------------------- |
| hostPath PV           | Inside node (/mnt/data) | ❌ Lost          | Move to NFS / Cloud storage |
| Dynamic PV            | Cloud disk              | ✅ Persists      | Auto reattaches             |
| NFS / Ceph / Longhorn | Shared storage          | ✅ Available     | Best for on-prem            |

### ✅ Conclusion

Before removing worker nodes, **migrate data** to shared or cloud-based storage. Avoid using `hostPath` for any environment where scaling or node replacement may occur.

---

## ⚡ Local PVs and Node Affinity

When you use **local storage PVs**, Kubernetes must ensure pods run on the node that owns that PV.

### 💡 Why

Local PVs reference a physical path on a node. If the pod runs elsewhere, the data path doesn’t exist → pod fails.

### ✅ Solution — Use Node Affinity

Specify node affinity in the PV definition.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: localpv-worker1
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: local-storage
  persistentVolumeReclaimPolicy: Retain
  local:
    path: /mnt/data
  nodeAffinity:
    required:
      nodeSelectorTerms:
        - matchExpressions:
            - key: kubernetes.io/hostname
              operator: In
              values:
                - worker1
```

This ensures Kubernetes schedules any pod using this PV **only** on `worker1`, where the data physically exists.
