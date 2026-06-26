# ☸️ Kubernetes PersistentVolume (PV) & PVC Manifests

This folder contains **Kubernetes PersistentVolume (PV) and PersistentVolumeClaim (PVC) manifest examples**. 

In Kubernetes, container filesystems are ephemeral. If a container crashes or is rescheduled, all files generated during its execution are lost. PV and PVC resources decouple storage implementation details from application specifications, allowing stateful workloads to persist data beyond the lifecycle of individual Pods.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── PersistentVolume/
    ├── pv.yaml                             # Static HostPath PV definition
    ├── pvc.yaml                            # PVC matching static PV parameters
    ├── pv-pvc.yaml                         # Combined PV and PVC manifest
    ├── pv-pvc-pod.yaml                     # Pod mounting static HostPath PVC
    ├── pv-pvc-deployment.yaml              # Deployment binding to HostPath PVC
    ├── pv-pvc-pod-using-node-affinity.yaml # Local PV with node selector rules
    └── README.md                           # This documentation file
```

---

## 🧩 Core Fields Used in PV & PVC YAML

### PersistentVolume (PV)
* **apiVersion**: `v1`
* **kind**: `PersistentVolume`
* **spec.capacity.storage**: Total capacity allocated (e.g. `5Gi`).
* **spec.accessModes**:
  - `ReadWriteOnce` (RWO): Mounted read-write by a single node.
  - `ReadOnlyMany` (ROM): Mounted read-only by many nodes.
  - `ReadWriteMany` (RWM): Mounted read-write by many nodes.
* **spec.persistentVolumeReclaimPolicy**:
  - `Retain` (default): PV remains; data must be manually cleaned.
  - `Delete`: Automatically deletes the backend physical storage (common in cloud).
  - `Recycle`: Performs basic scrub (`rm -rf`) and makes volume available again.
* **spec.hostPath**: Local path on the worker node hosting the volume (for development only).

### PersistentVolumeClaim (PVC)
* **apiVersion**: `v1`
* **kind**: `PersistentVolumeClaim`
* **spec.resources.requests.storage**: Requested volume capacity.
* **spec.storageClassName**: Storage class reference (must match the target PV).

---

## 📘 Practical Examples

### 1. Combined PV & PVC Manifest (`pv-pvc.yaml`)
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-local
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual                   # Matches storageClassName in PVC
  hostPath:
    path: /mnt/data                          # Local directory on worker node
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-local
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: manual                   # Matches storageClassName in PV
```

---

## ▶️ kubectl Operations

### Apply the Manifests
```bash
# Apply PV, PVC, and mounting Pod configs
kubectl apply -f kubernetes_manifest_files/PersistentVolume/
```

### Verify Bindings
```bash
# View Persistent Volumes (Status should show 'Bound')
kubectl get pv

# View Persistent Volume Claims (Status should show 'Bound')
kubectl get pvc
```

### Delete Storage Resources
```bash
# Delete the PVC first, then the PV
kubectl delete -f kubernetes_manifest_files/PersistentVolume/
```

---

## 🛡️ Best Practices & Common Mistakes

* **Reclaim Policies:** In production, use `Retain` or `Delete` depending on whether data safety or automated cleanup is preferred. Never use `Recycle` as it is deprecated.
* **AccessMode Restrictions:** Local volumes (like `hostPath` or `local` directories) only support `ReadWriteOnce`. They cannot be shared across multiple nodes.
* **Match storageClassNames:** Ensure that both the static PV and the PVC define matching `storageClassName` properties (e.g. `manual`) to prevent Kubernetes from attempting dynamic provisioning using the default storage class.

---

## 🛣️ Learning Path Navigation

    ⏮️ **[HPA](../HPA/README.md)** | ⏭️ **[Static Provisioning PV](../Static%20Provisioning%20PV/README.md)**
