# Kubernetes StatefulSet Manifests

This folder contains **Kubernetes StatefulSet manifest files**. A **StatefulSet** is a controller used to manage **stateful applications** that require **stable, unique network identifiers and persistent storage**.

StatefulSets are ideal for databases, messaging systems, and other applications where each Pod needs a consistent identity and storage.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── StatefulSet/
    ├── mysql-statefulset-persistentvolume.yaml   
    ├── mysql-statefulset.yaml
    ├── nginx-statefulset-service.yaml
    └── README.md
```

This folder includes YAML files defining StatefulSet resources.

---

## 📘 What is a Kubernetes StatefulSet?

A **StatefulSet** manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods:

* Each Pod has a **stable, unique hostname**
* Pods are **created and deleted in order**
* Supports **stable persistent storage** via PersistentVolumeClaims
* Manages scaling, rolling updates, and Pod identity

In simple terms:

> **StatefulSet ensures that stateful applications run reliably with stable identities and storage.**

---

## 🎓 What You Learn from StatefulSets

By working with StatefulSet manifests, you will understand:

* How to deploy stateful applications in Kubernetes
* Pod identity and stable network hostnames
* Persistent storage integration (PVCs)
* Ordered deployment, scaling, and termination
* Rolling updates for stateful workloads

---

## 🧩 Core Fields Used in StatefulSet YAML

A typical StatefulSet manifest includes:

* **apiVersion**: `apps/v1`
* **kind**: `StatefulSet`
* **metadata**:

  * `name`: StatefulSet name
  * `labels`: Metadata labels
* **spec**:

  * `serviceName`: Governing Service name for network identity
  * `replicas`: Desired number of Pods
  * `selector.matchLabels`: Labels used to manage Pods
  * `template`: Pod template

    * `metadata.labels`: Must match selector
    * `spec.containers`: Container definitions

      * `image`: Container image
      * `ports`: Container ports
      * `resources`: CPU/memory requests & limits
  * `volumeClaimTemplates`: PersistentVolumeClaims for stable storage
  * `updateStrategy`: RollingUpdate or OnDelete

**Example YAML:**

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web-statefulset
spec:
  serviceName: "web"
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
  updateStrategy:
    type: RollingUpdate
```

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply StatefulSet manifests:

```bash
kubectl apply -f kubernetes_manifest_files/StatefulSet/
```

Verify StatefulSets:

```bash
kubectl get statefulsets
```

Check Pods created by StatefulSet:

```bash
kubectl get pods
```

Describe a StatefulSet:

```bash
kubectl describe statefulset <statefulset-name>
```

Scale a StatefulSet:

```bash
kubectl scale statefulset <statefulset-name> --replicas=5
```

---

## 🔍 StatefulSet Working Flow

1. StatefulSet is created with desired replicas and Pod template
2. Pods are created **in order** with unique identities
3. PersistentVolumeClaims are provisioned for each Pod
4. Pods are terminated **in reverse order** during scaling down
5. Rolling updates are applied carefully to maintain state

```
StatefulSet → Pods (stable identity) → Persistent Volumes
```

---

## 🧪 Common Use Cases

* Databases (MySQL, PostgreSQL, MongoDB)
* Messaging systems (Kafka, RabbitMQ)
* Stateful microservices
* Applications requiring stable identity and persistent storage

---

## ⚠️ Common Mistakes

* Using StatefulSet for stateless applications
* Not creating a governing Service
* Misconfiguring volumeClaimTemplates
* Not understanding Pod creation and termination order

---

## ✅ Best Practices

* Use StatefulSet only for stateful workloads
* Always define a governing Service
* Use `volumeClaimTemplates` for persistent storage
* Monitor Pod readiness and health
* Choose the correct `updateStrategy` (RollingUpdate for updates, OnDelete for manual control)

---

## 🛣️ Learning Path Linkage

### Before StatefulSet

➡️ **Pod → Deployment → Service → Ingress → HPA**
Understand deployment, scaling, and traffic management

### After StatefulSet

➡️ **PersistentVolume / PVC → Static/Dynamic Provisioning PV**
Learn about persistent storage management for stateful workloads

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Stateful Application Management! 🚀
