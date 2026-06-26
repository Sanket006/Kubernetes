# ☸️ Kubernetes Horizontal Pod Autoscaler (HPA) Manifests

This folder contains **Kubernetes HPA manifest examples**. The **Horizontal Pod Autoscaler (HPA)** automatically scales the number of Pod replicas in a Deployment, ReplicaSet, DaemonSet, or StatefulSet based on observed CPU utilization, memory usage, or custom metrics.

Autoscaling helps maintain application performance and availability during unexpected traffic spikes, while scaling down during idle periods to minimize cluster resource costs.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── HPA/
    ├── hpa-cpu-based.yaml             # Autoscale based on CPU utilization
    ├── hpa-memory-based.yaml          # Autoscale based on Memory usage
    ├── hpa-cpu+memory.yaml            # Autoscale targeting both CPU and Memory
    ├── deployment+hpa(cpu+mem).yaml   # Combined Deployment with resource specs and HPA
    └── README.md                      # This documentation file
```

---

## 🧩 Core Fields Used in HPA YAML

* **apiVersion**: `autoscaling/v2`
* **kind**: `HorizontalPodAutoscaler`
* **metadata**: Name, namespace, and labels.
* **spec**: Specifies the autoscaling behavior:
  * **scaleTargetRef**: Points to the target workload to scale (e.g. `Deployment/nginx-deployment`).
  * **minReplicas**: Minimum number of replicas (e.g. `1`).
  * **maxReplicas**: Maximum scale limit (e.g. `10`).
  * **metrics**: Array of metrics targets to trigger scaling:
    - **type**: Target source (`Resource`, `Pods`, `Object`, `External`).
    - **resource**:
      - **name**: Metric name (`cpu` or `memory`).
      - **target**: Type of utilization (`Utilization` or `Value`) and average percentage/values.

---

## 📘 Practical Examples

### 1. Combined CPU & Memory HPA (`hpa-cpu+memory.yaml`)
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deployment
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 60             # Scales up if CPU average exceeds 60%
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 75             # Scales up if Memory average exceeds 75%
```

---

## ▶️ kubectl Operations

> ⚠️ **Prerequisite:** HPA requires a **Metrics Server** to be deployed in your cluster. If using Minikube, enable it with `minikube addons enable metrics-server`.

### Apply the Manifests
```bash
kubectl apply -f kubernetes_manifest_files/HPA/
```

### Verify & Monitor
```bash
# List all HPAs and view target/current resource usage
kubectl get hpa

# Watch autoscaling events and metrics changes in real-time
kubectl get hpa -w

# View detailed metrics calculations and scaling events history
kubectl describe hpa nginx-hpa
```

### Delete HPA
```bash
kubectl delete -f kubernetes_manifest_files/HPA/
```

---

## 🛡️ Best Practices & Common Mistakes

* **Define Container Resources:** HPA **cannot** scale a Deployment if the containers in the Pod template lack `resources.requests` specifications. The Metrics Server computes percentage utilization based on the requested resource boundaries.
* **Cooldown Delays (Stabilization Window):** Kubernetes has built-in scale-up and scale-down stabilization delays (defaults: 0s for scale-up, 5 mins for scale-down) to prevent scaling oscillation ("flapping") when load fluctuates quickly.

---

## 🛣️ Learning Path Navigation

    ⏮️ **[Secret](../Secret/README.md)** | ⏭️ **[PersistentVolume](../PersistentVolume/README.md)**
