# ☸️ Kubernetes Dynamic Provisioning PV Manifests

This folder contains **Kubernetes Dynamic Provisioning PV manifest examples**.

**Dynamic Provisioning** eliminates the need for cluster administrators to manually pre-allocate physical storage volumes and create corresponding `PersistentVolume` resources. Instead, storage is provisioned on-demand when a user creates a `PersistentVolumeClaim` referencing a defined **StorageClass**. Kubernetes coordinates with cloud-based or on-prem storage plug-ins to dynamically spin up physical volumes and bind them automatically to the requesting Pods.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Dynamic Provisioning PV/
    ├── StorageClass + PVC + Pod.yaml   # Modern ebs.csi.aws.com StorageClass, PVC, and Pod
    ├── Full-manifest-file.yaml          # Legacy aws-ebs gp2 StorageClass, PVC, and Pod
    └── README.md                       # This documentation file
```

---

## 🧩 Core Fields Used in Dynamic Provisioning YAML

* **apiVersion**: `storage.k8s.io/v1`
* **kind**: `StorageClass`
* **metadata.name**: Name identifying the StorageClass.
* **provisioner**: Defines the volume plugin/driver responsible for creating the physical volumes (e.g. `ebs.csi.aws.com` for AWS EBS CSI driver).
* **volumeBindingMode**:
  - `Immediate` (default): Provisioning and binding happen as soon as the PVC is created.
  - `WaitForFirstConsumer`: Postpones provisioning until a Pod requesting the PVC is scheduled. This is highly recommended for multi-zone clusters (e.g. AWS) to ensure the volume is created in the same Availability Zone where the Pod is running.
* **reclaimPolicy**: `Delete` (deletes volume when PVC is deleted) or `Retain`.

---

## 📘 Practical Examples

### 1. Modern Dynamic StorageClass & PVC Manifest (`StorageClass + PVC + Pod.yaml`)
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: sc-fast
provisioner: ebs.csi.aws.com                 # AWS EBS CSI driver provisioner
volumeBindingMode: WaitForFirstConsumer      # Zone-aware scheduling (Best Practice)
reclaimPolicy: Delete
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-fast
spec:
  storageClassName: sc-fast                  # Binds dynamically using the sc-fast StorageClass
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
```

---

## ▶️ kubectl Operations

### Apply the Manifests
```bash
# Note: Escape the folder name with double quotes or backslashes in your shell
kubectl apply -f "kubernetes_manifest_files/Dynamic Provisioning PV/"
```

### Verify Binding
```bash
# Verify that the StorageClass exists
kubectl get storageclass

# Check PVC status. If volumeBindingMode is 'WaitForFirstConsumer', status will stay
# 'Pending' until the matching Pod is deployed
kubectl get pvc
```

### Delete Resources
```bash
# Deleting the PVC automatically triggers deletion of the dynamically created PV and physical disk
kubectl delete -f "kubernetes_manifest_files/Dynamic Provisioning PV/"
```

---

## 🛡️ Best Practices & Common Mistakes

* **Use WaitForFirstConsumer Binding:** In multi-zone clusters (such as AWS, GCP, or Azure), always use `volumeBindingMode: WaitForFirstConsumer`. If set to `Immediate`, Kubernetes may allocate the physical volume in Zone A, but schedule the Pod on a node in Zone B, preventing the Pod from attaching the disk.
* **Legacy vs Modern AWS Drivers:** The legacy `kubernetes.io/aws-ebs` provisioner (gp2) is deprecated. Use the modern container storage interface (CSI) driver `ebs.csi.aws.com` for new deployments.

---

## 🛣️ Learning Path Navigation

    ⏮️ **[Static Provisioning PV](../Static%20Provisioning%20PV/README.md)** | ⏭️ **[Root Overview](../../README.md)**
