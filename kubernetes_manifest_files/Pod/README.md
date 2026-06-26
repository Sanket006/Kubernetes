# ☸️ Kubernetes Pod Manifests

This folder contains **Kubernetes Pod manifest examples**. A **Pod** is the smallest and simplest deployable unit in Kubernetes, representing one or more containers running together with shared storage, networking, and a specification for how to run the containers.

Pods are primarily used for **learning, testing, and debugging**. In production environments, Pods are rarely deployed directly and are instead managed by higher-level controllers like Deployments or StatefulSets.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Pod/
    ├── pod.yaml                 # Bare minimum single-container Pod
    ├── nginx-pod-service.yaml   # Pod combined with a NodePort Service configuration
    └── README.md                # This documentation file
```

---

## 🧩 Core Fields Used in Pod YAML

* **apiVersion**: `v1`
* **kind**: `Pod`
* **metadata**: Name, namespace, and labels to identify the Pod.
* **spec**: Desired state of the Pod, containing:
  * **containers**: List of containers to run inside the Pod.
  * **image**: The container image repository and tag.
  * **ports**: List of ports exposed by the container.
  * **resources**: CPU and memory limits and requests (best practice).

---

## 📘 Practical Examples

### 1. Minimal Pod Manifest (`pod.yaml`)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: nginx-app
spec:
  containers:
    - name: nginx-container
      image: nginx:1.25.3-alpine               # Pinned version (Best Practice)
      ports:
        - containerPort: 80
      resources:                               # Added requests/limits (Best Practice)
        requests:
          memory: "64Mi"
          cpu: "100m"
        limits:
          memory: "128Mi"
          cpu: "200m"
```

---

## ▶️ kubectl Operations

### Apply the Manifest
```bash
kubectl apply -f kubernetes_manifest_files/Pod/pod.yaml
```

### Verify & Describe
```bash
# View list of running pods
kubectl get pods

# View detailed pod specifications and lifecycle events
kubectl describe pod my-pod
```

### Debugging & Logs
```bash
# View container standard output logs
kubectl logs my-pod

# Run an interactive shell inside the container
kubectl exec -it my-pod -- /bin/sh
```

### Delete the Pod
```bash
kubectl delete -f kubernetes_manifest_files/Pod/pod.yaml
```

---

## 🛡️ Best Practices & Common Mistakes

### Best Practices
* **Set Resource Limits:** Always specify container memory/CPU requests and limits to ensure fair scheduling and stability on nodes.
* **Pin Image Tags:** Use exact image versions (e.g. `nginx:1.25.3-alpine`) instead of mutable tags like `:latest` or tagless entries.
* **Use Controllers:** For production workloads, use **Deployments** or **ReplicaSets** to automatically recreate failed Pods.

### Common Mistakes
* **Expecting Self-Healing:** Directly deployed Pods do not automatically recreate themselves if the node fails or restarts.
* **Port Conflicts:** Running multiple containers inside the same Pod that bind to the same port will result in container start failures.

---

## 🛣️ Learning Path Navigation

⏮️ **[Root Overview](../../README.md)** | ⏭️ **[ReplicationController](../ReplicationController/README.md)**
