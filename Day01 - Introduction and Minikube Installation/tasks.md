# Day 01 - Setting up Minikube Kubernetes Cluster

### Task 1: Setup Kubernetes using Minikube on AWS EC2

#### Step 1.1: Create EC2 Instance
- OS: Ubuntu
- Instance Type: t3.medium or higher



#### Step 1.2: Update System

```bash
sudo apt-get update -y
sudo apt-get upgrade -y
```
#### Step 1.3: Install Required Packages
```bash
sudo apt install curl wget apt-transport-https -y
```
#### Step 1.4: Install Docker
```bash
curl -fsSL https://get.docker.com -o install-docker.sh
chmod +x install-docker.sh
./install-docker.sh
```
#### Step 1.5: Install Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```

#### Step 1.6: Install kubectl
```bash
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
```

#### Step 1.7: Download Checksum
```bash
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256
```

#### Step 1.8: Validate kubectl
```bash
echo "$(cat kubectl.sha256) kubectl" | sha256sum --check
```
#### Step 1.9: Install kubectl
```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
---

### Task 2: Verify Installation
```bash
kubectl version --client
kubectl version --client --output=yaml
```

### Task 3: Start Minikube Cluster
```bash
minikube start --driver=docker --force
```

### Task 4: Set Default Driver
```bash
minikube config set driver docker
```

### Task 5: Check Cluster Status
```bash
minikube status
```
---

### Task 6: Interact with Cluster

#### Task 6.1: List all pods
```bash
kubectl get po -A
```

#### Task 6.2: Using Minikube kubectl
```bash
minikube kubectl -- get po -A
```

**Note:**  
**kubectl** → uses host-installed CLI  
**minikube kubectl** → uses internal kubectl

### Task 7: Explore Kubernetes API

```bash
kubectl api-resources
```