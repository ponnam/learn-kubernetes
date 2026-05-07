# Day 05 - Kubernetes Namespace


## Namespace

A Namespace in Kubernetes is a way to logically divide a cluster into multiple virtual environments.

Think of a Namespace like a folder inside the cluster that groups related resources together.


### Why do we need Namespaces?

In real-world Kubernetes clusters:

- Multiple teams use the same cluster
- Multiple applications run together

**This may cause:**
- Resource conflicts
- Naming collisions
- Lack of isolation
- Difficult resource management

👉 Namespace provides logical isolation.


### Key Features of Namespace

- Logical isolation
- Same resource names allowed in different namespaces
- Resource grouping
- Access control using RBAC
- Resource limits using Quotas

---

## Default Namespaces in Kubernetes

| Namespace        | Purpose |
|------------------|----------|
| default          | Default namespace for user workloads |
| kube-system      | Kubernetes system components |
| kube-public      | Publicly accessible resources |
| kube-node-lease  | Node heartbeat information |


### Important Note

By default, all resources are created inside the:

```bash
default
```

namespace unless explicitly specified.   

---

## Why Namespace Isolation is Important?
### **Example:**

Team A:
```bash
nginx-deploy
```

Team B:
```bash
bginx-deploy
```
**Without namespaces:**

- Name conflict occurs

**With namespaces:**

- Both deployments can exist independently  



### Resource Isolation

Namespaces isolate:

- Pods
- Deployments
- Services
- ConfigMaps
- Secrets  

### Namespace and Networking
Pods across namespaces can communicate using:

- Pod IP addresses
- Service FQDN (Fully Qualified Domain Name)

### Important DNS Behavior
Inside Kubernetes:
```bash
curl http://my-service
```
**Kubernetes automatically resolves it as:**
```bash
my-service.current-namespace.svc.cluster.local
```
👉 This means Service names work only inside the same namespace by default.

### Cross-Namespace Communication
For communication across namespaces, use FQDN:
```bash
<service-name>.<namespace>.svc.cluster.local
```
**Example:**
```bash
svc-demo.demo.svc.cluster.local
```

---

### Namespace Best Practices
- Create separate namespaces for teams
- Separate dev/stage/prod environments
- Apply RBAC policies
- Use resource quotas


### Summary
- Namespace provides logical isolation
- Resources are grouped inside namespaces
- Same names can exist in different namespaces
- Services are namespace scoped
- FQDN is required for cross-namespace service communication  
