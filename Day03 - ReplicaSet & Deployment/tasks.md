# Day 03 - ReplicaSet & Deployment



### Task 3.1: Explore Replication Controller (RC)

#### Step 3.1.1: Check the Replication Controllers

```bash
kubectl explain rc
```

#### Step 3.1.2: Create rc.yml
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
  labels:
    env: demo
spec:
  replicas: 3
  selector:
    env: demo
  template:
    metadata:
      labels:
        env: demo
    spec:
      containers:
      - name: nginx
        image: nginx
```

#### Step 3.1.3: Apply rc.yml

```bash
kubectl apply -f rc.yml
```

#### Step 3.1.4: Verify 

```bash
kubectl get pods
kubectl get rc
```

#### Step 3.1.5: View the Pods and RC information

```bash
kubectl describe rc nginx-rc
kubectl get po <PODNAME>
```

---

### Task 3.2: Deploy Nginx Pod using ReplicaSet
#### Step 3.2.1: create replicaset.yml
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: demo-rs
  labels:
    env: demo
spec:
  replicas: 3
  selector:
    matchLabels:
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
      - name: nginx
        image: nginx
```

#### Step 3.2.2: Apply the replicaset.yml

```bash
kubectl apply -f replicaset.yml
```

#### Step 3.2.3: Verify the ReplicaSet & Pods

```bash
kubectl get rs
kubectl get pods
```

#### Step 3.2.4: Self Healing Test

```bash
kubectl get pods
kubectl delete pod <PODNAME>
```
👉 **New Pod will be created automatically**    

---

### Task 3.3: Scaling ReplicaSet

#### Step 3.3.1: update the live replicaset

```bash
kubectl edit rs demo-rs
```

**Update:**
```bash
replicas: 5
```

#### Step 3.3.2: Imperative way

```bash
kubectl scale --replicas=10 rs/demo-rs
kubectl get pods
```
---
### Task 3.4: Deploy Pods using Deployment

#### Step 3.4.1: Create deployment.yml
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
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
      - name: demo
        image: nginx
```

#### Step 3.4.2: Apply the deployment.yml file

```bash
kubectl apply -f deployment.yml
```

#### Step 3.4.3: Verify Deployment, ReplicaSet & Pods
```bash
kubectl get deploy
kubectl get rs
kubectl get pods
```
---

### Task 3.5: View all resources
```bash
kubectl get deploy
kubectl get rs
kubectl get pods
```
### Task 3.6: Update Deployment Image
```bash
kubectl set image deploy/nginx-deploy demo=nginx:1.9.1
```
### Task 3.7: Check Rollout
```bash
kubectl set image deploy/nginx-deploy demo=nginx:1.9.1
```

### Task 3.8: View RollOut History
```bash
kubectl rollout status deploy/nginx-deploy
```

### Task 3.9: RollBack
```bash
kubectl rollout history deploy/nginx-deploy
```

### Task 3.10: Rollback to specific Version

```bash
kubectl rollout undo deploy/nginx-deploy --to-revision=1
```
---
## Challenge Tasks:

- Delete a Pod → Observe ReplicaSet behavior
- Scale ReplicaSet to 6 Pods
- Break deployment (wrong image) → Rollback
- Observe how Deployment creates ReplicaSet
---
## Outcome
- Understand ReplicaSet behavior
- Perform scaling & self-healing
- Work with Deployment
- Perform rolling updates & rollback