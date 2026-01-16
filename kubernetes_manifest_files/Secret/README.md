# Kubernetes Secret Manifests

This folder contains **Kubernetes Secret manifest files**. A **Secret** is a Kubernetes object used to store **sensitive information** such as passwords, tokens, and keys.

Secrets help you **keep confidential data separate from Pod definitions** and prevent exposing sensitive data in plain text.

---

## 📁 Folder Structure

```
kubernetes_manifest_files/
└── Secret/
    ├── secret-pod-envfrom.yaml   
    ├── secret-pod.yaml
    ├── secret.yaml
    └── README.md
```

This folder includes YAML files defining Secret resources.

---

## 📘 What is a Kubernetes Secret?

A **Secret** stores sensitive information securely. Key points:

* Data is base64-encoded (not encrypted by default)
* Can be consumed as environment variables, mounted files, or image pull secrets
* Keeps sensitive data separate from container images and manifests

In simple terms:

> **Secret lets you provide sensitive data to Pods without exposing it in plain text.**

---

## 🎓 What You Learn from Secrets

By working with Secret manifests, you will understand:

* How to store sensitive data in Kubernetes
* How to consume Secrets as environment variables or volume-mounted files
* How to use Secrets for image pull credentials
* Best practices for securing sensitive data in Kubernetes

---

## 🧩 Core Fields Used in Secret YAML

A typical Secret manifest includes:

* **apiVersion**: `v1`
* **kind**: `Secret`
* **metadata**:

  * `name`: Secret name
  * `namespace`: Namespace (optional)
* **type**: Type of Secret (`Opaque`, `kubernetes.io/dockerconfigjson`, `kubernetes.io/tls`, etc.)
* **data**: Base64-encoded key-value pairs
* **stringData**: Optional, plain text key-value pairs that Kubernetes will encode automatically

**Example YAML (Opaque Secret):**

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YWRtaW4=   # base64 for 'admin'
  password: MWYyZDFlMmU2N2Rm   # base64 for '1f2d1e2e67df'
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
    env:
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: username
    - name: DB_PASS
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: password
```

---

## ▶️ kubectl Commands (Apply, Verify & Describe)

Apply Secret manifests:

```bash
kubectl apply -f kubernetes_manifest_files/Secret/
```

Verify Secrets:

```bash
kubectl get secrets
```

Describe a Secret:

```bash
kubectl describe secret <secret-name>
```

Decode Secret values:

```bash
kubectl get secret <secret-name> -o jsonpath="{.data.username}" | base64 --decode
```

Delete a Secret:

```bash
kubectl delete secret <secret-name>
```

---

## 🔍 Secret Working Flow

1. Create a Secret with sensitive data
2. Pod references Secret via environment variables, volume, or image pull
3. Pod consumes the Secret data securely

```
Secret → Pod (Env vars / Volume / Image Pull) → Application
```

---

## 🧪 Common Use Cases

* Database credentials
* API keys and tokens
* TLS certificates
* Docker registry authentication

---

## ⚠️ Common Mistakes

* Storing Secrets in plain text in manifests or images
* Not using correct Secret type for use case
* Not restricting namespace or RBAC access
* Forgetting to decode base64 values when necessary

---

## ✅ Best Practices

* Use Secrets for all sensitive data
* Use RBAC to control access to Secrets
* Avoid embedding Secrets in container images or ConfigMaps
* Use `stringData` for simplicity when creating Secrets
* Combine with volume mounts for TLS certificates or configuration files

---

## 🛣️ Learning Path Linkage

### Before Secret

➡️ **ConfigMap → Namespace**
Understand non-sensitive configuration management and namespace isolation

### After Secret

➡️ **DaemonSet → StatefulSet → HPA → PersistentVolume / PVC**
Learn cluster-wide services, stateful workloads, autoscaling, and persistent storage

---

## 📬 Support & Contributions

For questions, issues, or improvements:

* Open a GitHub issue
* Submit a Pull Request

---

Happy Kubernetes Secret Management! 🔐
