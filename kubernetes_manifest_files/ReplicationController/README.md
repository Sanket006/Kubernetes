# Kubernetes ReplicationController Manifests

This folder contains **Kubernetes ReplicationController (RC) manifest files**. A **ReplicationController** ensures that a specified number of Pod replicas are running at any given time.

ReplicationController is a **legacy Kubernetes controller** and has largely been replaced by **ReplicaSet** and **Deployment**. It is included here for **learning, comparison, and interview preparation** purposes.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── ReplicationController/
    ├── nginx-rc-service.yaml   
    ├── replicationcontroller.yaml
    └── README.md
```

This folder includes YAML files defining ReplicationController resources.

---

## 📘 What is a Kubernetes ReplicationController?

A **ReplicationController** is a controller that:

* Maintains a desired number of Pod replicas
* Automatically creates Pods if they are deleted or fail
* Terminates extra Pods if there are more than required

In simple terms:

> **ReplicationController keeps the required number of Pods running at all times.**

It was one of the **first controllers introduced in Kubernetes**, but is now considered **deprecated in favor of ReplicaSet**.

---

## 🎓 What You Learn from ReplicationControllers

By working with ReplicationController manifests, you will understand:

* How Kubernetes originally handled Pod replication
* The concept of self-healing workloads
* Label-based Pod selection
* Why ReplicaSet and Deployment replaced ReplicationController

---

## 🧩 Core Fields Used in ReplicationController YAML

A typical ReplicationController manifest includes:

* **apiVersion**: `v1`
* **kind**: `ReplicationController`
* **metadata**:

  * `name`: ReplicationController name
  * `labels`: Resource metadata
* **spec**:

  * `replicas`: Desired number of Pods
  * `selector`: Label selector used to manage Pods
  * `template`: Pod template

    * `metadata.labels`: Must match selector
    * `spec.containers`: Container definitions

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply ReplicationController manifests:

```bash
kubectl apply -f kubernetes_manifest_files/ReplicationController/
```

Verify ReplicationControllers:

```bash
kubectl get rc
```

Check managed Pods:

```bash
kubectl get pods
```

Describe a ReplicationController:

```bash
kubectl describe rc <rc-name>
```

---

## 🔍 ReplicationController Working Flow

1. ReplicationController is created with a replica count
2. Kubernetes compares desired replicas with running Pods
3. Missing Pods are created automatically
4. Extra Pods are deleted if present

```
ReplicationController → Pod Template → Pods
```

---

## 🧪 Common Use Cases

* Understanding legacy Kubernetes workloads
* Comparing ReplicationController vs ReplicaSet
* Studying Kubernetes evolution

> ⚠️ ReplicationControllers are **not recommended** for new production workloads.

---

## ⚠️ Common Mistakes

* Using ReplicationController in new applications
* Overlapping selectors between controllers
* Editing Pods directly instead of the controller

---

## ✅ Best Practices

* Use **Deployment** for modern applications
* Use **ReplicaSet** if direct controller behavior is required
* Keep selectors simple and unique
* Treat ReplicationController as a learning tool

---

## 🆚 ReplicationController vs ReplicaSet

| Feature               | ReplicationController | ReplicaSet |
| --------------------- | --------------------- | ---------- |
| Selector type         | Equality-based only   | Set-based  |
| Current usage         | Legacy                | Modern     |
| Managed by Deployment | ❌                     | ✅          |

---

## 🛣️ Learning Path Linkage

### Before ReplicationController

➡️ **Pod**
Understand basic Pod lifecycle

### After ReplicationController

➡️ **ReplicaSet → Deployment → Service → Ingress → HPA → PersistentVolume / PVC**

---

## 📬 Support & Contributions

For questions or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Learning (Legacy Concepts)! 🚀
