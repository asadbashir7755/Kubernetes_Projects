# 🧠 Kubernetes Volumes — Persistent Storage

## 🔹 Why Use Volumes

* Pods are ephemeral — data inside a Pod is lost when the Pod is deleted.
* **Volumes** allow data to persist beyond the life of a Pod.
* Bind storage to the host or cloud to secure and persist data.

## 🔹 Key Concepts

### PersistentVolume (PV)

* A real piece of storage (directory, disk, cloud volume) managed by Kubernetes.
* Think of it as a physical drive inside Kubernetes.
* Created once and remains available for Pods.

### PersistentVolumeClaim (PVC)

* A Pod requests storage via a **PVC**.
* Kubernetes matches PVC with an available PV.

### Flow of PV and PVC

1. Create a **PV** (storage available in the cluster).
2. Create a **PVC** (request for storage).
3. When creating a Pod:

   * The Pod container uses the PVC to mount storage.
   * Kubernetes checks which node the PV is assigned to.
   * Pod is scheduled on that node to access the PV.

### Access Modes

* **ReadWriteOnce (RWO):** Only one Pod can access the PV at a time.
* Other modes include ReadOnlyMany (ROX) and ReadWriteMany (RWX) depending on use case.

### Important Notes

* Replicas > 1 with RWO → only one Pod can access the PV.
* Ensure storage class and access mode match your workload.

This mechanism ensures data persistence and proper scheduling of Pods according to the PV location and access mode.
