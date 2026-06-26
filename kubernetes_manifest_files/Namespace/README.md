# ☸️ Kubernetes Namespace Manifests

This folder contains **Kubernetes Namespace manifest examples**. A **Namespace** provides a way to logically partition and isolate resources within a single Kubernetes cluster.

Namespaces allow multiple teams, projects, or environments (such as dev, staging, production) to coexist within the same cluster without resource name collisions. They are also the primary boundary for resource quotas and Role-Based Access Control (RBAC).

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Namespace/
    ├── namespace.yaml           # Basic Namespace definition
    ├── namespace-pod.yaml       # Nginx Pod deployed inside a custom Namespace
    └── README.md                # This documentation file
```

---

## 🧩 Core Fields Used in Namespace YAML

* **apiVersion**: `v1`
* **kind**: `Namespace`
* **metadata**: Specifies the name of the Namespace (unique across the cluster) and labels.

---

## 📘 Practical Examples

### 1. Namespace Creation (`namespace.yaml`)
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev-team
  labels:
    env: development
    team: backend
```

### 2. Scoped Pod Manifest (`namespace-pod.yaml`)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  namespace: dev-team                       # Deploys this Pod in the 'dev-team' namespace
  labels:
    app: nginx-app
spec:
  containers:
    - name: nginx-container
      image: nginx:1.25.3-alpine             # Pinned version (Best Practice)
      ports:
        - containerPort: 80
      resources:                             # Requests/limits (Best Practice)
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
# Apply both the Namespace and the scoped Pod
kubectl apply -f kubernetes_manifest_files/Namespace/
```

### Verify & Describe
```bash
# List all Namespaces in the cluster
kubectl get namespaces

# Describe Namespace configurations
kubectl describe namespace dev-team

# View Pods in the specific namespace (defaults to 'default' otherwise)
kubectl get pods -n dev-team
```

### Set Default Namespace for Current Context
To avoid typing `-n dev-team` on every command, set the namespace context:
```bash
kubectl config set-context --current --namespace=dev-team
```

### Delete Namespace (Warning: Deletes all resources inside)
```bash
# Deleting a Namespace will automatically clean up all Pods, Services, and Deployments inside it
kubectl delete namespace dev-team
```

---

## 🛣️ Learning Path Navigation

    ⏮️ **[API Gateway](../API%20Gateway/README.md)** | ⏭️ **[ConfigMap](../ConfigMap/README.md)**
