# Kubernetes Namespace Manifests

This folder contains **Kubernetes Namespace manifest files**. A **Namespace** provides a way to **partition resources** within a Kubernetes cluster, allowing multiple teams, projects, or environments to coexist without conflicts.

Namespaces help organize cluster resources, apply access control, and separate workloads logically.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Namespace/
    ├── namespace-pod.yaml   
    ├── namespace.yaml
    └── README.md
```

This folder includes YAML files defining Namespace resources.

---

## 📘 What is a Kubernetes Namespace?

A **Namespace** is a virtual cluster inside a Kubernetes cluster that:

* Provides scope for names (so resource names can be reused in different namespaces)
* Enables **resource isolation** for teams or projects
* Supports resource quota and limit enforcement
* Segregates monitoring, logging, and access policies

In simple terms:

> **Namespaces are like folders for Kubernetes resources.**

---

## 🎓 What You Learn from Namespaces

By working with Namespace manifests, you will understand:

* How to isolate resources logically
* How to manage multiple environments in a single cluster
* Namespace-specific RBAC and policies
* How Namespaces integrate with Deployments, Services, and other resources

---

## 🧩 Core Fields Used in Namespace YAML

A typical Namespace manifest includes:

* **apiVersion**: `v1`
* **kind**: `Namespace`
* **metadata**:

  * `name`: Name of the Namespace
  * `labels` (optional): Labels for organization and selection
  * `annotations` (optional): Metadata for additional information

**Example YAML:**

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev-environment
  labels:
    environment: dev
```

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Create Namespace:

```bash
kubectl apply -f kubernetes_manifest_files/Namespace/
```

List Namespaces:

```bash
kubectl get namespaces
```

Describe a Namespace:

```bash
kubectl describe namespace <namespace-name>
```

Delete a Namespace:

```bash
kubectl delete namespace <namespace-name>
```

Use a Namespace for operations:

```bash
kubectl config set-context --current --namespace=<namespace-name>
```

---

## 🔍 Namespace Working Flow

1. Create a Namespace in the cluster
2. Deploy resources within the Namespace
3. Kubernetes isolates resources logically by namespace
4. Policies, quotas, and RBAC are applied at the Namespace level

```
Cluster → Namespace → Resources (Pods, Services, Deployments, etc.)
```

---

## 🧪 Common Use Cases

* Multiple teams sharing a single cluster
* Separate environments (dev, staging, prod)
* Resource quota enforcement
* RBAC and security boundaries

---

## ⚠️ Common Mistakes

* Deploying resources in the default namespace unintentionally
* Forgetting to specify the namespace in commands
* Misconfiguring resource quotas or RBAC

---

## ✅ Best Practices

* Use descriptive names for Namespaces
* Apply labels to identify purpose or environment
* Set resource quotas and limits per Namespace
* Use Namespaces to logically separate environments
* Integrate Namespaces with RBAC for access control

---

## 🛣️ Learning Path Linkage

### Before Namespace

➡️ **Pod → Deployment → Service**
Understand resource creation and networking

### After Namespace

➡️ **Ingress → HPA → PersistentVolume / PVC**
Learn about traffic routing, scaling, and storage within scoped environments

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Resource Management with Namespaces! 🚀
