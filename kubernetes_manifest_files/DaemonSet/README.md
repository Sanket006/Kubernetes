# Kubernetes DaemonSet Manifests

This folder contains **Kubernetes DaemonSet manifest files**. A **DaemonSet** ensures that a copy of a Pod runs on **all or selected nodes** in a cluster.

DaemonSets are used for cluster-level services like log collection, monitoring agents, or networking components.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── DaemonSet/
    ├── daemonset.yaml 
    ├── nginx-daemonset-service.yaml  
    └── README.md
```

This folder includes YAML files defining DaemonSet resources.

---

## 📘 What is a Kubernetes DaemonSet?

A **DaemonSet** is a controller that:

* Ensures a Pod runs on every node (or selected nodes) in the cluster
* Automatically adds Pods to new nodes when they join
* Deletes Pods from nodes when they are removed from the DaemonSet

In simple terms:

> **DaemonSet runs one or more Pods on every node for cluster-level tasks.**

---

## 🎓 What You Learn from DaemonSets

By working with DaemonSet manifests, you will understand:

* Running cluster-wide services automatically
* Scheduling Pods on specific nodes using node selectors or tolerations
* Integration with logging, monitoring, and networking tools

---

## 🧩 Core Fields Used in DaemonSet YAML

A typical DaemonSet manifest includes:

* **apiVersion**: `apps/v1`
* **kind**: `DaemonSet`
* **metadata**:

  * `name`: DaemonSet name
  * `labels`: Metadata labels
* **spec**:

  * `selector.matchLabels`: Labels used to manage Pods
  * `template`: Pod template

    * `metadata.labels`: Must match selector
    * `spec.containers`: Container definitions

      * `image`: Container image
      * `ports`: Container ports
      * `resources`: CPU/memory requests & limits
  * **updateStrategy**: RollingUpdate or OnDelete

**Example YAML:**

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-monitor
spec:
  selector:
    matchLabels:
      app: node-monitor
  template:
    metadata:
      labels:
        app: node-monitor
    spec:
      containers:
      - name: node-monitor
        image: myorg/node-monitor:1.0
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 200m
            memory: 256Mi
  updateStrategy:
    type: RollingUpdate
```

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply DaemonSet manifests:

```bash
kubectl apply -f kubernetes_manifest_files/DaemonSet/
```

Verify DaemonSets:

```bash
kubectl get daemonsets
```

Check Pods created by DaemonSet:

```bash
kubectl get pods -o wide
```

Describe a DaemonSet:

```bash
kubectl describe daemonset <daemonset-name>
```

Delete a DaemonSet:

```bash
kubectl delete daemonset <daemonset-name>
```

---

## 🔍 DaemonSet Working Flow

1. DaemonSet is created with Pod template
2. Kubernetes ensures Pods are deployed on all nodes (or matching nodes)
3. Pods are automatically added to new nodes
4. Pods are deleted when nodes are removed or DaemonSet is deleted

```
DaemonSet → Pods (one per node) → Cluster-wide Service
```

---

## 🧪 Common Use Cases

* Log collection agents (e.g., Fluentd, Logstash)
* Monitoring agents (e.g., Prometheus node exporter)
* Networking components (e.g., Calico, Weave Net)
* Security scanning agents

---

## ⚠️ Common Mistakes

* Not using correct labels for selector and template
* Running DaemonSet Pods without resource limits
* Forgetting tolerations for tainted nodes
* Not testing update strategy in production

---

## ✅ Best Practices

* Use node selectors or affinities for specific node workloads
* Set resource requests and limits
* Use RollingUpdate strategy for zero-downtime updates
* Include tolerations for nodes with taints
* Monitor DaemonSet Pods for cluster-wide performance

---

## 🛣️ Learning Path Linkage

### Before DaemonSet

➡️ **Pod → Deployment → ReplicaSet / StatefulSet**
Understand Pod lifecycle and scaling

### After DaemonSet

➡️ **Service → Ingress → HPA**
Learn traffic routing, networking, and autoscaling

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Cluster-Wide Management with DaemonSets! 🚀
