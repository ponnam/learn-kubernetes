# Day 03 - ReplicaSet & Deployment


## ReplicaSet

ReplicaSet is a Kubernetes controller that ensures a specified number of Pod replicas are always running.


### Key Responsibilities

- Maintains desired number of Pods
- Provides:
  - Self-Healing
  - Scaling
  - High Availability


### Desired State vs Current State

- **Desired State** → Number of Pods you WANT
- **Current State** → Number of Pods RUNNING

 ReplicaSet continuously adjusts Pods to match desired state



### Key Features

- Automatically recreates failed Pods
- Supports label selectors
- Ensures application availability


### Important Note

- ReplicaSet is mostly used internally by Deployments
- Rarely used directly in production

---

## Replication Controller (RC)

Replication Controller is the older version of ReplicaSet.



### Key Points

- Ensures a fixed number of Pods
- Uses only equality-based selectors
- Limited features compared to ReplicaSet


### Important Note

❗ Rarely used in modern Kubernetes



## ReplicaSet vs Replication Controller

| Feature              | RC        | ReplicaSet |
|---------------------|----------|------------|
| Selector Type       | Equality | Set-based  |
| Modern Usage        | ❌        | ✅          |
| Features            | Limited  | Advanced   |



### Scaling in ReplicaSet

ReplicaSet supports scaling in multiple ways:

1. Update YAML file
2. Edit live object
3. Use kubectl scale command

---

## Deployment

Deployment is a higher-level abstraction that manages ReplicaSets and Pods.



### What Deployment Does

- Manages ReplicaSets
- Ensures desired number of Pods
- Handles updates safely
- Supports rollback



## Why Deployment is Needed?

Before Deployment:
- Replication Controller → Limited features
- ReplicaSet → Better but still limited

**Missing Features:**
- Rolling updates
- Rollbacks
- Version history

👉 Deployment solves all of these



## Deployment Architecture

Deployment → ReplicaSet → Pods → Containers



### Key Features of Deployment

### 1. Rolling Updates
- Update application without downtime



#### 2. Rollback
- Revert to previous version



#### 3. Version History
- Track changes



#### 4. Scaling
- Easily increase/decrease Pods

---

### Important Fields in Deployment

#### replicas
- Number of Pods


#### selector
- Matches labels
- Must match template labels


#### template
- Defines Pod configuration

#### strategy
- Controls update behavior
- Default: RollingUpdate

---
## Updating Deployment

### Imperative Method

```bash
kubectl set image deploy/<name> <container>=<image>
```
**We can update the Image of the deployment using above command**  

### Rollout Commands

```bash
kubectl rollout status deploy/<name>
kubectl rollout history deploy/<name>
kubectl rollout undo deploy/<name>
```

### Key Takeaway

👉 In real-world usage:

- Do NOT use Pods directly  
- Do NOT use ReplicaSet directly

✅ Always use Deployment
