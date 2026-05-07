# Day 04 - Kubernetes Services

### Task 4.1: Create KIND Cluster with NodePort Mapping

#### Step 4.1.1: Create cluster.yml

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
# Control Plane Node
- role: control-plane
  extraPortMappings:
  - containerPort: 30001
    hostPort: 30001
    protocol: TCP

  - containerPort: 30002
    hostPort: 30002
    protocol: TCP

  - containerPort: 30003
    hostPort: 30003
    protocol: TCP

# Worker Nodes
- role: worker
- role: worker
```

#### Step 4.1.2: Create the kubernetes Cluster

```bash
kind create cluster --name mycluster1 --config cluster.yml
```

### Task 4.2: Deploy Nginx Application
#### Task 4.2.1: Create deploy.yml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
  labels:
    env: demo
spec:
  replicas: 3
  selector:
    matchLabels:
      env: demo
  template:
    metadata:
      labels:
        env: demo
    spec:
      containers:
      - name: demo
        image: nginx
```

#### Step 4.2.2: Create the Deployment

```bash
kubectl apply -f deploy.yml
```
### Task 4.3: Create nodePort Service

#### Step 4.3.1: Create svcnodeport.yml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nodeport-svc
  labels:
    env: demo
spec:
  type: NodePort
  ports:
  - nodePort: 30001
    port: 80
    targetPort: 80
  selector:
    env: demo
```

#### Step 4.3.2: Apply the Servcie

```bash
kubectl apply -f svcnodeport.yml
```

#### Step 4.3.3: Verfiy Services

```bash
kubectl get deploy
kubectl get svc
kubectl get endpoints
kubectl get pods --show-labels
```

#### Step 4.3.4: Access the Application
```bash
http://Public-IP:30001
```

### Task 4.4: Create ClusterIP Service

#### Step 4.4.1: Create svcclusterip.yml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: clusterip-svc
  labels:
    env: demo
spec:
  type: ClusterIP
  ports:
  - port: 80
    targetPort: 80
  selector:
    env: demo
```

#### Step 4.4.2: Apply Service

```bash
kubectl apply -f svcclusterip.yml
```

#### Step 4.4.3: Verify

```bash
kubectl get svc
kubectl describe svc clusterip-svc
```

#### Step 4.4.4: Voew Pod IPs

```bash
kubectl get pods -o wide
```

### Task 4.5: Create ExternalName Service
#### Step 4.5.1: Create svcexternalname.yml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-external-service
spec:
  type: ExternalName
  externalName: example.com
```

#### Step 4.5.2: Apply Service

```bash
kubectl apply -f svcexternalname.yml
```

#### Step 4.5.3: Verify
```bash
kubectl get svc
kubectl describe svc my-external-service
```

---

### Challenge Tasks 🚀

- Scale deployment to 5 Pods
- Observe endpoints update automatically
- Delete a Pod and verify Service still works
- Create another NodePort Service on different port
