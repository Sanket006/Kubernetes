# Kubernetes Service Manifests

This directory contains **Kubernetes Service manifest files**. A **Service** provides a stable network endpoint to access one or more Pods, enabling communication within the cluster and from external clients.

Services solve the problem of **dynamic Pod IP addresses** by offering a consistent IP, DNS name, and port for accessing applications.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Service/
    ├── service.yaml   
    └── README.md
```

This folder contains YAML files defining different types of Kubernetes Services.

---

## 📘 What is a Kubernetes Service?

A **Kubernetes Service** is an abstraction that defines a **stable network endpoint** for a group of Pods.

In Kubernetes, Pods are **ephemeral** — they can be created, destroyed, or rescheduled at any time, and their IP addresses change frequently. A Service solves this problem by:

* Providing a **fixed IP address** and **DNS name**
* Load-balancing traffic across multiple Pods
* Decoupling application access from Pod lifecycle

In simple terms:

> **Pods come and go, but a Service stays constant.**

---

## 🎓 What You Learn from Services

By working with Service manifests, you will understand:

* How Kubernetes networking works
* How Pods discover and communicate with each other
* How stable access is provided to dynamic workloads
* How traffic is load-balanced across replicas
* How Services integrate with Ingress and external traffic

---

## 🧩 Core Fields Used in Service YAML (Detailed)

A typical Kubernetes Service manifest includes:

* **apiVersion: v1**
  Specifies the Kubernetes API version for Service resources.

* **kind: Service**
  Declares the resource type.

* **metadata**

  * `name`: Unique name of the Service
  * `labels`: Key-value metadata for organization and selection

* **spec**

  * `selector`: Matches Pods using labels (traffic is routed only to matching Pods)
  * `ports`: Defines how traffic flows

    * `port`: Service port
    * `targetPort`: Container port
    * `protocol`: TCP/UDP (default: TCP)
  * `type`: Determines how the Service is exposed

---

## 🔑 Kubernetes Service Types (Detailed Explanation)

### 🟦 ClusterIP (Default)

* Assigns an internal virtual IP
* Accessible **only inside the cluster**
* Used for internal microservice communication

**Example use case:** Backend API accessed by frontend Pods

---

### 🟦 NodePort

* Exposes the Service on a static port on every node
* Port range: `30000–32767`
* Accessible via `<NodeIP>:<NodePort>`

**Example use case:** Quick testing without Ingress

---

### 🟦 LoadBalancer

* Provisions an external load balancer (cloud only)
* Automatically assigns a public IP
* Internally uses ClusterIP + NodePort

**Example use case:** Production workloads in AWS / Azure / GCP

---

### 🟦 ExternalName

* Maps the Service to an external DNS name
* No selector or Pod routing involved

**Example use case:** Accessing an external database or SaaS endpoint

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply all Service manifests:

```bash
kubectl apply -f kubernetes_manifest_files/Service/
```

Verify Services:

```bash
kubectl get svc
```

Describe a specific Service:

```bash
kubectl describe svc <service-name>
```

Check Service endpoints:

```bash
kubectl get endpoints
```

---

## 🔍 Kubernetes Service Networking Flow

1. Pods are created with labels (e.g., `app: nginx`)
2. A Service defines a selector matching those labels
3. Kubernetes creates **Endpoints** for matching Pods
4. kube-proxy programs networking rules
5. Traffic sent to the Service IP is load-balanced to Pods

```
Client → Service IP/DNS → kube-proxy → Pod
```

---

## 🧪 Common Use Cases

* Exposing applications to other Pods
* Providing stable access to Deployments
* Load balancing traffic across replicas
* Integrating with Ingress resources

---

## ⚠️ Common Mistakes

* Service selector does not match Pod labels
* Using NodePort in production instead of Ingress
* Exposing unnecessary ports
* Forgetting `targetPort` mapping

---

## ✅ Best Practices

* Use **ClusterIP** for internal communication
* Use **Ingress** for HTTP/HTTPS traffic
* Keep labels simple and consistent
* Document exposed ports clearly
* Use readiness probes for accurate load balancing

---

## 🛣️ Learning Path Linkage

### Before Services

➡️ **Pod → Deployment**
Understand how applications run and scale

### After Services

➡️ **Ingress → HPA → PersistentVolume / PVC**
Learn traffic routing, autoscaling, and storage

---

## 📬 Support & Contributions

For improvements or questions:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Networking! 🚀
