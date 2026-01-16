# Kubernetes Deployment Manifests

This folder contains **Kubernetes Deployment manifest files**. A **Deployment** manages the lifecycle of Pods and ReplicaSets, providing declarative updates, self-healing, and scaling capabilities.

Deployments are the **recommended way to manage stateless applications** in Kubernetes because they handle all ReplicaSet creation and management automatically.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Deployment/
    ├── deployment.yaml 
    ├── nginx-deployment-service.yaml 
    └── README.md
```

This folder includes YAML files defining Deployment resources.

---

## 📘 What is a Kubernetes Deployment?

A **Deployment** is a controller that:

* Defines the desired state for Pods and ReplicaSets
* Automatically creates and updates ReplicaSets
* Supports rolling updates and rollbacks
* Provides declarative management for stateless applications

In simple terms:

> **Deployment ensures your application runs the desired number of Pods and can be updated safely.**

---

## 🎓 What You Learn from Deployments

By working with Deployment manifests, you will understand:

* How Kubernetes manages Pods at scale
* Rolling updates and rollback mechanisms
* Declarative updates with minimal downtime
* Integration with ReplicaSets for automated scaling
* How Deployments interact with Services and Ingress

---

## 🧩 Core Fields Used in Deployment YAML

A typical Deployment manifest includes:

* **apiVersion**: `apps/v1`
* **kind**: `Deployment`
* **metadata**:

  * `name`: Deployment name
  * `labels`: Metadata labels
* **spec**:

  * `replicas`: Desired number of Pods
  * `selector.matchLabels`: Labels used to manage Pods
  * `template`: Pod template

    * `metadata.labels`: Must match selector
    * `spec.containers`: Container definitions

      * `image`: Container image
      * `ports`: Container ports
      * `resources`: CPU/memory requests & limits
  * **strategy**: Deployment update strategy

    * `type`: Either `RollingUpdate`, `Recreate`, or `BlueGreen` (custom strategies can be implemented via Operators)
    * `rollingUpdate` (optional): Parameters for RollingUpdate

      * `maxSurge`: Maximum number of Pods created above desired count during update
      * `maxUnavailable`: Maximum number of Pods unavailable during update

---

## 🔑 Deployment Strategies

### 1. RollingUpdate (Default)

* Updates Pods **gradually** with minimal downtime
* Creates new Pods while terminating old ones
* Controlled via `maxSurge` and `maxUnavailable`

**Example:**

* `maxSurge: 1`
* `maxUnavailable: 1`

### 2. Recreate

* Terminates all existing Pods **before** creating new ones
* Causes downtime but simpler for certain workloads

**Use cases:**

* Stateful applications where only one Pod can run at a time
* Workloads that cannot handle multiple versions running simultaneously

### 3. Blue/Green Deployment

* Deploys a **new version alongside the old version**
* Switches traffic to the new version after verification
* Requires manual or external traffic routing control (via Service or Ingress)

**Use cases:**

* Production systems requiring full version isolation
* Safe rollback scenarios

### 4. Canary Deployment

* Gradually exposes the new version to a **small subset of users**
* Monitors metrics before full rollout
* Typically implemented using Service, Ingress, or custom controllers

**Use cases:**

* Reducing risk of new releases
* Testing new features in production with limited impact

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply Deployment manifests:

```bash
kubectl apply -f kubernetes_manifest_files/Deployment/
```

Verify Deployments:

```bash
kubectl get deployments
```

Check Pods created by Deployment:

```bash
kubectl get pods
```

Describe a Deployment:

```bash
kubectl describe deployment <deployment-name>
```

Scale a Deployment:

```bash
kubectl scale deployment <deployment-name> --replicas=5
```

Rollout status:

```bash
kubectl rollout status deployment/<deployment-name>
```

Rollback to previous version:

```bash
kubectl rollout undo deployment/<deployment-name>
```

---

## 🔍 Deployment Working Flow

1. Deployment is created with desired replicas and Pod template
2. Kubernetes creates a ReplicaSet according to the template
3. ReplicaSet ensures the desired number of Pods are running
4. Deployment handles rolling updates, scaling, and rollbacks

```
Deployment → ReplicaSet → Pods
```

---

## 🧪 Common Use Cases

* Managing stateless applications
* Performing zero-downtime updates
* Autoscaling Pods with HPA
* Continuous Deployment in CI/CD pipelines

---

## ⚠️ Common Mistakes

* Selector labels not matching Pod template labels
* Editing Pods directly instead of updating the Deployment
* Not specifying resource requests and limits
* Using Recreate strategy when RollingUpdate is required

---

## ✅ Best Practices

* Always use **Deployments** for production applications
* Use **RollingUpdate** strategy for zero-downtime updates
* Keep labels consistent and unique
* Define resource requests and limits for each container
* Integrate with Services for networking
* Monitor rollout status and set readiness probes
* Consider Canary or Blue/Green strategies for high-risk production updates

---

## 🛣️ Learning Path Linkage

### Before Deployment

➡️ **Pod → ReplicaSet → ReplicationController**
Understand Pod lifecycle and scaling controllers

### After Deployment

➡️ **Service → Ingress → HPA → PersistentVolume / PVC**
Learn networking, traffic routing, autoscaling, and storage

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Deployment Management! 🚀










# Kubernetes Deployment Manifests

This folder contains **Kubernetes Deployment manifest files**. A **Deployment** manages the lifecycle of Pods and ReplicaSets, providing declarative updates, self-healing, and scaling capabilities.

Deployments are the **recommended way to manage stateless applications** in Kubernetes because they handle all ReplicaSet creation and management automatically.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Deployment/
    ├── *.yaml   # Kubernetes Deployment manifest files
```

This folder includes YAML files defining Deployment resources.

---

## 📘 What is a Kubernetes Deployment?

A **Deployment** is a controller that:

* Defines the desired state for Pods and ReplicaSets
* Automatically creates and updates ReplicaSets
* Supports rolling updates and rollbacks
* Provides declarative management for stateless applications

In simple terms:

> **Deployment ensures your application runs the desired number of Pods and can be updated safely.**

---

## 🎓 What You Learn from Deployments

By working with Deployment manifests, you will understand:

* How Kubernetes manages Pods at scale
* Rolling updates and rollback mechanisms
* Declarative updates with minimal downtime
* Integration with ReplicaSets for automated scaling
* How Deployments interact with Services and Ingress

---

## 🧩 Core Fields Used in Deployment YAML

A typical Deployment manifest includes:

* **apiVersion**: `apps/v1`
* **kind**: `Deployment`
* **metadata**:

  * `name`: Deployment name
  * `labels`: Metadata labels
* **spec**:

  * `replicas`: Desired number of Pods
  * `selector.matchLabels`: Labels used to manage Pods
  * `template`: Pod template

    * `metadata.labels`: Must match selector
    * `spec.containers`: Container definitions

      * `image`: Container image
      * `ports`: Container ports
      * `resources`: CPU/memory requests & limits
  * **strategy**: Deployment update strategy

    * `type`: Either `RollingUpdate` or `Recreate`
    * `rollingUpdate` (optional): Parameters for RollingUpdate

      * `maxSurge`: Maximum number of Pods created above desired count during update
      * `maxUnavailable`: Maximum number of Pods unavailable during update

---

## 🔑 Deployment Strategies

### 1. RollingUpdate (Default)

* Updates Pods **gradually** with minimal downtime
* Creates new Pods while terminating old ones
* Controlled via `maxSurge` and `maxUnavailable`

**Example:**

* `maxSurge: 1`
* `maxUnavailable: 1`

### 2. Recreate

* Terminates all existing Pods **before** creating new ones
* Causes downtime but simpler for certain workloads

**Use cases:**

* Stateful applications where only one Pod can run at a time
* Workloads that cannot handle multiple versions running simultaneously

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply Deployment manifests:

```bash
kubectl apply -f kubernetes_manifest_files/Deployment/
```

Verify Deployments:

```bash
kubectl get deployments
```

Check Pods created by Deployment:

```bash
kubectl get pods
```

Describe a Deployment:

```bash
kubectl describe deployment <deployment-name>
```

Scale a Deployment:

```bash
kubectl scale deployment <deployment-name> --replicas=5
```

Rollout status:

```bash
kubectl rollout status deployment/<deployment-name>
```

Rollback to previous version:

```bash
kubectl rollout undo deployment/<deployment-name>
```

---

## 🔍 Deployment Working Flow

1. Deployment is created with desired replicas and Pod template
2. Kubernetes creates a ReplicaSet according to the template
3. ReplicaSet ensures the desired number of Pods are running
4. Deployment handles rolling updates, scaling, and rollbacks

```
Deployment → ReplicaSet → Pods
```

---

## 🧪 Common Use Cases

* Managing stateless applications
* Performing zero-downtime updates
* Autoscaling Pods with HPA
* Continuous Deployment in CI/CD pipelines

---

## ⚠️ Common Mistakes

* Selector labels not matching Pod template labels
* Editing Pods directly instead of updating the Deployment
* Not specifying resource requests and limits
* Using Recreate strategy when RollingUpdate is required

---

## ✅ Best Practices

* Always use **Deployments** for production applications
* Use **RollingUpdate** strategy for zero-downtime updates
* Keep labels consistent and unique
* Define resource requests and limits for each container
* Integrate with Services for networking
* Monitor rollout status and set readiness probes

---

## 🛣️ Learning Path Linkage

### Before Deployment

➡️ **Pod → ReplicaSet → ReplicationController**
Understand Pod lifecycle and scaling controllers

### After Deployment

➡️ **Service → Ingress → HPA → PersistentVolume / PVC**
Learn networking, traffic routing, autoscaling, and storage

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Deployment Management! 🚀










# Kubernetes Deployment Manifests

This folder contains **Kubernetes Deployment manifest files**. A **Deployment** manages the lifecycle of Pods and ReplicaSets, providing declarative updates, self-healing, and scaling capabilities.

Deployments are the **recommended way to manage stateless applications** in Kubernetes because they handle all ReplicaSet creation and management automatically.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Deployment/
    ├── *.yaml   # Kubernetes Deployment manifest files
```

This folder includes YAML files defining Deployment resources.

---

## 📘 What is a Kubernetes Deployment?

A **Deployment** is a controller that:

* Defines the desired state for Pods and ReplicaSets
* Automatically creates and updates ReplicaSets
* Supports rolling updates and rollbacks
* Provides declarative management for stateless applications

In simple terms:

> **Deployment ensures your application runs the desired number of Pods and can be updated safely.**

---

## 🎓 What You Learn from Deployments

By working with Deployment manifests, you will understand:

* How Kubernetes manages Pods at scale
* Rolling updates and rollback mechanisms
* Declarative updates with minimal downtime
* Integration with ReplicaSets for automated scaling
* How Deployments interact with Services and Ingress

---

## 🧩 Core Fields Used in Deployment YAML

A typical Deployment manifest includes:

* **apiVersion**: `apps/v1`
* **kind**: `Deployment`
* **metadata**:

  * `name`: Deployment name
  * `labels`: Metadata labels
* **spec**:

  * `replicas`: Desired number of Pods
  * `selector.matchLabels`: Labels used to manage Pods
  * `template`: Pod template

    * `metadata.labels`: Must match selector
    * `spec.containers`: Container definitions

      * `image`: Container image
      * `ports`: Container ports
      * `resources`: CPU/memory requests & limits
  * `strategy`: Update strategy (`RollingUpdate` or `Recreate`)

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply Deployment manifests:

```bash
kubectl apply -f kubernetes_manifest_files/Deployment/
```

Verify Deployments:

```bash
kubectl get deployments
```

Check Pods created by Deployment:

```bash
kubectl get pods
```

Describe a Deployment:

```bash
kubectl describe deployment <deployment-name>
```

Scale a Deployment:

```bash
kubectl scale deployment <deployment-name> --replicas=5
```

Rollout status:

```bash
kubectl rollout status deployment/<deployment-name>
```

Rollback to previous version:

```bash
kubectl rollout undo deployment/<deployment-name>
```

---

## 🔍 Deployment Working Flow

1. Deployment is created with desired replicas and Pod template
2. Kubernetes creates a ReplicaSet according to the template
3. ReplicaSet ensures the desired number of Pods are running
4. Deployment handles rolling updates, scaling, and rollbacks

```
Deployment → ReplicaSet → Pods
```

---

## 🧪 Common Use Cases

* Managing stateless applications
* Performing zero-downtime updates
* Autoscaling Pods with HPA
* Continuous Deployment in CI/CD pipelines

---

## ⚠️ Common Mistakes

* Selector labels not matching Pod template labels
* Editing Pods directly instead of updating the Deployment
* Not specifying resource requests and limits
* Using Recreate strategy when RollingUpdate is required

---

## ✅ Best Practices

* Always use **Deployments** for production applications
* Use **RollingUpdate** strategy for zero-downtime updates
* Keep labels consistent and unique
* Define resource requests and limits for each container
* Integrate with Services for networking
* Monitor rollout status and set readiness probes

---

## 🛣️ Learning Path Linkage

### Before Deployment

➡️ **Pod → ReplicaSet → ReplicationController**
Understand Pod lifecycle and scaling controllers

### After Deployment

➡️ **Service → Ingress → HPA → PersistentVolume / PVC**
Learn networking, traffic routing, autoscaling, and storage

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Dep
