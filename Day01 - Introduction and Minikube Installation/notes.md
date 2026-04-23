# Day 01 - Introduction to Kubernetes & minikube Installation

## What is Kubernetes?

Kubernetes is an open-source container orchestration platform used to automate the deployment, scaling, and management of containerized applications.

It is hosted by the Cloud Native Computing Foundation (CNCF).

**Kubernetes is:** 
- NOT a container tool
- A container orchestration tool

👉 In simple terms:
Kubernetes helps you run, manage, and scale containers automatically.

---

### Why Kubernetes?

**Docker works well for:**
- Single container applications
- Small-scale environments

**But Docker alone cannot:**
- Handle high traffic
- Scale automatically
- Manage failures efficiently

👉 Kubernetes solves these problems.



### Core Responsibilities of Kubernetes

1. **Container Scheduling**
   - Decides which container runs on which node

2. **Self-Healing**
   - Restarts failed containers
   - Replaces failed nodes

3. **Auto Scaling**
   - Scales containers based on load

4. **Load Balancing**
   - Distributes traffic across containers

5. **Rolling Updates**
   - Updates applications without downtime

---

### Kubernetes Architecture

A Kubernetes cluster consists of:

- **Control Plane (Master)**
- **Worker Nodes**

👉 Control Plane → Manages cluster  
👉 Worker Nodes → Run applications  



### Control Plane Components

#### kube-apiserver
- Entry point of Kubernetes
- Accepts all requests (kubectl, REST API)
- Updates cluster state in etcd



#### etcd
- Distributed key-value store
- Stores cluster data and configuration


#### kube-scheduler
- Assigns pods to nodes
- Considers CPU, memory, and constraints



#### kube-controller-manager
- Maintains desired state

**Includes:**
- Node Controller
- ReplicaSet Controller
- Job Controller



#### cloud-controller-manager (Optional)
Used in cloud environments like AWS, Azure, GCP.

- Manages cloud resources
- Handles load balancers
- Configures networking


### Worker Node Components

#### kubelet
- Agent running on each node
- Communicates with API server
- Ensures containers are running properly



#### kube-proxy
- Handles networking
- Routes traffic to pods
- Provides load balancing


#### Container Runtime
- Engine that runs containers

Examples:
- containerd
- CRI-O

---

### How Kubernetes Works (Flow)

1. User runs:
   kubectl apply -f deployment.yaml

2. Request goes to API Server

3. API Server stores data in etcd

4. Scheduler assigns pod to a node

5. kubelet executes instructions

6. Container runtime runs container

7. kube-proxy enables networking

---

### Simple Analogy

| Component        | Role                     |
|-----------------|--------------------------|
| API Server      | CEO                      |
| etcd            | Database                 |
| Scheduler       | HR                       |
| Controller      | Manager                  |
| Worker Nodes    | Employees                |
| kubelet         | Supervisor               |

---

## Kubernetes Installation Options

### Self-Managed

- Minikube → Single node (Best for beginners)
- KIND → Multi-node in Docker
- kubeadm → Production-grade setup
- KOPS
- Kubespray


### Managed Cloud Services

- AWS EKS
- Azure AKS
- GCP GKE
- IBM IKS

---

## Minikube Overview

Minikube runs a single-node Kubernetes cluster locally.

### Prerequisites:
- 2 CPUs
- 2 GB RAM
- 20 GB Disk
- Docker / VirtualBox / Podman

---

## Summary

- Kubernetes is a container orchestration tool
- It automates deployment, scaling, and management
- Works on cluster architecture
- Core components: API Server, Scheduler, etcd, kubelet
- Minikube is best for beginners