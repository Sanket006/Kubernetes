# ☸️ Kubernetes Service Manifests

This folder contains **Kubernetes Service manifest examples**. A **Service** provides a stable network endpoint (IP address, DNS name, and port) to access one or more Pods, enabling network communication within the cluster and from external clients.

Because Pods in Kubernetes are ephemeral, their IP addresses change constantly. Services solve this problem by offering a consistent, unchanging access point that load-balances traffic across matching Pod instances.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Service/
    ├── service.yaml                 # Skeleton ClusterIP Service manifest
    └── README.md                    # This documentation file
```

---

## 🧩 Core Fields Used in Service YAML

* **apiVersion**: `v1`
* **kind**: `Service`
* **metadata**: Name, namespace, and labels to identify the Service.
* **spec**: Specifies how traffic is directed:
  * **type**: Defines how the Service is exposed:
    - `ClusterIP` (default): Internal cluster access only.
    - `NodePort`: Exposes the service on each Node's IP at a static port (30000-32767).
    - `LoadBalancer`: Creates an external cloud provider load balancer.
    - `ExternalName`: Maps the service to an external DNS name.
  * **selector**: Key-value label selectors to target Pods (e.g. `app: nginx-app`).
  * **ports**: List of port mappings:
    - **port**: The port exposed by the Service.
    - **targetPort**: The port exposed on the target container.
    - **protocol**: Network protocol (`TCP` or `UDP`).

---

## 📘 Practical Examples

### 1. Minimal ClusterIP Service Manifest (`service.yaml`)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-svc
spec:
  type: ClusterIP                           # Internal communication (Default)
  selector:
    app: nginx-app                          # Routes traffic to pods with this label
  ports:
    - protocol: TCP
      port: 80                              # Port exposed by the Service
      targetPort: 80                        # Target port on the container
```

---

## ▶️ kubectl Operations

### Apply the Manifests
```bash
kubectl apply -f kubernetes_manifest_files/Service/
```

### Verify & Describe
```bash
# List all Services
kubectl get svc

# Show detailed Service configurations and active endpoints
kubectl describe svc my-svc

# Get active backends/endpoints mapped to Services
kubectl get endpoints
```

### Delete Service
```bash
kubectl delete -f kubernetes_manifest_files/Service/
```

---

## 🆚 Service Types at a Glance

| Service Type | Exposes To | Port Details | Typical Use Case |
| :--- | :--- | :--- | :--- |
| **ClusterIP** | Inside Cluster | Virtual Internal IP | Database or Internal Backend API communication. |
| **NodePort** | Outside Cluster | High Ports (`30000-32767`) | Direct developer testing. |
| **LoadBalancer**| Outside Cluster | External Cloud Load Balancer | Production public services. |
| **ExternalName**| Inside Cluster | DNS CNAME Redirect | Connecting to external databases (e.g., AWS RDS). |

---

## 🛣️ Learning Path Navigation

    ⏮️ **[StatefulSet](../StatefulSet/README.md)** | ⏭️ **[Ingress](../Ingress/README.md)**
