
# Day 06 - Multi-Container Pods - SideCar vs INIT Car


### Task 6.1: Create Pod with Init Container

#### Step 6.1.1: Create initpod.yml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp
  labels:
    name: myapp-pod
spec:
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh' , '-c']
    args: ['echo "Init Container running..."; sleep 30; echo "Init Completed"']
  containers:
  - name: myapp-pod
    image: nginx
```
#### Step 6.1.2: Create Pod
```bash
kubectl create -f initpod.yml
```

### Task 6.2: Observe Pod Status

```bash
kubectl get pods -w
```

👉 **Observe:**

- Init container runs first  
- Main container starts after completion

### Task 6.3: Check Init Container Logs

```bash
kubectl logs myapp -c init-myservice
```

### Task 6.4: Create Init Container Dependency Check
**Create another pod with Init car to observe until the initcar completes the tasks, main car won’t start**

#### Step 6.4.1: Create initpod2.yml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mytestapp
  labels:
    app: mytestapp

spec:
  initContainers:
  - name: wait-for-db-service
    image: busybox
    command: ['sh', '-c']
    args:
      - >
        until nslookup mydb-svc.default.svc.cluster.local;
        do
          echo "DB Service not found, Retrying...";
          sleep 5;
        done;
        echo "DB Service found!"

  containers:
  - name: myapp
    image: nginx
```

#### Step 6.4.2: Create Pod

```bash
kubectl apply -f initpod2.yml
```

#### Step 6.4.3: Observe Pod Status

```bash
kubectl get pods
```

👉 **Observe:**

- Pod stays in Init state
- Main container does not start

#### Step 6.4.4: Check Init Logs

```bash
kubectl logs mytestapp -c wait-for-db-service
```

### Task 6.5: Create Service Required by Init Container
#### Step 6.5.1: Create mydb-svc.yml
```yaml
apiVersion: v1
kind: Service
metadata:
  name: mydb-svc

spec:
  selector:
    app: mytestapp

  ports:
  - port: 80
    targetPort: 80
```

#### Step 6.5.2: Create Service
```bash
kubectl create -f mydb-svc.yml
```

#### Step 6.5.3: Verify Pod Status
```bash
kubectl get pods
```

👉 **Observe:**

- Init container completes
- Main container starts successfully  


### Task 6.6: Create Multi-Container Pod (Sidecar Pattern)
#### Step 6.6.1: Create sidecar.yml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-mysql-pod

spec:
  containers:

  # Main Application Container
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80

    command: ['sh', '-c']
    args:
      - |
        echo "Starting Nginx...";
        echo "Trying to connect to MySQL...";
        sleep 15;
        nc -zv localhost 3306;
        echo "Connection attempt done";
        nginx -g 'daemon off;'

  # Sidecar Database Container
  - name: mysql
    image: mysql:5.7

    env:
    - name: MYSQL_ROOT_PASSWORD
      value: root

    ports:
    - containerPort: 3306
```

#### Step 6.6.2: Create Pod
```bash
kubectl create -f sidecar.yml
```

#### Step 6.6.3: Verify Pod
```bash
kubectl get pods
kubectl describe pod nginx-mysql-pod
```

### Task 6.7: View Logs
#### Step 6.7.1: View nginx container logs
```bash
kubectl logs nginx-mysql-pod -c nginx
```
#### Step 6.7.2: View mysql COntainer Logs
```bash
kubectl logs nginx-mysql-pod -c mysql
```

### Task 6.8: Exec into Containers
#### Step 6.8.1: Access Nginx Container
```bash
kubectl exec -it nginx-mysql-pod -c nginx -- sh
```

#### Step 6.8.2: Access mysql Container
```bash
kubectl exec -it nginx-mysql-pod -c mysql -- sh
```