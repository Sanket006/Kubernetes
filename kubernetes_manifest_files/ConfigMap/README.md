# ☸️ Kubernetes ConfigMap Manifests

This folder contains **Kubernetes ConfigMap manifest examples**. A **ConfigMap** is an API object used to store non-confidential data in key-value pairs. 

ConfigMaps allow you to decouple environment-specific configuration (such as database URLs, ports, log levels, and properties) from container images, making your applications easily portable across environments without rebuilding container images.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── ConfigMap/
    ├── app-configmap.yaml               # Standard ConfigMap definition
    ├── configmap-pod.yaml               # Injecting ConfigMap as env vars and mounting as a file volume
    ├── configmap-pod-specific-env.yaml  # Injecting specific keys from ConfigMap
    ├── configmap-deployment.yaml        # Consuming ConfigMap inside a Deployment
    └── README.md                        # This documentation file
```

---

## 🧩 Core Fields Used in ConfigMap YAML

* **apiVersion**: `v1`
* **kind**: `ConfigMap`
* **metadata**: Name, namespace, and labels.
* **data**: Key-value pairs of configuration data (strings).
* **binaryData**: Optional base64-encoded binary data.

---

## 📘 Practical Examples

### 1. ConfigMap Manifest (`app-configmap.yaml`)
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "development"
  APP_DEBUG: "true"
  DATABASE_URL: "jdbc:mysql://mysql-svc:3306/testdb"
  WELCOME_MESSAGE: "Welcome to Developer Environment!"
```

### 2. Injecting Specific Keys in Pod Env (`configmap-pod-specific-env.yaml`)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configmap-pod-specific-env
spec:
  containers:
    - name: app-container
      image: nginx:1.25.3-alpine
      env:
        - name: DEBUG_MODE
          valueFrom:
            configMapKeyRef:
              name: app-config               # Name of ConfigMap
              key: APP_DEBUG                 # Key to extract
```

---

## ▶️ kubectl Operations

### Apply the Manifests
```bash
kubectl apply -f kubernetes_manifest_files/ConfigMap/
```

### Verify & Describe
```bash
# List all ConfigMaps
kubectl get configmaps

# View key-value data inside a ConfigMap
kubectl describe configmap app-config
```

### Delete ConfigMap
```bash
kubectl delete -f kubernetes_manifest_files/ConfigMap/
```

---

## 🔑 Injection Methods Comparison

1. **`envFrom`**: Injects all key-value pairs in the ConfigMap as environment variables directly inside the container.
2. **`valueFrom.configMapKeyRef`**: Selects specific keys from the ConfigMap to inject as environment variables.
3. **Volume Mount**: Mounts the ConfigMap as a directory inside the container, where each key becomes a filename and the value is the file's content. Mounted volumes auto-update in real-time when the ConfigMap is modified.

---

## 🛡️ Best Practices & Common Mistakes

* **Never Store Sensitive Data:** ConfigMaps are not encrypted. For passwords, API keys, and certificates, use **Secrets**.
* **Volume Mount Auto-Updates:** Files mounted via ConfigMap volumes update automatically within a few minutes after the ConfigMap is edited. However, environment variables injected via `envFrom` or `configMapKeyRef` will **not** update until the Pod is restarted.

---

## 🛣️ Learning Path Navigation

    ⏮️ **[Namespace](../Namespace/README.md)** | ⏭️ **[Secret](../Secret/README.md)**
