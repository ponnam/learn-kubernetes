
# Day 05 - Kubernetes Namespace


### Task 5.1: View the default namespaces comes with the cluster

```bash
kubectl get ns
```

### Task 5.2: View Resources in kube-system Namespace

```bash
kubectl get all -n kube-system
```

or

```bash
kubectl get all --namespace=kube-system
```

### Task 5.3: Create Namespace `demo` using Declarative Method
#### Step 5.3.1: Create nsdemo.yml
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: demo
```

#### Step 5.3.2: Apply NameSpace
```bash
kubectl apply -f nsdemo.yml
```

#### Step 5.3.3: Verify Namespace

```bash
kubectl get ns
```

### Task 5.4: Create another name space `demo2` using Imperative method

```bash
kubectl create ns demo2
```

### Task 5.5: Create Deployment in `demo` Namespace
```bash
kubectl create deploy nginx-demo --image=nginx -n demo
```

### Task 5.6: Verify resources in `demo` Namespace
```bash
kubectl get deploy -n demo
kubectl get pods -n demo
```

### Task 5.7: Create Deployment in default Namespace

```bash
kubectl create deploy nginx-test --image=nginx -n default
```

### Task 5.8: Verify Cross-Namespace Pod Communication
#### Step 5.8.1: Get Pod Names and IPs
```bash
kubectl get pods -o wide -n demo
kubectl get pods -o wide
```

#### Step 5.8.2: Exec into demo Pod

```bash
kubectl exec -it <demo-pod-name> -n demo -- sh
```

#### step 5.8.3: Access Pod in default Namespace using IP
```bash
curl http://<default-pod-ip>
```

#### Step 5.8.4: Verify Reverse Communication

**Exec into default namespace Pod:**
```bash
kubectl exec -it <default-pod-name> -- sh
```
**Access demo Pod:**
```bash
curl http://<demo-pod-ip>
```

**Important Observation** : Pods across namespaces can communicate using Pod IP addresses.

---
### Task 5.9: Scale Deployments
```bash
kubectl scale --replicas=3 deploy/nginx-demo -n demo

kubectl scale --replicas=3 deploy/nginx-test
```

### Task 5.10: Verify Scaled Pods

```bash
kubectl get pods -n demo
kubectl get pods
```

### Task 5.11: Expose Deployments using ClusterIP Service

#### Step 5.11.1: Expose demo Deployment
```bash
kubectl expose deploy/nginx-demo \
--name=svc-demo \
--port=80 \
-n demo
```

#### Step 5.11.2: Expose default Deployment
```bash
kubectl expose deploy/nginx-test \
--name=svc-test \
--port=80
```

#### Step 5.11.3: Verify Services

```bash
kubectl get svc -n demo
kubectl get svc
```

### Task 5.12: Test Service Communication Across Namespaces

#### Step 5.12.1: Exec into demo Pod

```bash
kubectl exec -it <demo-pod-name> -n demo -- sh
```

#### Step 5.12.2: Try Accessing Service (Defautl Namespace) using Short Name

```bash
curl svc-test
```
**👉 This may fail because the Service exists in another namespace.**

#### Step 5.12.3: Access Service using FQDN

```bash
curl svc-test.default.svc.cluster.local
```

### TAsk 5.13: Access demo Service from default Namespace
#### Step 5.13.1: Exec into default namespace Pod
```bash
kubectl exec -it <default-pod-name> -- sh
```

### Step 5.13.2: Access demo Service using FQDN

```bash
curl svc-demo.demo.svc.cluster.local
```

---

## Challenge Tasks 🚀

- Create another namespace named: 
  - **production**
- Deploy nginx inside production namespace
- Create service for production deployment
- Test communication across all namespaces
- Observe:
  - Pod IP communication
  - Service DNS communication

  


