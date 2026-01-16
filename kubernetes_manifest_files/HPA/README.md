# Kubernetes Horizontal Pod Autoscaler (HPA) Manifests

This folder contains **Kubernetes HPA manifest files**. The **Horizontal Pod Autoscaler (HPA)** automatically scales the number of Pods in a Deployment, ReplicaSet, or StatefulSet based on observed CPU utilization, memory usage, or custom metrics.

HPA helps maintain **application performance and availability** under varying load conditions.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── HPA/
    ├── deployment+hpa(cpu+mem).yaml 
    ├── hpa-cpu-based.yaml
    ├── hpa-cpu+memory.yaml
    ├── hpa-memory-based.yaml  
    └── README.md
```

This folder includes YAML files defining HPA resources.

---

## 📘 What is a Horizontal Pod Autoscaler?

The **HPA** monitors metrics for Pods and dynamically adjusts the number of replicas to maintain the desired performance.

Key points:

* Scales Pods horizontally (increase/decrease replicas)
* Can use CPU, memory, or custom metrics
* Integrates with Deployments, ReplicaSets, and StatefulSets

In simple terms:

> **HPA automatically adjusts the number of Pods to handle traffic changes and maintain performance.**

---

## 🎓 What You Learn from HPA

By working with HPA manifests, you will understand:

* How automatic scaling works in Kubernetes
* Resource-based scaling using metrics
* Integration with Deployments, ReplicaSets, and StatefulSets
* Configuring target metrics and thresholds

---

## 🧩 Core Fields Used in HPA YAML

A typical HPA manifest includes:

* **apiVersion**: `autoscaling/v2` or `autoscaling/v2beta2`
* **kind**: `HorizontalPodAutoscaler`
* **metadata**:

  * `name`: HPA name
  * `namespace`: Namespace where HPA applies
* **spec**:

  * `scaleTargetRef`: The resource to scale (Deployment, ReplicaSet, StatefulSet)

    * `apiVersion`
    * `kind`
    * `name`
  * `minReplicas`: Minimum number of replicas
  * `maxReplicas`: Maximum number of replicas
  * `metrics`: Metrics to use for scaling

    * `type`: e.g., `Resource` or `Pods`
    * `resource.name`: `cpu` or `memory`
    * `target.averageUtilization`: Desired percentage of resource usage

**Example YAML:**

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: hpa-example
  namespace: default
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deployment
  minReplicas: 2
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply HPA manifests:

```bash
kubectl apply -f kubernetes_manifest_files/HPA/
```

Verify HPAs:

```bash
kubectl get hpa
```

Describe an HPA:

```bash
kubectl describe hpa <hpa-name>
```

Monitor HPA scaling in real time:

```bash
kubectl get hpa -w
```

---

## 🔍 HPA Working Flow

1. HPA is created targeting a Deployment, ReplicaSet, or StatefulSet
2. Metrics server collects CPU/memory or custom metrics
3. HPA compares observed metrics to target thresholds
4. Number of replicas is adjusted automatically within `minReplicas` and `maxReplicas`

```
Metrics Server → HPA → Scale Target → Pods
```

---

## 🧪 Common Use Cases

* Scaling web applications based on traffic
* Autoscaling microservices in a cluster
* Reducing costs by scaling down idle workloads
* Ensuring performance during traffic spikes

---

## ⚠️ Common Mistakes

* Not deploying the metrics-server in the cluster
* Setting unrealistic `minReplicas` or `maxReplicas`
* Using incorrect metric types
* Not monitoring HPA events and logs

---

## ✅ Best Practices

* Deploy metrics-server before using HPA
* Set realistic min and max replicas
* Monitor HPA behavior and logs
* Use custom metrics for advanced scaling scenarios
* Combine HPA with resource requests and limits for accurate scaling

---

## 🛣️ Learning Path Linkage

### Before HPA

➡️ **Pod → Deployment → Service → Ingress → Namespace**
Understand workload deployment, exposure, and resource isolation

### After HPA

➡️ **StatefulSet → PersistentVolume / PVC → Static/Dynamic Provisioning PV**
Learn scaling stateful applications and managing persistent storage

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Autoscaling with HPA! 🚀
