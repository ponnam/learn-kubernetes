# Day 04 - Kubernetes Services



## SERVICE

A Kubernetes Service is an abstraction that provides a stable way to access a group of Pods.

In Kubernetes, Pods are temporary and their IP addresses change frequently. A Service solves this problem by providing:
- Stable IP Address
- Stable DNS Name
- Load Balancing across Pods



### Problems with Pods

Pods are:

- Ephemeral (Created & destroyed dynamically)
- Assigned dynamic IP addresses
- Not reliable for direct communication

👉 Because of this, applications should NOT directly communicate with Pod IPs.

---

## Why Kubernetes Service?

A Service provides:

- Stable networking
- Internal communication
- External access
- Load balancing



## Types of Kubernetes Services

1. ClusterIP (Default)
2. NodePort
3. LoadBalancer
4. ExternalName


### Service Ports

| Field       | Description |
|-------------|-------------|
| port        | Service Port |
| targetPort  | Container Port |
| nodePort    | External Port |

---

## NodePort Service

NodePort is a Kubernetes Service type that exposes your application outside the cluster using a port on each node.

It allows external users to access the application using:

``` id="hnkk6x"
<NodeIP>:<NodePort>
```

### NodePort Range
Default NodePort range:
```bash
30000 - 32767
```

## How NodePort Works

1. Create Service with:
```yaml
type: NodePort
```

2. Kubernetes:

- Assigns ClusterIP internally
- Opens NodePort on every node

3. External traffic reaches:
```bash
NodeIP + NodePort
```
4. Traffic is forwarded to Pods

### Key Features of NodePort

- External application access
- Same port opened on all nodes
- Built on top of ClusterIP
- Easy to use
- Good for testing and demos

### Important Note for KIND
If you are using KIND cluster:   

👉 You must expose ports while creating the cluster using extraPortMappings.   

Otherwise NodePort may not be accessible externally.   

---

## ClusterIP Service

ClusterIP is the default Service type in Kubernetes.

It exposes the application only inside the Kubernetes cluster.

### Key Features of ClusterIP
- Internal communication only
- Automatically assigned virtual IP
- Stable DNS name
- Load balancing across Pods

### Why ClusterIP?
Pods are ephemeral and receive dynamic IP addresses.

ClusterIP solves this by:

- Providing stable access
- Acting as an internal load balancer

### How ClusterIP Works?
1. Create Service
2. Kubernetes assigns ClusterIP
3. Service maps to Pods using labels
4. Traffic is distributed to Pods

---


## LoadBalancer Service

LoadBalancer is a Kubernetes Service type used to expose applications to the internet using a cloud provider’s load balancer.

### Key Features
- Public External IP
- Automatic load balancing
- Production-ready
- Cloud integrated

Examples:  
- AWS ELB
- Azure Load Balancer
- GCP Load Balancer

### How LoadBalancer Works

1. Create Service with:
```yaml
type: LoadBalancer
```
2. Kubernetes:
- Creates ClusterIP
- Creates NodePort internally
- Requests cloud provider to create Load Balancer
3. Cloud provider assigns Public IP
4. External traffic flow:

```bash
Internet → LoadBalancer → Node → Pods
```
### Important Note:

LoadBalancer works only in cloud environments:

AWS  
Azure  
GCP

👉 In Minikube or KIND, no real external load balancer is created.

---

## ExternalName Service

ExternalName is a Kubernetes Service type that maps a service to an external DNS name.

It works only at the DNS level.

### Why ExternalName?
Used when applications inside Kubernetes need access to:

- External Databases
- Third-party APIs
- Legacy Systems

Instead of hardcoding URLs, ExternalName provides clean abstraction.  

### How ExternalName Works?

1. Create ExternalName Service
2. Kubernetes DNS creates CNAME mapping
3. Pods use Service name
4. DNS resolves to external domain

### Important Note

ExternalName:

- Does NOT create ClusterIP
- Does NOT create Proxy
- Only performs DNS mapping

## Kubernetes Service Flow
```bash
User → Service → Pods
```