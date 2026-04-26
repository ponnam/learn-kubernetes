# Day 02 - KIND Cluster Setup

---

### Task 2.1: Install KIND

#### Step 2.1.1: Update System

```bash
apt-get update -y
apt-get upgrade -y
```
#### Step 2.1.2: Install Docker & Verify
```bash
apt install -y docker.io
docker --version
```

#### Step 2.1.3: Install KIND
```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.31.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

#### Step 2.1.4: Verify KIND
```bash
kind --version
```

#### Step 2.1.5: Install kubectl
```bash
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```
---
### Task 2.2: Create KIND Cluster & Verify

```bash
kind create cluster --name cluster1
```
**Verify:**

```bash
kubectl cluster-info --context kind-cluster1
kubectl get nodes
```
---
### Task 2.3: Create Multi-Node Cluster
#### Step 2.3.1: Create config.yml

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

#### Step 2.3.2: Create Cluster

```bash
kind create cluster --name cluster2 --config config.yml
```

#### Step 2.3.3: Verify Nodes

```bash
kubectl get nodes
```
---
### Task 2.4: Work with contexts

```bash
kubectl config get-contexts
kubectl config use-context <ClusterName>
kubectl get nodes
```
---
### Task 2.5: Create Pod using Imperative way

```bash
kubectl run nginx-pod --image=nginx:latest
```
**Verfiy:**
```bash
kubectl get pods
```
---
### Task 2.6: Create Pod using Declarative way
#### Step 2.6.1: Create Pod.yml
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
#### Step 2.6.2: Apply the YAML to create Pod

```bash
kubectl apply -f pod.yml
```
**Verify:**
```bash
kubectl get po
```

#### Step 2.6.3: Describe the Pod
```bash
kubectl describe pod nginx-pod
```
---
### Task 2.7: Access the Pod
```bash
kubectl exec -it nginx-pod -- sh
```

**Exit from the Pod**
---
### Task 2.8: Generate YAML Using Imperative Command
```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml
```
---
### Task 2.9: Explore Pods

```bash
kubectl get pods --show-labels
kubectl get pods -o wide
```
---
### Task 2.10: Cleanup/Delete all Pods
```bash
kubectl delete pod <PodName>
```

