# ☸️ Kubernetes Manifest Files

<div align="center">

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![YAML](https://img.shields.io/badge/YAML-CB171E?style=for-the-badge&logo=yaml&logoColor=white)
![AWS EBS](https://img.shields.io/badge/AWS_EBS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)

*A structured collection of production-ready Kubernetes manifests — covering all 15 core resource types from Pod to StatefulSet, HPA, Ingress, Secrets, and dynamic/static PV provisioning*

</div>

---

## 📌 Overview

This repository contains **ready-to-apply Kubernetes YAML manifests** organized by resource type. Every folder includes standalone manifests, combined multi-resource files, and detailed READMEs — making this a complete reference for learning and real-world Kubernetes deployments.

---

## 📁 Repository Structure

```
kubernetes_manifest_files/
├── Pod/                          # Basic pod + pod with NodePort service
├── ReplicationController/        # Legacy RC (learning/comparison)
├── ReplicaSet/                   # ReplicaSet + NodePort service
├── Deployment/                   # Deployment + rolling update + NodePort service
├── DaemonSet/                    # DaemonSet + NodePort service
├── StatefulSet/                  # MySQL (with PVC) + Nginx (with headless svc)
├── Service/                      # ClusterIP / NodePort / LoadBalancer types
├── Ingress/                      # Host-based + path-based routing + setup guide
├── HPA/                          # CPU-based, memory-based, combined HPA
├── ConfigMap/                    # ConfigMap + envFrom + specific key injection + volume mount
├── Secret/                       # Opaque secret + envFrom + secretKeyRef + volume mount
├── Namespace/                    # Namespace + scoped Pod
├── PersistentVolume/             # PV, PVC, Pod, Deployment + node affinity variant
├── Static Provisioning PV/       # AWS EBS manual PV + PVC + Pod
└── Dynamic Provisioning PV/      # StorageClass (ebs.csi.aws.com) + PVC + Pod
```

---

## 📦 What's Inside Each Folder

| Folder | Key Files | Resources |
|---|---|---|
| `Pod/` | `pod.yaml`, `nginx-pod-service.yaml` | Pod, NodePort Service |
| `ReplicationController/` | `replicationcontroller.yaml`, `nginx-rc-service.yaml` | RC (3 replicas), NodePort Service |
| `ReplicaSet/` | `replicaset.yaml`, `nginx-replicaset-service.yaml` | RS (3 replicas), NodePort Service |
| `Deployment/` | `deployment.yaml`, `nginx-deployment-service.yaml` | Deployment (3 replicas), NodePort Service |
| `DaemonSet/` | `daemonset.yaml`, `nginx-daemonset-service.yaml` | DaemonSet, NodePort Service |
| `StatefulSet/` | `mysql-statefulset.yaml`, `mysql-statefulset-persistentvolume.yaml`, `nginx-statefulset-service.yaml` | StatefulSet + Headless Service + volumeClaimTemplates |
| `Service/` | `service.yaml` | ClusterIP Service skeleton |
| `Ingress/` | `ingress-host-based-routing.yaml`, `ingress-path-based routing.yaml`, `ingress-setup.md` | Host-based Ingress, path-based Ingress, NGINX Ingress setup guide |
| `HPA/` | `hpa-cpu-based.yaml`, `hpa-memory-based.yaml`, `hpa-cpu+memory.yaml`, `deployment+hpa(cpu+mem).yaml` | 4 HPA configs using `autoscaling/v2` |
| `ConfigMap/` | `app-configmap.yaml`, `configmap-pod.yaml`, `configmap-pod-specific-env.yaml`, `configmap-deployment.yaml` | ConfigMap + all 3 injection methods |
| `Secret/` | `secret.yaml`, `secret-pod.yaml`, `secret-pod-envfrom.yaml` | Opaque Secret + secretKeyRef + envFrom + volume mount |
| `Namespace/` | `namespace.yaml`, `namespace-pod.yaml` | Namespace with labels, scoped Pod |
| `PersistentVolume/` | `pv.yaml`, `pvc.yaml`, `pv-pvc.yaml`, `pv-pvc-pod.yaml`, `pv-pvc-deployment.yaml`, `pv-pvc-pod-using-node-affinity.yaml` | Static hostPath PV (5Gi, Retain, manual), PVC, Pod, Deployment, Node Affinity variant |
| `Static Provisioning PV/` | `AWS EBS + PV + PVC Manifest File.yaml` | AWS EBS PV (`vol-0abcd...`, `ap-south-1a`), PVC, Pod |
| `Dynamic Provisioning PV/` | `StorageClass + PVC + Pod.yaml`, `Full-manifest-file.yaml` | StorageClass (`ebs.csi.aws.com`, `WaitForFirstConsumer`), PVC (2Gi), Pod |

---

## 🗂️ Manifest Details

---

### 🔵 Pod

Two files covering the simplest deployable unit:

```yaml
# pod.yaml — bare minimum Pod
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: nginx-container
      image: nginx
      ports:
        - containerPort: 80
```

`nginx-pod-service.yaml` adds a **NodePort Service** that selects Pods via `app: nginx-app` label.

---

### 🔵 ReplicationController (Legacy)

Uses `apiVersion: v1` (not `apps/v1`), equality-based selector only. Kept for learning and comparison with ReplicaSet.

```yaml
# replicationcontroller.yaml
replicas: 3
selector:
  app: nginx-app        # equality-based only — no set-based support
```

---

### 🔵 ReplicaSet

Uses `apps/v1` with `matchLabels` (set-based selector). 3 replicas of `nginx:latest`.

```yaml
selector:
  matchLabels:
    app: nginx-app      # set-based — supports In, NotIn, Exists
```

`nginx-replicaset-service.yaml` combines RS + NodePort Service in one file.

---

### 🔵 Deployment

Two files — a bare skeleton (`deployment.yaml`) and a complete production example (`nginx-deployment-service.yaml`):

```yaml
# nginx-deployment-service.yaml
replicas: 3
image: nginx:latest
type: NodePort
```

> ⚠️ Note: `deployment.yaml` contains a YAML syntax error — `apiVersion: v1` should be `apps/v1`, and `template.metadata.labels` is missing its map structure. Use `nginx-deployment-service.yaml` as the reference.

---

### 🔵 DaemonSet

Runs `nginx:latest` on every node. `nginx-daemonset-service.yaml` adds a **NodePort Service** for external access:

```yaml
# nginx-daemonset-service.yaml
type: NodePort     # ClusterIP / NodePort / LoadBalancer
```

---

### 🔵 StatefulSet

Three files covering different use cases:

**`mysql-statefulset.yaml`** — MySQL 8.0, 3 replicas, environment variables (root password, DB name, user, password):
```yaml
image: mysql:8.0
replicas: 3
env:
  MYSQL_ROOT_PASSWORD: rootpassword
  MYSQL_DATABASE: testdb
  MYSQL_USER: testuser
  MYSQL_PASSWORD: testpass
```

**`mysql-statefulset-persistentvolume.yaml`** — MySQL 8.0, 1 replica + headless Service + `volumeClaimTemplates` (2Gi, RWO), data persisted at `/var/lib/mysql`:
```yaml
clusterIP: None                    # Headless service — required for StatefulSet DNS
volumeClaimTemplates:
  - storage: 2Gi
    mountPath: /var/lib/mysql
```

**`nginx-statefulset-service.yaml`** — Nginx StatefulSet, 3 replicas + headless Service + `volumeClaimTemplates` (1Gi per Pod):
```yaml
volumeClaimTemplates:
  - storage: 1Gi
    mountPath: /usr/share/nginx/html
```

---

### 🔵 Service

Four types documented with examples:

| Type | Access | Port Range |
|---|---|---|
| `ClusterIP` | Internal only | — |
| `NodePort` | `<NodeIP>:<port>` | 30000–32767 |
| `LoadBalancer` | External (cloud) | — |
| `ExternalName` | External DNS | — |

---

### 🔵 Ingress

**`ingress-host-based-routing.yaml`** — routes two domains to two different services:
```yaml
ingressClassName: nginx
rules:
  - host: app1.example.com → app1-service:80
  - host: app2.example.com → app2-service:80
```

**`ingress-path-based routing.yaml`** — routes URL paths on one domain:
```yaml
host: example.com
annotations:
  nginx.ingress.kubernetes.io/rewrite-target: /
paths:
  /app1 → app1-service:80
  /app2 → app2-service:80
```

**`ingress-setup.md`** — complete NGINX Ingress Controller setup guide:
```bash
# Install NGINX Ingress Controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml

# Verify
kubectl get pods -n ingress-nginx
kubectl get svc -n ingress-nginx
kubectl get ingressclass
```

---

### 🔵 HPA

All 4 files use `autoscaling/v2` (not deprecated `v2beta2`):

| File | Target | Min | Max | Metric |
|---|---|---|---|---|
| `hpa-cpu-based.yaml` | `nginx-deployment` | 1 | 5 | CPU @ 50% |
| `hpa-memory-based.yaml` | `nginx-deployment` | 1 | 5 | Memory @ 70% |
| `hpa-cpu+memory.yaml` | `nginx-deployment` | 1 | 10 | CPU @ 60% + Memory @ 75% |
| `deployment+hpa(cpu+mem).yaml` | `nginx-deployment` | 2 | 10 | CPU @ 60% + Memory @ 75% |

The combined file also includes the Deployment with resource requests (`cpu: 100m`, `memory: 128Mi`) and limits (`cpu: 300m`, `memory: 256Mi`) — required for HPA to function.

---

### 🔵 ConfigMap

ConfigMap `app-config` stores 4 keys: `APP_ENV`, `APP_DEBUG`, `DATABASE_URL`, `WELCOME_MESSAGE`.

Three injection methods demonstrated:

| File | Method |
|---|---|
| `configmap-pod.yaml` | `envFrom.configMapRef` — all keys as env vars + volume mount |
| `configmap-pod-specific-env.yaml` | `configMapKeyRef` — inject specific keys only |
| `configmap-deployment.yaml` | Deployment with `envFrom` + volume mount at `/etc/config` |

---

### 🔵 Secret

Secret `app-secret` (type: `Opaque`) stores 3 base64-encoded keys:

```yaml
MYSQL_USER: dGVzdHVzZXI=       # testuser
MYSQL_PASSWORD: dGVzdHBhc3M=   # testpass
API_KEY: YXBpa2V5MTIzNDU=      # apikey12345
```

Three consumption methods:

| File | Method |
|---|---|
| `secret.yaml` | Secret definition only |
| `secret-pod-envfrom.yaml` | `envFrom.secretRef` — all keys as env vars |
| `secret-pod.yaml` | `secretKeyRef` per key + volume mount at `/etc/secret-data` (readOnly) |

---

### 🔵 Namespace

`namespace.yaml` — creates `dev-team` namespace with labels `env: development`, `team: backend`.

`namespace-pod.yaml` — same Namespace + Nginx Pod deployed into it via `namespace: dev-team`.

---

### 🔵 PersistentVolume

All static PVs use `hostPath: /mnt/data`, `storageClassName: manual`, `5Gi`, `ReadWriteOnce`, `Retain`.

| File | Resources |
|---|---|
| `pv.yaml` | PV only |
| `pvc.yaml` | PVC only (`storageClassName: manual`) |
| `pv-pvc.yaml` | PV + PVC together |
| `pv-pvc-pod.yaml` | PV + PVC + Pod (data at `/usr/share/nginx/html`) |
| `pv-pvc-deployment.yaml` | PV + PVC + Deployment (2 replicas) |
| `pv-pvc-pod-using-node-affinity.yaml` | Local PV with `nodeAffinity` (requires `disk=ssd` label) + PVC + Pod with matching node affinity |

---

### 🔵 Static Provisioning PV (AWS EBS)

```yaml
# AWS EBS + PV + PVC Manifest File.yaml
awsElasticBlockStore:
  volumeID: vol-0abcd1234ef567890   # Replace with real EBS Volume ID
  fsType: ext4
nodeAffinity:
  required:
    - topology.kubernetes.io/zone: ap-south-1a   # Must match EBS AZ
storageClassName: ""                              # Empty = manual binding
capacity: 5Gi
accessModes: ReadWriteOnce
reclaimPolicy: Retain
```

Includes the matching PVC (`storageClassName: ""`) and Pod (mounts at `/data`).

---

### 🔵 Dynamic Provisioning PV

**`StorageClass + PVC + Pod.yaml`** — production-ready dynamic provisioning:
```yaml
provisioner: ebs.csi.aws.com               # AWS EBS CSI driver
volumeBindingMode: WaitForFirstConsumer    # Zone-aware binding
reclaimPolicy: Delete
# PVC requests 2Gi — K8s auto-creates the EBS volume
```

**`Full-manifest-file.yaml`** — alternative using legacy `kubernetes.io/aws-ebs` provisioner with `gp2` type.

---

## 🎯 Recommended Learning Order

```
1. Pod                         ← Smallest deployable unit
2. ReplicationController       ← Legacy controller (understand history)
3. ReplicaSet                  ← Modern replacement for RC
4. Deployment                  ← Production standard for stateless apps
5. Service                     ← ClusterIP → NodePort → LoadBalancer
6. Ingress                     ← HTTP routing + NGINX controller setup
7. Namespace                   ← Cluster isolation and multi-tenancy
8. ConfigMap                   ← Non-sensitive config injection
9. Secret                      ← Sensitive data management
10. DaemonSet                  ← Node-level cluster services
11. HPA                        ← Automatic scaling based on metrics
12. PersistentVolume + PVC     ← Static storage provisioning
13. StatefulSet                ← Stateful apps (MySQL, databases)
14. Static Provisioning PV     ← AWS EBS manual volume allocation
15. Dynamic Provisioning PV    ← StorageClass + auto-provisioned volumes
```

---

## 🚀 Getting Started

### Prerequisites

- Kubernetes cluster (Minikube / EKS / k3s / Docker Desktop)
- `kubectl` configured

```bash
# Verify cluster access
kubectl cluster-info
kubectl get nodes
```

### Apply Any Manifest

```bash
# Apply a single file
kubectl apply -f kubernetes_manifest_files/Deployment/nginx-deployment-service.yaml

# Apply an entire folder
kubectl apply -f kubernetes_manifest_files/HPA/

# Apply everything
kubectl apply -f kubernetes_manifest_files/
```

### Verify Resources

```bash
kubectl get pods,svc,deployments -A
kubectl get pv,pvc
kubectl get hpa
kubectl get ingress
kubectl get configmap,secret
kubectl get statefulsets,daemonsets
```

---

## 🔍 Quick Command Reference

```bash
# Deployments
kubectl rollout status deployment/<name>
kubectl rollout undo deployment/<name>
kubectl scale deployment <name> --replicas=5

# HPA
kubectl get hpa -w                          # Watch real-time scaling

# Ingress — install NGINX controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml

# Secrets — decode a value
kubectl get secret app-secret -o jsonpath="{.data.MYSQL_USER}" | base64 --decode

# PV / PVC
kubectl get pv,pvc
kubectl describe pvc <name>

# StatefulSet
kubectl get statefulsets
kubectl scale statefulset <name> --replicas=3
```

---

## ⚠️ Known Issues in Manifests (Learning Opportunities)

| File | Issue | Fix |
|---|---|---|
| `StatefulSet/mysql-statefulset.yaml` | Credentials hardcoded in plain text | Move to a Kubernetes `Secret` and reference via `secretKeyRef` |


---

## 🔗 Related Projects

| Repo | Description |
|---|---|
| [student-app-kubernetes](https://github.com/Sanket006/student-app-kubernetes) | Three-tier app using Deployment, StatefulSet, HPA, Secrets, PVC from this collection |
| [jenkins-cicd-pipelines](https://github.com/Sanket006/jenkins-cicd-pipelines) | Jenkins pipeline that deploys apps using these manifest patterns |
| [terraform-aws-iac](https://github.com/Sanket006/terraform-aws-iac) | Provisions the EKS cluster where these manifests are applied |

---

## 👨‍💻 Author

**Sanket Ajay Chopade** — DevOps Engineer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/sanketchopade07)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/Sanket006)
