# ☸️ Kubernetes Manifest Files

<div align="center">

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![YAML](https://img.shields.io/badge/YAML-CB171E?style=for-the-badge&logo=yaml&logoColor=white)
![AWS EBS](https://img.shields.io/badge/AWS_EBS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)

*A structured collection of production-ready Kubernetes manifests — covering all 15 core resource types from Pod to StatefulSet, HPA, Ingress, Secrets, and dynamic/static PV provisioning.*

</div>

---

## 📌 Overview

This repository contains **ready-to-apply Kubernetes YAML manifests** organized by resource type. Each folder includes standalone manifests, combined multi-resource configurations, and detailed README documents. It serves as a comprehensive reference guide for learning Kubernetes hands-on and deploying resources in real-world environments.

---

## 📁 Repository Structure

All manifests are organized under the [kubernetes_manifest_files/](./kubernetes_manifest_files/) directory:

```
kubernetes_manifest_files/
├── Pod/                          # Basic Pod and NodePort Service configurations
├── ReplicationController/        # Legacy ReplicationController (comparison purposes)
├── ReplicaSet/                   # ReplicaSet and NodePort Service setup
├── Deployment/                   # Deployments, rolling updates, and services
├── DaemonSet/                    # DaemonSet and node-level service exposure
├── StatefulSet/                  # MySQL (with PVC) and Nginx StatefulSet templates
├── Service/                      # ClusterIP, NodePort, and LoadBalancer examples
├── Ingress/                      # Ingress routing manifests and controller setup guide
├── API Gateway/                  # Gateway API classes, gateways, and HTTP route rules
├── HPA/                          # CPU, memory, and combined Horizontal Pod Autoscalers
├── ConfigMap/                    # ConfigMaps and key injection techniques
├── Secret/                       # Opaque Secrets and environment variables injection
├── Namespace/                    # Namespaces and resource-scoped workload deployment
├── PersistentVolume/             # HostPath PV, PVC, and Deployment bindings
├── Static Provisioning PV/       # AWS EBS manual PV and PVC attachments
└── Dynamic Provisioning PV/      # StorageClass setups with auto-provisioned volumes
```

---

## 📦 What's Inside Each Folder

| Resource Directory | Key Manifests | Description |
| :--- | :--- | :--- |
| 📦 **[Pod](./kubernetes_manifest_files/Pod/)** | `pod.yaml`, `nginx-pod-service.yaml` | Basic Pod structure and NodePort Service selection. |
| 🔄 **[ReplicationController](./kubernetes_manifest_files/ReplicationController/)** | `replicationcontroller.yaml`, `nginx-rc-service.yaml` | Legacy controller for historical and learning comparison. |
| 🛡️ **[ReplicaSet](./kubernetes_manifest_files/ReplicaSet/)** | `replicaset.yaml`, `nginx-replicaset-service.yaml` | Modern ReplicaSet with set-based selectors. |
| 🚀 **[Deployment](./kubernetes_manifest_files/Deployment/)** | `deployment.yaml`, `nginx-deployment-service.yaml` | Deployment rolling update strategies and standard workloads. |
| ⚙️ **[DaemonSet](./kubernetes_manifest_files/DaemonSet/)** | `daemonset.yaml`, `nginx-daemonset-service.yaml` | Node-level daemon configuration (logging, monitoring). |
| 💾 **[StatefulSet](./kubernetes_manifest_files/StatefulSet/)** | `mysql-statefulset.yaml`, `mysql-statefulset-persistentvolume.yaml` | Headless Service integrations and volumeClaimTemplates. |
| 🔌 **[Service](./kubernetes_manifest_files/Service/)** | `service.yaml` | Service communication skeletons (ClusterIP, NodePort). |
| 🌐 **[Ingress](./kubernetes_manifest_files/Ingress/)** | `ingress-host-based-routing.yaml`, `ingress-setup.md` | Host-based and path-based routing via Nginx Ingress. |
| 🛣️ **[API Gateway](./kubernetes_manifest_files/API%20Gateway/)** | `gateway-class.yaml`, `gateway.yaml`, `httproute.yaml` | Standard Gateway API classes, gateways, and HTTPRoute setups. |
| 📈 **[HPA](./kubernetes_manifest_files/HPA/)** | `hpa-cpu-based.yaml`, `hpa-cpu+memory.yaml` | Autoscaling configurations using the `autoscaling/v2` API. |
| 📝 **[ConfigMap](./kubernetes_manifest_files/ConfigMap/)** | `app-configmap.yaml`, `configmap-pod.yaml` | Environment variable and volume file configurations. |
| 🔑 **[Secret](./kubernetes_manifest_files/Secret/)** | `secret.yaml`, `secret-pod.yaml` | Securely loading base64-encoded credentials. |
| 🗃️ **[Namespace](./kubernetes_manifest_files/Namespace/)** | `namespace.yaml`, `namespace-pod.yaml` | Isolated environments and resource scoping. |
| 💽 **[PersistentVolume](./kubernetes_manifest_files/PersistentVolume/)** | `pv.yaml`, `pvc.yaml`, `pv-pvc-pod.yaml` | Local and static hostPath volumes. |
| ☁️ **[Static Provisioning PV](./kubernetes_manifest_files/Static%20Provisioning%20PV/)** | `AWS EBS + PV + PVC Manifest File.yaml` | Manually attached AWS EBS volumes. |
| ⚡ **[Dynamic Provisioning PV](./kubernetes_manifest_files/Dynamic%20Provisioning%20PV/)** | `StorageClass + PVC + Pod.yaml`, `Full-manifest-file.yaml` | Dynamic provisioner (`ebs.csi.aws.com`) StorageClass definitions. |

---

## 🎯 Recommended Learning Path

For developers and DevOps engineers learning Kubernetes, it is recommended to explore the resources in the following order:

```
1. Pod                         ← Smallest deployable unit
2. ReplicationController       ← Legacy controller (comparison)
3. ReplicaSet                  ← Modern selector-based ReplicaSet
4. Deployment                  ← The production standard for stateless apps
5. Service                     ← Internal/External routing basics
6. Ingress                     ← Layer 7 HTTP routing & path rules
7. API Gateway                 ← Gateway API (modern Layer 7 routing)
8. Namespace                   ← Logical resource partitioning
9. ConfigMap                   ← Non-sensitive configuration data
10. Secret                      ← Sensitive credential management
11. DaemonSet                  ← Node-level services (logs/agents)
12. HPA                        ← Scaling workloads automatically
13. PersistentVolume + PVC     ← Basic storage and local volumes
14. StatefulSet                ← Databases and persistent network IDs
15. Static Provisioning PV     ← AWS EBS static volumes
16. Dynamic Provisioning PV    ← StorageClass-driven storage
```

---

## 🚀 Getting Started

### 1. Prerequisites
Before applying these manifests, ensure you have:
* A running Kubernetes cluster (e.g., **Minikube**, **Kind**, **Docker Desktop**, **k3s**, or **EKS**).
* The `kubectl` command-line tool installed and configured to point to your cluster.

### 2. Verify Your Connection
Check that your CLI communicates with the active cluster:
```bash
# Display cluster info
kubectl cluster-info

# Check active nodes
kubectl get nodes
```

---

## ⚙️ Installation & Usage Guide

### Deploying a Single Manifest
To deploy an individual resource, use the `apply` command:
```bash
# Example: Deploy a single Pod
kubectl apply -f kubernetes_manifest_files/Pod/pod.yaml
```

### Deploying an Entire Folder
To deploy all manifests related to a specific resource type:
```bash
# Example: Deploy HPA resources
kubectl apply -f kubernetes_manifest_files/HPA/
```

### Deploying Everything
To apply all configurations across all folders:
```bash
kubectl apply -f kubernetes_manifest_files/
```

### Verification & Debugging
Run the following common commands to check the state of deployed resources:
```bash
# List all pods, services, and deployments in the active namespace
kubectl get pods,svc,deploy -o wide

# Describe a specific resource for details and events
kubectl describe pod <pod-name>

# View logs for a running container
kubectl logs <pod-name>

# Check storage resources
kubectl get pv,pvc,storageclass
```

---

## 🛡️ Production Best Practices

When moving from these basic learning manifests to production environments, adopt the following guidelines:

1. **Never Hardcode Credentials:** Use Kubernetes `Secrets` to store sensitive data and reference them via `secretKeyRef` or volume mounts.
2. **Define Resource Limits:** Always set `resources.requests` and `resources.limits` for CPU and Memory in container specs to prevent resource starvation.
3. **Pin Container Image Tags:** Avoid using `:latest` or tagless image references. Pin to explicit minor or patch versions (e.g., `nginx:1.25.3-alpine`) for reproducible deployments.
4. **Use apps/v1 for Workloads:** Standardize on `apps/v1` for Deployments, StatefulSets, and DaemonSets. Do not use deprecated beta API versions.

---

## 🤝 Contribution Guidelines

Contributions are welcome! If you want to add new manifests or improve the existing documentation, please follow these steps:

1. **Fork** the repository and create your feature branch:
   ```bash
   git checkout -b feature/new-manifest
   ```
2. **Lint & Verify Your YAML:** Ensure there are no syntax errors or incorrect indentations. You can use tools like `yamllint` or run a local dry-run:
   ```bash
   kubectl apply -f <your-file>.yaml --dry-run=client
   ```
3. **Maintain Structure:** Follow the formatting structure used in existing folders. Include a brief `README.md` inside your new resource directory explaining the configuration.
4. **Submit a Pull Request** describing your changes and the resources added.

---

## 🔗 Related Projects

Check out these related projects to see these manifests in action:
* 🌐 [student-app-kubernetes](https://github.com/Sanket006/student-app-kubernetes) - A three-tier application utilizing Deployments, StatefulSets, HPAs, and PVCs.
* 🤖 [jenkins-cicd-pipelines](https://github.com/Sanket006/jenkins-cicd-pipelines) - Jenkins pipeline scripts that deploy workloads using these manifest patterns.
* 🏗️ [terraform-aws-iac](https://github.com/Sanket006/terraform-aws-iac) - Infrastructure as Code to provision EKS clusters where these manifests can be run.

---

## 👨‍💻 Author

**Sanket Ajay Chopade** — DevOps Engineer
* 💼 [LinkedIn](https://www.linkedin.com/in/sanketchopade07)
* 🐙 [GitHub](https://github.com/Sanket006)
