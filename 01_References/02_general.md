# Kubernetes: Three‑Tier App (Networking & Resource Mapping)

**Purpose:** concise reference mapping Docker 3‑tier patterns to Kubernetes primitives, with minimal manifests and best practices. Use this as a quick guide when designing frontend → backend → database systems on Kubernetes.

---

## 1. Conceptual mapping (Docker → Kubernetes)

| Concern           |                  Docker | Kubernetes                                                             |
| ----------------- | ----------------------: | ---------------------------------------------------------------------- |
| Run container     |            `docker run` | **Pod** (usually managed by **Deployment**)                            |
| App group         | `docker network` bridge | **Cluster network** (CNI plugin) — flat pod networking                 |
| Compose           |    `docker‑compose.yml` | **Deployment** + **Service** (+ ConfigMap/Secret)                      |
| Service discovery |         container names | **Service names** (DNS: `svc.cluster.local`)                           |
| Expose HTTP       |     Nginx reverse proxy | **Ingress** (Ingress Controller)                                       |
| Scaling           |                  manual | `kubectl scale` / `replicas` in Deployment                             |
| Persistence       |            local volume | **PersistentVolume** + **PersistentVolumeClaim** (or cloud/dynamic PV) |

---

## 2. High‑level architecture

```
[Ingress (HTTP)]
      ↓
[Frontend Deployment + Service]
      ↔
[Backend Deployment + Service]
      ↔
[Database StatefulSet/Deployment + ClusterIP + PVC]
```

* Frontend is typically exposed to users via an **Ingress** backed by a Service (ClusterIP).
* Backend and DB use ClusterIP Services for internal communication.
* Pods communicate via service DNS (e.g., `http://backend-service:8000`).

---

## 3. Key resources & roles (short)

* **Deployment** — desired replica count, rolling updates, self‑healing.
* **StatefulSet** — use for stateful apps needing stable network IDs & stable storage (recommended for many DBs).
* **Service (ClusterIP)** — internal stable endpoint and DNS name.
* **Service (NodePort / LoadBalancer)** — external exposure (labs / cloud).
* **Ingress** — HTTP(S) routing to Services.
* **ConfigMap / Secret** — configuration + credentials (use Secret for DB passwords).
* **PersistentVolume / PersistentVolumeClaim** — durable storage (avoid `emptyDir` for DBs — it's ephemeral).

---

## 4. Minimal examples (trimmed)

### Frontend Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: {name: frontend}
spec:
  replicas: 2
  selector: {matchLabels: {app: frontend}}
  template:
    metadata: {labels: {app: frontend}}
    spec:
      containers:
        - name: frontend
          image: myfrontend:latest
          ports: [{containerPort: 80}]
```

### Backend Deployment (env via Secret/Config)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: {name: backend}
spec:
  replicas: 2
  template:
    metadata: {labels: {app: backend}}
    spec:
      containers:
        - name: backend
          image: mybackend:latest
          env:
            - name: DB_URL
              value: "mongodb://db-service:27017/mydb"
          ports: [{containerPort: 8000}]
```

### DB (StatefulSet recommended) — simplified

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata: {name: db}
spec:
  serviceName: db
  replicas: 1
  template:
    metadata: {labels: {app: db}}
    spec:
      containers:
        - name: mongo
          image: mongo:6
          volumeMounts: [{name: data, mountPath: /data/db}]
  volumeClaimTemplates:
    - metadata: {name: data}
      spec: {accessModes: ["ReadWriteOnce"], resources: {requests: {storage: 5Gi}}}
```

### Services (ClusterIP)

```yaml
apiVersion: v1
kind: Service
metadata: {name: backend-service}
spec:
  selector: {app: backend}
  ports: [{port: 8000, targetPort: 8000}]
  type: ClusterIP
```

### Ingress (example)

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata: {name: three-tier-ingress}
spec:
  rules:
    - http:
        paths:
          - path: /frontend
            pathType: Prefix
            backend: {service: {name: frontend-service, port: {number: 80}}}
          - path: /api
            pathType: Prefix
            backend: {service: {name: backend-service, port: {number: 8000}}}
```

---

## 5. DNS & service addressing

* Service DNS format: `<service>.<namespace>.svc.cluster.local`.
* Within same namespace, `http://backend-service:8000` resolves automatically.
* Pods have their own IPs, but you should use Services for stability and load balancing.

---

## 6. Best practices (short)

* Use **StatefulSet + PVCs** for databases; avoid `emptyDir` for DBs.
* Store credentials in **Secrets**; inject via env or volumes.
* Use **readiness** & **liveness** probes for reliable rolling updates.
* Keep Services ClusterIP for internal-only components.
* Use Ingress (with TLS) for external HTTP(S) routing.
* Prefer cloud PVs or networked storage (EBS, GCE PD, NFS, Longhorn) for durability.
* Test scaling: ensure backend statelessness where possible.

---

## 7. API groups quick reference

| Resource Type                   |                 API Group | apiVersion                   |
| ------------------------------- | ------------------------: | ---------------------------- |
| Pod, Service, ConfigMap, Secret |                      core | v1                           |
| Deployment, StatefulSet         |                      apps | apps/v1                      |
| Role, RoleBinding, ClusterRole  | rbac.authorization.k8s.io | rbac.authorization.k8s.io/v1 |
| Ingress                         |         networking.k8s.io | networking.k8s.io/v1         |
| CRD                             |      apiextensions.k8s.io | apiextensions.k8s.io/v1      |

---

### Short checklist before deploy

* Secrets present (DB credentials)
* PVCs bound and `ReadWriteOnce` / adequate storage class
* Health probes configured
* Ingress controller installed for routing
* Namespace & RBAC correctly set

---

*This document is intentionally concise — expand any section into a separate file for deep dives (StatefulSets, Operators, backup strategies, etc.).*
