# Kubernetes PersistentVolume (PV) and PersistentVolumeClaim (PVC) Manifests

This folder contains **Kubernetes PersistentVolume (PV) and PersistentVolumeClaim (PVC) manifest files**.

A **PersistentVolume (PV)** is a piece of storage in the cluster that has been provisioned by an administrator or dynamically created using a StorageClass.

A **PersistentVolumeClaim (PVC)** is a request for storage by a user that binds to a matching PV to provide persistent storage for Pods.

PV and PVC provide **persistent storage independent of Pod lifecycle**, allowing stateful applications to store data reliably.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── PersistentVolume/
    ├── pv-pvc-deployment.yaml 
    ├── pv-pvc-using-node-affinity.yaml
    ├── pv-pvc-pod.yaml
    ├── pv-pvc.yaml
    ├── pv.yaml
    ├── pvc.yaml
    └── README.md 
```

This folder includes YAML files defining PV and PVC resources.

---

## 📘 What are PersistentVolume and PersistentVolumeClaim?

**PersistentVolume (PV)**:

* Represents a storage resource in the cluster
* Exists independently of Pod lifecycle
* Can be **statically** or **dynamically provisioned**

**PersistentVolumeClaim (PVC)**:

* Requests storage from the cluster
* Specifies access mode and storage size
* Binds to a suitable PV to provide storage to a Pod

In simple terms:

> **PV is the storage, and PVC is the request for storage by Pods.**

---

## 🎓 What You Learn from PV and PVC

By working with PV and PVC manifests, you will understand:

* Static and dynamic provisioning of storage
* How PVC binds to PV
* Access modes (`ReadWriteOnce`, `ReadOnlyMany`, `ReadWriteMany`)
* Storage capacity and reclaim policies
* Integrating PV/PVC with Pods and StatefulSets

---

## 🧩 Core Fields Used in PV YAML

* **apiVersion**: `v1`
* **kind**: `PersistentVolume`
* **metadata.name**: PV name
* **spec.capacity.storage**: Size of the volume (e.g., 1Gi)
* **spec.accessModes**: `ReadWriteOnce`, `ReadOnlyMany`, `ReadWriteMany`
* **spec.persistentVolumeReclaimPolicy**: `Retain`, `Recycle`, `Delete`
* **spec.storageClassName**: StorageClass name (optional)
* **spec.hostPath** or other volume type (e.g., NFS, AWS EBS)

**Example PV YAML:**

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-example
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /mnt/data
```

---

## 🧩 Core Fields Used in PVC YAML

* **apiVersion**: `v1`
* **kind**: `PersistentVolumeClaim`
* **metadata.name**: PVC name
* **spec.accessModes**: Must match PV access mode
* **spec.resources.requests.storage**: Requested storage size
* **spec.storageClassName**: Optional, must match PV storageClassName

**Example PVC YAML:**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-example
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply PV manifests:

```bash
kubectl apply -f kubernetes_manifest_files/PersistentVolume/pv-*.yaml
```

Apply PVC manifests:

```bash
kubectl apply -f kubernetes_manifest_files/PersistentVolume/pvc-*.yaml
```

Verify PVs and PVCs:

```bash
kubectl get pv
kubectl get pvc
```

Describe a PV or PVC:

```bash
kubectl describe pv <pv-name>
kubectl describe pvc <pvc-name>
```

Delete PV or PVC:

```bash
kubectl delete pv <pv-name>
kubectl delete pvc <pvc-name>
```

---

## 🔍 PV & PVC Working Flow

1. PV is created by admin (static) or dynamically via StorageClass
2. PVC is created by a user requesting storage
3. Kubernetes binds PVC to a matching PV
4. Pod consumes storage via PVC
5. PV retains data based on `persistentVolumeReclaimPolicy`

```
PV → PVC → Pod → Persistent Storage
```

---

## 🧪 Common Use Cases

* Databases (MySQL, PostgreSQL, MongoDB)
* Stateful applications requiring persistent storage
* Sharing data across Pods
* Backup and restore of application data

---

## ⚠️ Common Mistakes

* Mismatched PVC and PV access modes
* Insufficient storage capacity
* Misconfigured storage path or type
* Forgetting to set `persistentVolumeReclaimPolicy`
* Not specifying correct StorageClass for dynamic provisioning

---

## ✅ Best Practices

* Use StorageClass for dynamic provisioning whenever possible
* Define access modes based on application requirements
* Set `persistentVolumeReclaimPolicy` thoughtfully
* Monitor PV and PVC usage and status
* Combine PV/PVC with StatefulSets for stable stateful applications

---

## 🛣️ Learning Path Linkage

### Before PV/PVC

➡️ **Pod → Deployment → Service → Ingress → HPA → StatefulSet**

Understand workload deployment, autoscaling, and stateful application management

### After PV/PVC

➡️ **Static Provisioning PV / Dynamic Provisioning PV**

Learn advanced storage provisioning methods and cluster storage management

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Persistent Storage Management with PV & PVC! 🚀
