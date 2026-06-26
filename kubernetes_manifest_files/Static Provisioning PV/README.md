# ☸️ Kubernetes Static Provisioning PV Manifests

This folder contains **Kubernetes Static Provisioning PV manifest examples**. 

**Static Provisioning** means a cluster administrator manually provisions raw physical storage resources (such as AWS EBS volumes, local disks, or NFS folders) and creates `PersistentVolume` resources in the cluster ahead of time. Developers then create `PersistentVolumeClaims` that declare storage requirements matching the capacity and parameters of these pre-created PVs.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Static Provisioning PV/
    ├── AWS EBS + PV + PVC Manifest File.yaml  # Combined static AWS EBS volume manifest
    └── README.md                              # This documentation file
```

---

## 🧩 Core Fields Used in Static Provisioning YAML

* **apiVersion**: `v1`
* **kind**: `PersistentVolume` / `PersistentVolumeClaim` / `Pod`
* **spec.awsElasticBlockStore** (PV specific):
  - **volumeID**: The physical AWS EBS volume identifier (e.g. `vol-0abcd1234ef567890`).
  - **fsType**: Filesystem type to initialize (e.g. `ext4`).
* **spec.nodeAffinity** (PV specific): Restricts volume mounting to specific Availability Zones (AZ) matching the physical EBS storage location.

---

## 📘 Practical Examples

### 1. Static AWS EBS Volume Manifest (`AWS EBS + PV + PVC Manifest File.yaml`)
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: ebs-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: ""                        # Empty string prevents dynamic matching
  awsElasticBlockStore:
    volumeID: vol-0abcd1234ef567890           # Physical AWS EBS volume ID
    fsType: ext4
  nodeAffinity:
    required:
      nodeSelectorTerms:
        - matchExpressions:
            - key: topology.kubernetes.io/zone
              operator: In
              values:
                - ap-south-1a                 # EBS volume must match the Node AZ
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ebs-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: ""                        # Binds manually to the empty storageClass PV above
```

---

## ▶️ kubectl Operations

### Apply the Manifests
```bash
# Note: Escape the folder name with double quotes or backslashes in your shell
kubectl apply -f "kubernetes_manifest_files/Static Provisioning PV/AWS EBS + PV + PVC Manifest File.yaml"
```

### Verify Binding
```bash
# Verify the PVC binds successfully to the static EBS PV
kubectl get pv,pvc
```

### Delete Resources
```bash
kubectl delete -f "kubernetes_manifest_files/Static Provisioning PV/AWS EBS + PV + PVC Manifest File.yaml"
```

---

## 🛡️ Best Practices & Common Mistakes

* **Zone Match constraint:** Physical block storage (like AWS EBS or GCP Persistent Disk) is bound to a specific availability zone (AZ). If your cluster nodes span multiple zones, you **must** use `nodeAffinity` in your static PV to guarantee Pods are only scheduled on nodes located in the same zone as the disk.
* **StorageClass Override:** To bind manually to a static PV without a dynamic storage class overriding the request, set `storageClassName: ""` (empty string) in both the PV and the PVC.

---

## 🛣️ Learning Path Navigation

    ⏮️ **[PersistentVolume](../PersistentVolume/README.md)** | ⏭️ **[Dynamic Provisioning PV](../Dynamic%20Provisioning%20PV/README.md)**
