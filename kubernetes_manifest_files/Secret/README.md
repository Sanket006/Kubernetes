# ☸️ Kubernetes Secret Manifests

This folder contains **Kubernetes Secret manifest examples**. A **Secret** is an API object used to store and manage sensitive information, such as passwords, OAuth tokens, ssh keys, and certificates.

Storing confidential data in a Secret is safer and more flexible than putting it verbatim in a Pod definition or container image. This decoupling allows you to control access to sensitive configurations separately from resource templates.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Secret/
    ├── secret.yaml              # Base64-encoded Opaque Secret definition
    ├── secret-pod.yaml          # Pod mounting Secret as files or specific key envs
    ├── secret-pod-envfrom.yaml  # Pod injecting all keys from Secret as env vars
    └── README.md                # This documentation file
```

---

## 🧩 Core Fields Used in Secret YAML

* **apiVersion**: `v1`
* **kind**: `Secret`
* **metadata**: Name, namespace, and labels.
* **type**: Defines the Secret category:
  - `Opaque` (default): Arbitrary user-defined data.
  - `kubernetes.io/tls`: Public/private TLS certificate keys.
  - `kubernetes.io/dockerconfigjson`: Docker registry credentials.
* **data**: Key-value pairs containing **base64-encoded** strings.
* **stringData**: Optional field to write plain-text values that Kubernetes will automatically base64-encode during creation.

---

## 📘 Practical Examples

### 1. Secret Manifest (`secret.yaml`)
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  MYSQL_USER: dGVzdHVzZXI=                   # base64 for 'testuser'
  MYSQL_PASSWORD: dGVzdHBhc3M=               # base64 for 'testpass'
  API_KEY: YXBpa2V5MTIzNDU=                  # base64 for 'apikey12345'
```

### 2. Injecting Secrets as Environment Variables (`secret-pod-envfrom.yaml`)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
    - name: myapp-container
      image: nginx:1.25.3-alpine
      envFrom:
        - secretRef:
            name: app-secret                 # Injects all key-value pairs as env vars
      resources:
        requests:
          memory: "64Mi"
          cpu: "100m"
        limits:
          memory: "128Mi"
          cpu: "200m"
```

---

## ▶️ kubectl Operations

### Apply the Manifests
```bash
kubectl apply -f kubernetes_manifest_files/Secret/
```

### Verify & Decode Secrets
```bash
# List all Secrets
kubectl get secrets

# View Secret metadata (Values are hidden under 'describe')
kubectl describe secret app-secret

# Decode and view the plain text password value
kubectl get secret app-secret -o jsonpath="{.data.MYSQL_PASSWORD}" | base64 --decode
```

### Delete Secret
```bash
kubectl delete -f kubernetes_manifest_files/Secret/
```

---

## 🛡️ Best Practices & Common Mistakes

* **Base64 is NOT Encryption:** Base64 is merely encoding and can be easily decoded. Enable **Encryption at Rest** in your Kubernetes API server and use role-based access control (RBAC) to lock down secret access.
* **Mounting Secrets as Volumes:** Mount secrets as files for security-sensitive operations like TLS certificates. Mounted secrets are dynamically updated in the container when changed.

---

## 🛣️ Learning Path Navigation

    ⏮️ **[ConfigMap](../ConfigMap/README.md)** | ⏭️ **[HPA](../HPA/README.md)**
