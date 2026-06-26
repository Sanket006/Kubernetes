# ☸️ Kubernetes ReplicaSet Manifests

This folder contains **Kubernetes ReplicaSet manifest examples**. A **ReplicaSet** ensures that a specified number of identical Pod replicas are running at any given time.

ReplicaSets provide **self-healing and scaling** for Pods. In modern Kubernetes configurations, ReplicaSets are typically **managed by Deployments**, but understanding them directly is essential for learning how Kubernetes controllers manage Pod lifecycles.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── ReplicaSet/
    ├── replicaset.yaml                 # Standard ReplicaSet manifest
    ├── nginx-replicaset-service.yaml   # Combined ReplicaSet and NodePort Service
    └── README.md                       # This documentation file
```

---

## 🧩 Core Fields Used in ReplicaSet YAML

* **apiVersion**: `apps/v1`
* **kind**: `ReplicaSet`
* **metadata**: Name, namespace, and labels to identify the ReplicaSet.
* **spec**: Specifies the controller's desired behavior, including:
  * **replicas**: Desired number of Pod replicas.
  * **selector**: Label selector to identify managed Pods (supports set-based selectors).
  * **template**: Pod template describing the Pods to launch when matching replicas are missing.

---

## 📘 Practical Examples

### 1. Standard ReplicaSet Manifest (`replicaset.yaml`)
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
  labels:
    app: nginx-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-app                         # Set-based selector
  template:
    metadata:
      labels:
        app: nginx-app                       # Must match the selector above
    spec:
      containers:
        - name: nginx-container
          image: nginx:1.25.3-alpine         # Pinned version (Best Practice)
          ports:
            - containerPort: 80
          resources:                         # Requests/limits (Best Practice)
            requests:
              memory: "64Mi"
              cpu: "100m"
            limits:
              memory: "128Mi"
              cpu: "200m"
```

---

## ▶️ kubectl Operations

### Apply the Manifests
```bash
kubectl apply -f kubernetes_manifest_files/ReplicaSet/
```

### Verify & Describe
```bash
# Get running ReplicaSets
kubectl get rs

# Get detailed description of a ReplicaSet
kubectl describe rs nginx-replicaset

# View Pods matching the selector label
kubectl get pods -l app=nginx-app
```

### Scaling Manually
```bash
kubectl scale rs nginx-replicaset --replicas=5
```

### Delete the ReplicaSet
```bash
kubectl delete -f kubernetes_manifest_files/ReplicaSet/
```

---

## 🆚 ReplicaSet vs ReplicationController

| Feature | ReplicaSet | ReplicationController |
| :--- | :--- | :--- |
| **Selector Type** | Set-based (supports `matchExpressions` like `In`, `NotIn`, `Exists`) | Equality-based only (e.g. `app=nginx`) |
| **API Version** | `apps/v1` | `v1` |
| **Managed by Deployment** | Yes, automatically created/updated | No, legacy only |

---

## 🛣️ Learning Path Navigation

    ⏮️ **[ReplicationController](../ReplicationController/README.md)** | ⏭️ **[Deployment](../Deployment/README.md)**
