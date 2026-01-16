# Kubernetes ConfigMap Manifests

This folder contains **Kubernetes ConfigMap manifest files**. A **ConfigMap** is a Kubernetes object that stores **configuration data in key-value pairs**, which can be consumed by Pods as environment variables, command-line arguments, or configuration files.

ConfigMaps decouple **configuration from container images**, enabling dynamic configuration changes without rebuilding images.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── ConfigMap/
    ├── app-configmap.yaml  
    ├── configmap-deployment.yaml
    ├── configmap-pod-specific-env.yaml
    ├── configmap-pod.yaml
    └── README.md
```

This folder includes YAML files defining ConfigMap resources.

---

## 📘 What is a Kubernetes ConfigMap?

A **ConfigMap** stores configuration data for applications. Key points:

* Stores non-confidential configuration data
* Can be consumed as environment variables, volume-mounted files, or command arguments
* Supports multiple data sources in a single object

In simple terms:

> **ConfigMap lets you provide configuration to your Pods without baking it into the container image.**

---

## 🎓 What You Learn from ConfigMaps

By working with ConfigMap manifests, you will understand:

* How to separate configuration from code
* Consuming configuration as environment variables or files
* Updating ConfigMaps without redeploying containers
* Integration with Deployments, Pods, and StatefulSets

---

## 🧩 Core Fields Used in ConfigMap YAML

A typical ConfigMap manifest includes:

* **apiVersion**: `v1`
* **kind**: `ConfigMap`
* **metadata**:

  * `name`: ConfigMap name
  * `namespace`: Namespace (optional)
* **data**: Key-value pairs containing configuration data
* **binaryData**: Optional base64-encoded binary data

**Example YAML:**

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  LOG_LEVEL: DEBUG
  APP_PORT: "8080"
```

Consume in Pod as environment variables:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: app-container
    image: myapp:1.0
    envFrom:
    - configMapRef:
        name: app-config
```

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply ConfigMap manifests:

```bash
kubectl apply -f kubernetes_manifest_files/ConfigMap/
```

Verify ConfigMaps:

```bash
kubectl get configmap
```

Describe a ConfigMap:

```bash
kubectl describe configmap <configmap-name>
```

Delete a ConfigMap:

```bash
kubectl delete configmap <configmap-name>
```

---

## 🔍 ConfigMap Working Flow

1. Create ConfigMap with key-value data
2. Pods reference ConfigMap via environment variables, volume mounts, or command arguments
3. Changes to ConfigMap can be propagated to Pods (depending on consumption method)

```
ConfigMap → Pod (Env vars / Volume / Args) → Application
```

---

## 🧪 Common Use Cases

* Application configuration (ports, log levels, feature flags)
* Environment-specific settings (dev/test/prod)
* Passing configuration files to containers
* Centralized configuration management

---

## ⚠️ Common Mistakes

* Mounting ConfigMap keys that don’t exist
* Not handling updates for running Pods properly
* Storing sensitive data in ConfigMaps (use Secret instead)
* Naming conflicts across namespaces

---

## ✅ Best Practices

* Use ConfigMap for non-sensitive configuration only
* Keep key names consistent and meaningful
* Combine with Secrets for sensitive data
* Monitor usage and update strategy for running Pods
* Use versioning or naming conventions for updates

---

## 🛣️ Learning Path Linkage

### Before ConfigMap

➡️ **Pod → Deployment → Service → Ingress → Namespace**
Understand workload deployment and namespace isolation

### After ConfigMap

➡️ **Secret → DaemonSet → StatefulSet → HPA**
Learn secure configuration, cluster-wide services, and autoscaling

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Configuration Management with ConfigMaps! 🚀
