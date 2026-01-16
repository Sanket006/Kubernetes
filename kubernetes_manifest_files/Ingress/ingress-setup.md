# How to Setup Ingress in Kubernetes Cluster

### Using YAML (Direct Download from Kubernetes) :

```bash
 kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

### check status :
```bash
kubectl get pods -n ingress-nginx
```

### ✅ 1. Check Ingress Controller Namespace

Most ingress controllers run in the ingress-nginx namespace.
```bash
 kubectl get ns
```

### ✅ 2. Check Ingress Controller Pods
```bash
 kubectl get pods -n ingress-nginx
```

### ✅ 3. Check Ingress Controller Service
```bash
 kubectl get svc -n ingress-nginx
```



### ✅ 4. Check IngressClass
```bash
kubectl get ingressclass
```
You can also describe:

```bash 
kubectl describe ingressclass nginx
```
