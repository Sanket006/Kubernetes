# Kubernetes Ingress Manifests

This folder contains **Kubernetes Ingress manifest files**. An **Ingress** is a collection of rules that allow **external HTTP and HTTPS traffic** to reach Kubernetes Services.

Ingress provides **fine-grained routing, SSL termination, and name-based virtual hosting**, acting as an entry point to your cluster's applications.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Ingress/
    ├── ingress-host-based-routing.yaml   
    ├── ingress-path-based-routing.yaml
    ├── ingress-setup.md
    └── README.md
```

This folder includes YAML files defining Ingress resources.

---

## 📘 What is a Kubernetes Ingress?

An **Ingress** manages external access to services in a cluster, typically HTTP/HTTPS. It:

* Provides routing based on host and path
* Supports TLS/SSL termination
* Enables name-based virtual hosting
* Works with Ingress Controllers for traffic management

In simple terms:

> **Ingress is like a smart router that controls HTTP/HTTPS traffic into your cluster.**

Ingress supports two primary routing types:

1. **Host-based routing** – routes requests based on the `host` field (e.g., `example.com`) to different services.
2. **Path-based routing** – routes requests based on the URL `path` (e.g., `/app` or `/blog`) to specific services.

---

## 🎓 What You Learn from Ingress

By working with Ingress manifests, you will understand:

* How external traffic enters a Kubernetes cluster
* Path-based and host-based routing
* TLS/SSL setup for secure communication
* Integration with Services and Deployments
* Role of Ingress Controllers

---

## 🧩 Core Fields Used in Ingress YAML

A typical Ingress manifest includes:

* **apiVersion**: `networking.k8s.io/v1`
* **kind**: `Ingress`
* **metadata**:

  * `name`: Ingress name
  * `annotations`: Metadata for ingress controller configuration
* **spec**:

  * `ingressClassName`: Specifies the Ingress Controller
  * `tls` (optional): TLS configuration

    * `hosts`: List of hosts for TLS
    * `secretName`: TLS secret containing certificate and key
  * `rules`: Array of routing rules

    * `host`: Domain name for host-based routing
    * `http.paths`: Array of path rules for path-based routing

      * `path`: URL path
      * `pathType`: `Prefix` or `Exact`
      * `backend.service.name`: Target Service name
      * `backend.service.port.number`: Service port

**Example YAML with host and path-based routing:**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - example.com
    secretName: example-tls
  rules:
  - host: example.com        # Host-based routing
    http:
      paths:
      - path: /app            # Path-based routing
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80
      - path: /blog           # Another path-based route
        pathType: Prefix
        backend:
          service:
            name: blog-service
            port:
              number: 80
```

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply Ingress manifests:

```bash
kubectl apply -f kubernetes_manifest_files/Ingress/
```

Verify Ingress resources:

```bash
kubectl get ingress
```

Describe an Ingress:

```bash
kubectl describe ingress <ingress-name>
```

Check endpoints via Service:

```bash
kubectl get svc <service-name>
```

---

## 🔍 Ingress Working Flow

1. External client sends HTTP/HTTPS request
2. DNS resolves to Ingress Controller (LoadBalancer/NodePort)
3. Ingress Controller evaluates **host and path rules**
4. Request is routed to the appropriate Service
5. Service forwards traffic to Pods

```
Client → Ingress Controller → Service → Pods
```

---

## 🧪 Common Use Cases

* Exposing multiple services via a single IP/Domain
* Implementing TLS/SSL termination
* Path-based routing for microservices
* Host-based routing for multiple domains
* Name-based virtual hosting

---

## ⚠️ Common Mistakes

* Not installing an Ingress Controller
* Missing `ingressClassName` or incorrect annotations
* Service names or ports mismatched in backend
* Forgetting TLS secret configuration

---

## ✅ Best Practices

* Use an Ingress Controller (e.g., NGINX, Traefik) compatible with your cluster
* Use TLS for secure traffic
* Validate Service names and ports
* Keep path and host rules clear and organized
* Monitor Ingress logs and metrics for traffic issues

---

## 🛣️ Learning Path Linkage

### Before Ingress

➡️ **Pod → Deployment → Service → Namespace**
Understand resource deployment, scaling, and service exposure

### After Ingress

➡️ **HPA → PersistentVolume / PVC**
Learn autoscaling and storage management with cluster access

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Traffic Management with Ingress! 🚀
