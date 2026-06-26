# ☸️ Kubernetes API Gateway (Gateway API) Manifests

This folder contains **Kubernetes Gateway API manifest examples**. 

The **Gateway API** is an open-source project managed by the Kubernetes Network Special Interest Group (SIG-Network). It is a collection of resources (`GatewayClass`, `Gateway`, `HTTPRoute`, `ReferenceGrant`, etc.) that model service networking in Kubernetes. It serves as the modern, expressive, and role-oriented successor to the legacy **Ingress** resource, providing advanced Layer 7 routing, traffic splitting, header modifications, and cross-namespace routing.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── API Gateway/
    ├── gateway-class.yaml   # GatewayClass resource (Infrastructure provider)
    ├── gateway.yaml         # Gateway resource (Port and protocol listeners)
    ├── httproute.yaml       # HTTPRoute resource (Routing paths and backends)
    └── README.md            # This documentation file
```

---

## 🧩 Core Fields Used in Gateway API YAML

### 1. GatewayClass
* **apiVersion**: `gateway.networking.k8s.io/v1`
* **kind**: `GatewayClass`
* **spec.controllerName**: The unique identifier of the controller implementing the class (e.g. `nginx.org/gateway-controller`).

### 2. Gateway
* **apiVersion**: `gateway.networking.k8s.io/v1`
* **kind**: `Gateway`
* **spec.gatewayClassName**: References the matching `GatewayClass` template.
* **spec.listeners**: List of logical endpoints exposed by the gateway:
  - **port**: External port (e.g., `80` or `443`).
  - **protocol**: Networking protocol (`HTTP`, `HTTPS`, `TCP`, `UDP`).
  - **allowedRoutes**: Filters which namespaces can attach routing definitions (like `HTTPRoute`) to this listener.

### 3. HTTPRoute
* **apiVersion**: `gateway.networking.k8s.io/v1`
* **kind**: `HTTPRoute`
* **spec.parentRefs**: Links the route to the target `Gateway` listener.
* **spec.rules**: Rules to evaluate traffic:
  - **matches**: Conditions matching URL paths, headers, or query parameters.
  - **backendRefs**: Destination Kubernetes Service and port mappings.

---

## 📘 Practical Examples

### 1. Gateway Routing Configuration
Below is a unified view of how a `Gateway` attaches to its `GatewayClass` and routes traffic to a backend service using an `HTTPRoute`.

* **Gateway Listener (`gateway.yaml`):**
```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: prod-gateway
  namespace: default
spec:
  gatewayClassName: example-gateway-class
  listeners:
    - name: http
      protocol: HTTP
      port: 80
      allowedRoutes:
        namespaces:
          from: Same
```

* **Routing Rule (`httproute.yaml`):**
```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: app-route
  namespace: default
spec:
  parentRefs:
    - name: prod-gateway
  rules:
    - matches:
        - path:
            type: PathPrefix
            value: /app
      backendRefs:
        - name: app-service
          port: 80
```

---

## ▶️ kubectl Operations

> ⚠️ **Prerequisite:** Just like Ingress, the Gateway API requires a supporting controller (e.g., NGINX, Envoy Gateway, Contour, Cilium, or Istio) installed in the cluster to manage the resources.

### Apply the Manifests
```bash
# Note: Escape the folder name with double quotes in your shell
kubectl apply -f "kubernetes_manifest_files/API Gateway/"
```

### Verify Resources
```bash
# List Gateway Classes
kubectl get gatewayclass

# List active Gateways and their external IPs
kubectl get gateway

# Check HTTPRoute configurations
kubectl get httproute
```

### Delete Resources
```bash
kubectl delete -f "kubernetes_manifest_files/API Gateway/"
```

---

## 🆚 Gateway API vs Ingress

| Feature | Ingress | Gateway API (Modern) |
| :--- | :--- | :--- |
| **API Expressiveness** | Limited (requires vendor-specific annotations) | High (custom filters, headers, redirects built-in) |
| **Role-Oriented** | Single resource handles entire configuration | Split resources (`Gateway` for Ops, `HTTPRoute` for Devs) |
| **Cross-Namespace Routing** | Hard to implement | Supported out-of-the-box via `ReferenceGrant` |

---

## 🛣️ Learning Path Navigation

    ⏮️ **[Ingress](../Ingress/README.md)** | ⏭️ **[Namespace](../Namespace/README.md)**
