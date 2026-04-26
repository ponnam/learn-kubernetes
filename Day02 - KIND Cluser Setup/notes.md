# Day 02 - Kubernetes Setup (KIND) & Pods Deep Dive

## KIND (Kubernetes IN Docker)

KIND is a tool used to run Kubernetes clusters using Docker containers as nodes.

👉 Key Points:
- Runs Kubernetes inside Docker
- Ideal for local development and testing
- Supports multi-node clusters
- Lightweight and fast

---

## Why KIND?

- No need for VMs
- Easy multi-node setup
- Perfect for CI/CD pipelines
- Faster than Minikube in many cases

---

## Kubernetes Core Objects

Before going deeper, understand these building blocks:

1. Pods
2. ReplicaSet
3. Deployment
4. Service

---

## Pod (Core Concept)

A Pod is the **smallest deployable unit in Kubernetes**.

👉 Key Characteristics:
- Contains one or more containers
- Shares:
  - Network (same IP)
  - Storage (volumes)
- Runs on a single node

---

## Important Notes about Pods

- Kubernetes does NOT run containers directly
- Containers are always inside Pods
- Pods are **ephemeral** (temporary)
- If a Pod dies → it is NOT automatically recreated

---

## Pod Communication

- Containers inside a Pod → communicate via `localhost`
- Each Pod → gets a unique IP
- Pod-to-Pod communication → via cluster network

---

## Pod Creation Methods

### 1. Imperative Method

👉 You tell Kubernetes **HOW to do it**

- Uses commands
- Quick and simple
- Best for:
  - Learning
  - Debugging
  - Testing

Example:
```bash
kubectl run nginx-pod --image=nginx
```
### 2. Declarative Method

👉 You define WHAT you want

Uses YAML/JSON
Kubernetes manages execution
Best for:
Production
CI/CD
Infrastructure as Code

#### Sample Pod YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    env: dev
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
#### Understanding YAML Fields
- apiVersion → API version
- kind → Resource type (Pod)
- metadata → Name, labels
- spec → Desired state
- containers → Container details  

### Pod Lifecycle Errors

#### ErrImagePull
Kubernetes failed to pull the image immediately
#### ImagePullBackOff
Kubernetes retrying after multiple failures  

**Common Causes:**

- Wrong image name
- Private image without access
- Network issues

### Debugging Pods
**Useful Commands**

```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl get pods -o wide
```

### Accessing Pod

```bash
kubectl exec -it <pod-name> -- sh
```

### Generate YAML

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

### Kubernetes Context

Used to switch between clusters.  
```bash
kubectl config get-contexts
kubectl config use-context <context-name>
```

## Summary
- KIND is used to run Kubernetes locally using Docker
- Pod is the smallest unit in Kubernetes
- Pods are ephemeral
- Two ways to create Pods: Imperative & Declarative
- YAML is preferred for production
- Debugging is a critical skill