# ☸️ Kubernetes StatefulSet Manifests

This folder contains **Kubernetes StatefulSet manifest examples**. A **StatefulSet** is a workload API object used to manage stateful applications, managing the deployment and scaling of a set of Pods, and providing guarantees about their ordering and uniqueness.

Unlike a Deployment, a StatefulSet maintains a sticky identity for each of its Pods. These pods are created from the same spec, but are not interchangeable: each has a persistent identifier that it maintains across any rescheduling.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── StatefulSet/
    ├── mysql-statefulset.yaml                  # MySQL StatefulSet skeleton (plain-text env)
    ├── mysql-statefulset-persistentvolume.yaml # MySQL StatefulSet with PVC and Headless Service
    ├── nginx-statefulset-service.yaml          # Nginx StatefulSet with PVC and Headless Service
    └── README.md                               # This documentation file
```

---

## 🧩 Core Fields Used in StatefulSet YAML

* **apiVersion**: `apps/v1`
* **kind**: `StatefulSet`
* **metadata**: Name, namespace, and labels to identify the StatefulSet.
* **spec**: Specifies the StatefulSet behavior:
  * **serviceName**: The name of the Headless Service that governs this StatefulSet (required for Pod DNS).
  * **replicas**: Desired number of Pods.
  * **selector**: Match labels used to identify Pods managed by the StatefulSet.
  * **template**: Pod template describing the containers.
  * **volumeClaimTemplates**: A list of PVC templates that Kubernetes will use to dynamically or statically bind persistent volumes to each replica Pod (e.g. `mysql-persistent-storage-mysql-statefulset-0`).

---

## 📘 Practical Examples

### 1. Nginx StatefulSet Manifest (`nginx-statefulset-service.yaml`)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-headless
  labels:
    app: nginx-stateful
spec:
  ports:
    - port: 80
      name: web
  clusterIP: None                            # None = Headless Service (Required)
  selector:
    app: nginx-stateful
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: nginx-statefulset
spec:
  serviceName: "nginx-headless"              # Links to the Headless Service above
  replicas: 3
  selector:
    matchLabels:
      app: nginx-stateful
  template:
    metadata:
      labels:
        app: nginx-stateful
    spec:
      containers:
        - name: nginx-container
          image: nginx:1.25.3-alpine         # Pinned version (Best Practice)
          ports:
            - containerPort: 80
              name: web
          volumeMounts:
            - name: nginx-storage
              mountPath: /usr/share/nginx/html
          resources:                         # Requests/limits (Best Practice)
            requests:
              memory: "64Mi"
              cpu: "100m"
            limits:
              memory: "128Mi"
              cpu: "200m"
  volumeClaimTemplates:                      # Volume template for each Pod instance
    - metadata:
        name: nginx-storage
      spec:
        accessModes: [ "ReadWriteOnce" ]
        storageClassName: "manual"
        resources:
          requests:
            storage: 1Gi
```

---

## ▶️ kubectl Operations

### Apply the Manifests
```bash
kubectl apply -f kubernetes_manifest_files/StatefulSet/
```

### Verify & Describe
```bash
# Get StatefulSets
kubectl get statefulsets

# Describe a specific StatefulSet
kubectl describe statefulset mysql-statefulset

# View the stable hostnames and IP addresses of StatefulSet Pods
kubectl get pods -l app=mysql -o wide
```

### Scaling StatefulSets
```bash
kubectl scale statefulset mysql-statefulset --replicas=3
```

### Delete StatefulSet (Note: PVCs are retained)
```bash
# Deleting a StatefulSet does NOT delete its PVCs/PVs (Best practice for data safety)
kubectl delete -f kubernetes_manifest_files/StatefulSet/
```

---

## 🛡️ Best Practices & Common Mistakes

* **Governing Headless Service:** Always define a governing Headless Service (`clusterIP: None`) matching `spec.serviceName` to ensure each Pod gets a stable DNS record (e.g. `pod-name.service-name.namespace.svc.cluster.local`).
* **Handle Persistent Volumes Wisely:** When scaling down or deleting a StatefulSet, Kubernetes does not automatically delete the corresponding `PersistentVolumeClaims`. You must clean these up manually if you no longer need the data.
* **Avoid Plain-Text Passwords:** The provided `mysql-statefulset.yaml` contains hardcoded plain-text credentials for learning purposes. In real-world environments, refactor these to consume values from `Secrets` via `secretKeyRef` (as demonstrated in the [Secret Directory](../Secret/README.md)).

---

## 🛣️ Learning Path Navigation

    ⏮️ **[DaemonSet](../DaemonSet/README.md)** | ⏭️ **[Service](../Service/README.md)**
