# Kubernetes Dynamic Provisioning PV Manifests

This folder contains **Kubernetes PersistentVolume (PV) manifests for Dynamic Provisioning**.

**Dynamic provisioning** allows Kubernetes to automatically provision storage when a PersistentVolumeClaim (PVC) is created, using a **StorageClass**.

This eliminates the need for cluster administrators to manually create PersistentVolumes ahead of time.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Dynamic Provisioning PV/
    ├── Full-manifest-file.yaml   
    ├── StorageClass + PVC + Pod.yaml   
    └── README.md
```

This folder includes YAML files defining StorageClass, PV, and PVC resources for dynamic provisioning.

---

## 📘 What is Dynamic Provisioning in Kubernetes?

**Dynamic Provisioning** automatically creates PVs when a PVC is requested. It relies on a **StorageClass** that defines the storage backend, parameters, and reclaim policy.

Key points:

* No manual creation of PVs required
* PVC triggers automatic PV creation
* StorageClass defines the storage type and behavior
* Supports cloud-based and on-prem storage systems

In simple terms:

> **Dynamic provisioning lets you request storage on demand without manually creating PVs.**

---

## 🎓 What You Learn from Dynamic Provisioning

By working with dynamic provisioning manifests, you will understand:

* How StorageClass defines storage backend and parameters
* How PVC automatically triggers PV creation
* Reclaim policies for dynamically provisioned PVs
* Integration with StatefulSets and Pods

---

## 🧩 Core Fields Used in StorageClass YAML

* **apiVersion**: `storage.k8s.io/v1`
* **kind**: `StorageClass`
* **metadata.name**: Name of the StorageClass
* **provisioner**: Storage backend (e.g., `kubernetes.io/aws-ebs`, `kubernetes.io/gce-pd`, `kubernetes.io/no-provisioner`)
* **parameters**: Optional backend-specific configuration
* **reclaimPolicy**: `Delete` (default) or `Retain`
* **volumeBindingMode**: `Immediate` or `WaitForFirstConsumer`

**Example StorageClass YAML:**

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-storage
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
reclaimPolicy: Delete
volumeBindingMode: Immediate
```

---

## 🧩 Core Fields Used in PVC YAML (Dynamic Provisioning)

* **apiVersion**: `v1`
* **kind**: `PersistentVolumeClaim`
* **metadata.name**: PVC name
* **spec.accessModes**: `ReadWriteOnce`, `ReadOnlyMany`, `ReadWriteMany`
* **spec.resources.requests.storage**: Requested storage size
* **spec.storageClassName**: Name of StorageClass to trigger dynamic PV provisioning

**Example PVC YAML:**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-dynamic-example
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: fast-storage
```

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply StorageClass:

```bash
kubectl apply -f kubernetes_manifest_files/Dynamic\ Provisioning\ PV/storageclass-*.yaml
```

Apply PVC manifests:

```bash
kubectl apply -f kubernetes_manifest_files/Dynamic\ Provisioning\ PV/pvc-*.yaml
```

Verify dynamically provisioned PVs and PVCs:

```bash
kubectl get pv
kubectl get pvc
```

Describe PV or PVC:

```bash
kubectl describe pv <pv-name>
kubectl describe pvc <pvc-name>
```

Delete PV, PVC, or StorageClass:

```bash
kubectl delete pvc <pvc-name>
kubectl delete pv <pv-name>
kubectl delete storageclass <storageclass-name>
```

---

## 🔍 Dynamic Provisioning Working Flow

1. Define a StorageClass for dynamic provisioning
2. User creates a PVC specifying the StorageClass
3. Kubernetes automatically provisions a PV that matches the PVC
4. PVC binds to the PV
5. Pod uses PVC to access storage

```
StorageClass → PVC → Dynamically Provisioned PV → Pod → Persistent Storage
```

---

## 🧪 Common Use Cases

* Cloud-based workloads needing on-demand storage
* StatefulSets that require dynamically allocated persistent volumes
* Avoiding manual PV creation in multi-tenant clusters

---

## ⚠️ Common Mistakes

* Missing or incorrect StorageClass in PVC
* Unsupported provisioner for the cluster
* Incorrect access mode or storage size
* Not understanding reclaim policies (Delete vs Retain)

---

## ✅ Best Practices

* Use StorageClass for all dynamically provisioned storage
* Choose appropriate `reclaimPolicy` for backup and retention
* Monitor dynamically provisioned PVs and PVCs
* Use `WaitForFirstConsumer` binding mode for zone-aware storage
* Combine with StatefulSets for stateful workloads

---

## 🛣️ Learning Path Linkage

### Before Dynamic Provisioning PV

➡️ **PersistentVolume / PVC**
Understand manual PV creation and PVC binding

### After Dynamic Provisioning PV

➡️ **Applications & StatefulSets**
Learn how dynamically provisioned storage is used by Pods and stateful workloads

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Dynamic Storage Management! 🚀
