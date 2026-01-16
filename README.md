# Kubernetes Manifests 📦

A collection of **Kubernetes manifest files** to deploy and manage various workloads and resources on a Kubernetes cluster.

This repository contains a **structured collection of Kubernetes manifest files**, organized by resource type. It is designed for **learning, practice, and real-world Kubernetes deployments**.

Each folder represents a specific Kubernetes object and contains YAML manifests demonstrating how that resource is defined and used.

---

## 📁 Repository Structure

```
kubernetes_manifest_files/
├── ConfigMap/
├── DaemonSet/
├── Deployment/
├── Dynamic Provisioning PV/
├── HPA/
├── Ingress/
├── Namespace/
├── PersistentVolume/
├── Pod/
├── ReplicaSet/
├── ReplicationController/
├── Secret/
├── Service/
├── StatefulSet/
└── Static Provisioning PV/
```

---

## 📦 Folder Description

| Folder Name             | Description                                  |
| ----------------------- | -------------------------------------------- |
| ConfigMap               | Configuration data for pods                  |
| DaemonSet               | Run a pod on each (or selected) node         |
| Deployment              | Manage stateless application deployments     |
| Dynamic Provisioning PV | Storage with dynamic volume provisioning     |
| HPA                     | Horizontal Pod Autoscaler configurations     |
| Ingress                 | External HTTP/HTTPS routing                  |
| Namespace               | Logical cluster isolation                    |
| PersistentVolume        | Cluster-level storage resources              |
| Pod                     | Basic pod definitions                        |
| ReplicaSet              | Maintain a stable set of pod replicas        |
| ReplicationController   | Legacy pod replication controller            |
| Secret                  | Sensitive data (passwords, tokens, keys)     |
| Service                 | Expose applications internally or externally |
| StatefulSet             | Manage stateful applications                 |
| Static Provisioning PV  | Manually created persistent volumes          |

---

## 🚀 Getting Started

### Prerequisites

* Running Kubernetes cluster (Minikube, Docker Desktop, EKS, AKS, GKE)
* `kubectl` installed and configured

Verify cluster access:

```bash
kubectl cluster-info
```

---

## ▶️ Applying Manifests

Apply all Kubernetes manifests at once:

```bash
kubectl apply -f kubernetes_manifest_files/
```

Apply manifests from a specific folder:

```bash
kubectl apply -f kubernetes_manifest_files/Deployment/
```

---

## 🔍 Verification Commands

```bash
kubectl get namespaces
kubectl get pods -A
kubectl get svc -A
kubectl get pv
kubectl get pvc
```

---

## 🎯 Kubernetes Learning Path

Follow this **recommended learning order** to understand Kubernetes concepts step by step:

1. **Pod** – Understand the smallest deployable unit in Kubernetes
2. **Deployment** – Manage stateless applications and rolling updates
3. **Service** – Expose applications inside and outside the cluster
4. **Ingress** – HTTP/HTTPS routing to Services
5. **HPA (Horizontal Pod Autoscaler)** – Automatically scale pods based on metrics
6. **PersistentVolume & PersistentVolumeClaim (PV/PVC)** – Manage application storage

This progression mirrors real-world Kubernetes usage and is ideal for beginners to advanced learners.

---

## 📘 Example YAML Explanations (Per Folder)

### 🟦 Pod

Defines a single running instance of a container.

**Key fields:**

* `apiVersion`, `kind`
* `metadata.name`
* `spec.containers`

Used mainly for learning and debugging.

---

### 🟦 Deployment

Manages replicated Pods and supports rolling updates.

**Key fields:**

* `replicas`
* `selector.matchLabels`
* `template`

Used for stateless applications.

---

### 🟦 Service

Exposes Pods using a stable network endpoint.

**Common types:**

* ClusterIP
* NodePort
* LoadBalancer

Maps traffic to Pods via labels.

---

### 🟦 Ingress

Routes external HTTP/HTTPS traffic to Services.

**Key fields:**

* `rules`
* `host`
* `path`

Requires an Ingress Controller (e.g., NGINX).

---

### 🟦 HPA (Horizontal Pod Autoscaler)

Automatically scales Pods based on CPU or memory usage.

**Key fields:**

* `minReplicas`
* `maxReplicas`
* `metrics`

Works with Deployments and StatefulSets.

---

### 🟦 PersistentVolume (PV) & PersistentVolumeClaim (PVC)

Provides persistent storage for applications.

* **Static Provisioning PV**: Manually created storage
* **Dynamic Provisioning PV**: Automatically created using StorageClass

Ensures data survives Pod restarts.

---

## 🤝 Contributing

Contributions are welcome!

1. Fork the repository
2. Create a new branch
3. Add or improve manifest files
4. Open a Pull Request

---

## 📄 License

No license file is currently present. Consider adding **MIT** or **Apache 2.0** for open-source usage clarity.

---

## 📬 Contact

For questions, issues, or suggestions, please open a GitHub issue.

Happy Kubernetes Learning & Deployment! 🚀
