# ☸️ Kubernetes ReplicationController Manifests

This folder contains **Kubernetes ReplicationController (RC) manifest examples**. A **ReplicationController** ensures that a specified number of Pod replicas are running at any given time.

> ⚠️ **Important Node:** The `ReplicationController` is a legacy Kubernetes controller that has been replaced by `ReplicaSet` and `Deployment`. It is kept here as a learning tool to understand the historical evolution of Kubernetes replication and self-healing systems.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── ReplicationController/
    ├── replicationcontroller.yaml   # Basic ReplicationController manifest
    ├── nginx-rc-service.yaml        # Combined RC and NodePort Service manifest
    └── README.md                    # This documentation file
```

---

## 🧩 Core Fields Used in ReplicationController YAML

* **apiVersion**: `v1`
* **kind**: `ReplicationController`
* **metadata**: Name and labels for identifying the controller.
* **spec**: Specifies the configuration, including:
  * **replicas**: Desired number of Pod replicas.
  * **selector**: Label selector to match Pods (supports equality-based selectors only).
  * **template**: Pod template describing the Pods to launch.

---

## 📘 Practical Examples

### 1. Standard ReplicationController (`replicationcontroller.yaml`)
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
spec:
  replicas: 3
  selector:
    app: nginx-app                           # Equality-based label selector
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
kubectl apply -f kubernetes_manifest_files/ReplicationController/
```

### Verify & Describe
```bash
# Get list of ReplicationControllers
kubectl get rc

# Describe a specific ReplicationController
kubectl describe rc nginx-rc

# View Pods managed by the controller
kubectl get pods -l app=nginx-app
```

### Delete the Controller
```bash
kubectl delete -f kubernetes_manifest_files/ReplicationController/
```

---

## 🆚 ReplicationController vs ReplicaSet

| Feature | ReplicationController | ReplicaSet |
| :--- | :--- | :--- |
| **Selector Type** | Equality-based only (e.g. `app=nginx-app`) | Set-based support (e.g. `app in (nginx-app, web)`) |
| **Production Use** | Legacy / Not recommended | Modern standard (managed by Deployments) |
| **API Version** | `v1` | `apps/v1` |

---

## 🛣️ Learning Path Navigation

⏮️ **[Pod](../Pod/README.md)** | ⏭️ **[ReplicaSet](../ReplicaSet/README.md)**
