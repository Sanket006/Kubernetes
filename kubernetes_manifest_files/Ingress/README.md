# ☸️ Kubernetes Ingress Manifests

This folder contains **Kubernetes Ingress manifest examples**. An **Ingress** is an API object that manages external access to the services in a cluster, typically HTTP and HTTPS. 

Ingress provides Layer 7 routing rules, allowing you to expose multiple microservices under a single IP address or domain name. It supports path-based routing (e.g. `/app1` vs `/app2`), host-based routing (e.g. `app1.example.com` vs `app2.example.com`), and TLS/SSL termination.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Ingress/
    ├── ingress-host-based-routing.yaml   # Host-based routing template
    ├── ingress-path-based routing.yaml   # Path-based routing template (rewrite-target)
    ├── ingress-setup.md                  # Detailed controller setup guide
    └── README.md                         # This documentation file
```

---

## 🧩 Core Fields Used in Ingress YAML

* **apiVersion**: `networking.k8s.io/v1`
* **kind**: `Ingress`
* **metadata**: Name, namespace, and critical annotations (e.g., `nginx.ingress.kubernetes.io/rewrite-target`).
* **spec**: Specifies the Ingress routing rules:
  * **ingressClassName**: Identifies the Ingress Controller (e.g., `nginx`).
  * **rules**: List of routing hosts and HTTP paths:
    - **host**: The domain name (e.g., `example.com`).
    - **http.paths**:
      - **path**: The URL path prefix or exact match.
      - **pathType**: Match algorithm (usually `Prefix` or `Exact`).
      - **backend**: The target Service (`service.name`) and destination port (`service.port.number`).
  * **tls**: Configures secure HTTPS connections by referencing a Kubernetes Secret.

---

## 📘 Practical Examples

### 1. Path-Based Ingress Manifest (`ingress-path-based routing.yaml`)
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: path-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /    # NGINX specific rewrite rule
spec:
  ingressClassName: nginx                             # Tells NGINX controller to process this
  rules:
    - host: example.com
      http:
        paths:
          - path: /app1
            pathType: Prefix
            backend:
              service:
                name: app1-service
                port:
                  number: 80
          - path: /app2
            pathType: Prefix
            backend:
              service:
                name: app2-service
                port:
                  number: 80
```

---

## ▶️ kubectl Operations

> ⚠️ **Prerequisite:** Ingress resources require an **Ingress Controller** (such as NGINX Ingress Controller) to be installed in the cluster. See [ingress-setup.md](./ingress-setup.md) for installation steps.

### Apply the Manifests
```bash
kubectl apply -f kubernetes_manifest_files/Ingress/
```

### Verify & Describe
```bash
# List Ingress resources and their assigned IP address
kubectl get ingress

# View detailed routing tables and annotation configurations
kubectl describe ingress path-ingress
```

### Delete Ingress
```bash
kubectl delete -f kubernetes_manifest_files/Ingress/
```

---

## 🛣️ Learning Path Navigation

    ⏮️ **[Service](../Service/README.md)** | ⏭️ **[API Gateway](../API%20Gateway/README.md)**
