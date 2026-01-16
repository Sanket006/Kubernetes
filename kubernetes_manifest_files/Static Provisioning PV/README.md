# Kubernetes Static Provisioning PV Manifests

This folder contains **Kubernetes PersistentVolume (PV) manifests for Static Provisioning**.

**Static provisioning** means that cluster administrators manually create PersistentVolumes ahead of time. Users then create PersistentVolumeClaims (PVCs) that match these pre-created PVs.

Static provisioning is useful when you have **pre-existing storage resources** or need **fine-grained control** over volume allocation.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Static Provisioning PV/
    ├── AWS EBS + PV + PVC ManifestFile.yaml    
    └── README.md
```

This folder includes YAML files defining PV and PVC resources for static provisioning.

---

## 📘 What is Static Provisioning in Kubernetes?

**Static Provisioning** requires administrators to manually create PVs that define storage capacity, access modes, and reclaim policies.

Key points:

* PVs are pre-created by admin
* PVCs bind to matching PVs automatically
* Useful for on-prem storage or specialized storage setups

In simple terms:

> **Static provisioning is manual storage allocation where PVs are created first and Pods request them via PVCs.**

---

## 🎓 What You Learn from Static Provisioning

By working with static provisioning manifests, you will understand:

* How to pre-create PVs and manage storage manually
* How PVCs claim pre-existing PVs
* Access modes and storage policies
* Reclaim policies (`Retain`, `Recycle`, `Delete`)
* Integration with StatefulSets and Pods

---

## 🧩 Core Fields Used in PV YAML (Static Provisioning)

* **apiVersion**: `v1`
* **kind**: `PersistentVolume`
* **metadata.name**: PV name
* **spec.capacity.storage**: Size of the volume (e.g., 5Gi)
* **spec.accessModes**: `ReadWriteOnce`, `ReadOnlyMany`, `ReadWriteMany`
* **spec.persistentVolumeReclaimPolicy**: `Retain`, `Recycle`, `Delete`
* **spec.storageClassName**: Optional, must match PVC if used
* **spec.hostPath** or other volume type (e.g., NFS)

**Example PV YAML:**

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-static-example
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /mnt/data/static
```

---

## 🧩 Core Fields Used in PVC YAML

* **apiVersion**: `v1`
* **kind**: `PersistentVolumeClaim`
* **metadata.name**: PVC name
* **spec.accessModes**: Must match PV access mode
* **spec.resources.requests.storage**: Requested storage size
* **spec.storageClassName**: Optional, should match PV if defined

**Example PVC YAML:**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-static-example
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply PV manifests:

```bash
kubectl apply -f kubernetes_manifest_files/Static\ Provisioning\ PV/pv-*.yaml
```

Apply PVC manifests:

```bash
kubectl apply -f kubernetes_manifest_files/Static\ Provisioning\ PV/pvc-*.yaml
```

Verify PVs and PVCs:

```bash
kubectl get pv
kubectl get pvc
```

Describe PV or PVC:

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

## 🔍 Static Provisioning Working Flow

1. Administrator creates PV with desired capacity and access modes
2. User creates PVC requesting storage
3. Kubernetes binds PVC to a matching PV
4. Pod consumes storage via PVC
5. PV retains data based on `persistentVolumeReclaimPolicy`

```
PV (pre-created) → PVC → Pod → Persistent Storage
```

---

## 🧪 Common Use Cases

* On-prem storage with pre-allocated volumes
* Applications requiring exact storage configuration
* Backup and archival systems

---

## ⚠️ Common Mistakes

* PVC access modes not matching PV
* Storage size mismatch between PV and PVC
* Misconfigured storage path or type
* Forgetting to set reclaim policy
* Not defining storageClassName correctly if used

---

## ✅ Best Practices

* Pre-create PVs for known workloads
* Ensure PVC access mode matches PV
* Set appropriate `persistentVolumeReclaimPolicy`
* Monitor PV and PVC binding and usage
* Combine PV/PVC with StatefulSets for stateful applications

---

## 🛣️ Learning Path Linkage

### Before Static Provisioning PV

➡️ **PersistentVolume / PVC**
Understand manual PV and PVC creation

### After Static Provisioning PV

➡️ **Applications & StatefulSets**
Learn how statically provisioned storage is consumed by Pods and StatefulSets

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Static Storage Management! 🚀
