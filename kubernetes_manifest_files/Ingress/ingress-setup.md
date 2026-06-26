# ⚙️ Setting Up NGINX Ingress Controller

An Ingress resource requires a running **Ingress Controller** to intercept incoming requests and route them to your cluster services. This guide provides instructions for setting up the official **NGINX Ingress Controller**.

---

## 🚀 1. Installation

### A. General Cloud Clusters (AWS EKS, GCP GKE, Azure AKS)
Deploy the NGINX Ingress Controller using the official community manifests:
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

### B. Local Development (Minikube)
If you are running a local cluster with Minikube, enable the built-in Ingress addon:
```bash
minikube addons enable ingress
```

---

## 🔍 2. Verifying the Installation

After deploying the controller, verify that all components are running and healthy.

### A. Check the Controller Namespace
Ensure that the `ingress-nginx` namespace has been created:
```bash
kubectl get namespaces
```

### B. Check Controller Pods
Wait for the ingress controller pod to reach the `Running` state:
```bash
kubectl get pods -n ingress-nginx -w
```

### C. Check the Controller Service
Ensure the controller service has been allocated an IP address (especially for LoadBalancer services in cloud environments):
```bash
kubectl get svc -n ingress-nginx
```

### D. Verify IngressClass Availability
Make sure the `nginx` IngressClass is registered and ready:
```bash
# View list of Ingress Classes
kubectl get ingressclass

# Get details about the nginx Ingress Class
kubectl describe ingressclass nginx
```

---

## 🛠️ 3. Troubleshooting Tips

* **Pending LoadBalancer IP:** In local environments (like bare metal or standard Docker Desktop), the controller service might stay in a `Pending` state. You can resolve this on Minikube by running `minikube tunnel` in a separate terminal window.
* **Mismatched ingressClassName:** Make sure your Ingress YAML resources explicitly define `ingressClassName: nginx` in their specifications.
