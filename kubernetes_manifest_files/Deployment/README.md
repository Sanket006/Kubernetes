# ☸️ Kubernetes Deployment Manifests

This folder contains **Kubernetes Deployment manifest examples**. A **Deployment** manages the lifecycle of Pods and ReplicaSets, providing declarative updates, self-healing, scaling, and rollback capabilities.

Deployments are the **standard recommended way to run stateless workloads** in production because they automate all ReplicaSet management and rollout processes under the hood.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Deployment/
    ├── deployment.yaml                 # Basic Deployment manifest (nginx)
    ├── nginx-deployment-service.yaml   # Combined Deployment and NodePort Service
    └── README.md                       # This documentation file
```

---

## 🧩 Core Fields Used in Deployment YAML

* **apiVersion**: `apps/v1`
* **kind**: `Deployment`
* **metadata**: Name, namespace, and labels to identify the Deployment.
* **spec**: Specifies the Deployment behavior:
  * **replicas**: Desired number of Pod replicas.
  * **selector**: Match labels used to identify Pods managed by the Deployment.
  * **strategy**: Rollout/update configuration:
    * **type**: `RollingUpdate` (default, zero-downtime) or `Recreate` (drops all old pods first).
    * **rollingUpdate**: Configures `maxSurge` and `maxUnavailable` boundaries.
  * **template**: Pod template describing container images, ports, and resource specs.

---

## 📘 Practical Examples

### 1. Production-Ready Deployment Manifest (`deployment.yaml`)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx-app
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1                            # Max pods created above target during rollout
      maxUnavailable: 1                      # Max pods down during rollout
  selector:
    matchLabels:
      app: nginx-app
  template:
    metadata:
      labels:
        app: nginx-app
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
kubectl apply -f kubernetes_manifest_files/Deployment/
```

### Rollout Management (Updates & Rollbacks)
```bash
# Check the status of a running update rollout
kubectl rollout status deployment/nginx-deployment

# View the deployment rollout history
kubectl rollout history deployment/nginx-deployment

# Roll back to the previous deployment version
kubectl rollout undo deployment/nginx-deployment
```

### Scale Workload
```bash
kubectl scale deployment nginx-deployment --replicas=5
```

### Verify & Describe
```bash
kubectl get deployments
kubectl get replicasets
kubectl describe deployment nginx-deployment
```

### Delete Deployment
```bash
kubectl delete -f kubernetes_manifest_files/Deployment/
```

---

## 🔑 Deployment Strategies

1. **RollingUpdate (Default)**:
   - Replaces old pods with new ones gradually.
   - Ensures zero downtime by keeping a fraction of pods running at all times.
2. **Recreate**:
   - Deletes all active pods before creating the new version.
   - Causes temporary service downtime but avoids running multiple code versions simultaneously.

---

## 🛣️ Learning Path Navigation

⏮️ **[ReplicaSet](../ReplicaSet/README.md)** | ⏭️ **[DaemonSet](../DaemonSet/README.md)**
