# Kubernetes Pod

This folder contains **Kubernetes Pod manifest examples**. A **Pod** is the smallest and simplest deployable unit in Kubernetes and represents one or more containers running together with shared networking and storage.

Pods are mainly used for **learning, testing, and debugging**. In production, Pods are usually managed by higher-level controllers like Deployments or StatefulSets.

---

## 📘 What is a Pod?

A Pod:

* Runs one or more containers
* Shares the same IP address and port space
* Can share storage volumes
* Is ephemeral (can be recreated at any time)

---

## 📁 Folder Contents

```
Pod/
├── pod.yaml       
├── nginx-pod-service.yaml  
└── README.md
```

---

## 🧩 Basic Pod YAML Explanation

Below is a simplified explanation of common fields used in a Pod manifest:

* **apiVersion**: Kubernetes API version (usually `v1`)
* **kind**: Resource type (`Pod`)
* **metadata**: Name and labels of the Pod
* **spec**: Desired state of the Pod

  * **containers**: List of containers
  * **image**: Container image to run
  * **ports**: Ports exposed by the container

---

## ▶️ How to Apply a Pod

Apply the Pod manifest:

```bash
kubectl apply -f pod.yaml
```

Check Pod status:

```bash
kubectl get pods
kubectl describe pod <pod-name>
```

View container logs:

```bash
kubectl logs <pod-name>
```

---

## 🧪 Common Pod Use Cases

* Learning Kubernetes fundamentals
* Testing container images
* Debugging networking or storage issues
* Running short-lived tasks

---

## ⚠️ Important Notes

* Pods are **not self-healing** by default
* If a Pod dies, it will not restart unless managed by a controller
* For production workloads, prefer **Deployments** or **StatefulSets**

---

## 📌 Best Practices

* Use Pods only for simple or experimental workloads
* Add resource limits (`resources.requests` and `resources.limits`)
* Use labels consistently

---

## 📓 Learning Progression

After mastering Pods, move to:

➡️ **Deployment** → **Service** → **Ingress** → **HPA** → **PV/PVC**

---

## 📬 Support

For questions or improvements, feel free to open an issue or submit a pull request.

Happy Kubernetes Learning! 🚀
