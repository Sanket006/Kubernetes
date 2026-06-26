# ☸️ Kubernetes DaemonSet Manifests

This folder contains **Kubernetes DaemonSet manifest examples**. A **DaemonSet** ensures that a copy of a Pod runs on all (or selected) nodes in a cluster. 

DaemonSets are typically used for cluster-level services and infrastructure agents, such as log collectors (e.g., Fluentd, Logstash), monitoring agents (e.g., Prometheus node exporter), and networking plugins (e.g., Calico).

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── DaemonSet/
    ├── daemonset.yaml                 # Basic DaemonSet manifest (nginx)
    ├── nginx-daemonset-service.yaml   # Combined DaemonSet and NodePort Service
    └── README.md                      # This documentation file
```

---

## 🧩 Core Fields Used in DaemonSet YAML

* **apiVersion**: `apps/v1`
* **kind**: `DaemonSet`
* **metadata**: Name, namespace, and labels to identify the DaemonSet.
* **spec**: Specifies the DaemonSet configuration:
  * **selector**: Match labels used to identify Pods managed by the DaemonSet.
  * **template**: Pod template describing the containers to launch.
    * **spec.tolerations**: Allows DaemonSet Pods to run on tainted nodes (like master/control-plane nodes).
  * **updateStrategy**:
    * **type**: `RollingUpdate` (default) or `OnDelete`.

---

## 📘 Practical Examples

### 1. Production-Ready DaemonSet Manifest (`daemonset.yaml`)
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
  labels:
    app: nginx-daemon
spec:
  selector:
    matchLabels:
      app: nginx-daemon
  template:
    metadata:
      labels:
        app: nginx-daemon
    spec:
      tolerations:
        - key: node-role.kubernetes.io/control-plane
          operator: Exists
          effect: NoSchedule
        - key: node-role.kubernetes.io/master
          operator: Exists
          effect: NoSchedule
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
kubectl apply -f kubernetes_manifest_files/DaemonSet/
```

### Verify & Describe
```bash
# Get list of running DaemonSets
kubectl get ds

# View detailed information of a DaemonSet
kubectl describe ds nginx-daemonset

# Check that Pods run on all nodes (compare count to node count)
kubectl get pods -o wide
```

### Delete DaemonSet
```bash
kubectl delete -f kubernetes_manifest_files/DaemonSet/
```

---

## 🛣️ Learning Path Navigation

⏮️ **[Deployment](../Deployment/README.md)** | ⏭️ **[StatefulSet](../StatefulSet/README.md)**
