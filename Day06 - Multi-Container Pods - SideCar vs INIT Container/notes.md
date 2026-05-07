# Day 06 - Multi-Container Pods, Sidecar & Init Containers


## Multi-Container Pods

A Kubernetes Pod can run multiple containers together.

These containers:
- Share the same network
- Share the same IP address
- Share storage volumes
- Work together as a single unit


### Shared Resources inside a Pod

Containers inside the same Pod share:

| Resource | Description |
|----------|-------------|
| Network  | Same IP & Port space |
| Storage  | Shared Volumes |
| Lifecycle | Managed together |

---

## Why Multi-Container Pods?

Sometimes applications need supporting containers for:
- Logging
- Monitoring
- Proxy
- Data synchronization
- Initialization tasks

Instead of running separately, Kubernetes allows them to run together in the same Pod.



## Types of Multi-Container Patterns

1. Sidecar Container
2. Init Container


## Sidecar Container

A Sidecar container runs alongside the main application container inside the same Pod.

It supports or enhances the main application.


### Key Characteristics of Sidecar Container

- Runs parallel with the main container
- Starts together with main container
- Stops together with main container
- Shares network and volumes
- Usually always running


### Common Use Cases of Sidecar

| Use Case | Description |
|----------|-------------|
| Logging | Collect logs from application |
| Monitoring | Prometheus exporter |
| Proxy | Envoy / Istio sidecar |
| Data Sync | Sync shared files |



### Example Architecture

```text
Main App Container
        +
Logging Sidecar
```

### SideCar Communication
Since containers share the same network namespace:
```bash
localhost
```
can be used for communication between containers.   

**Example:**
- App container connects to DB sidecar using:
```bash
localhost:3306
```

## Init Container

An Init Container runs BEFORE the main application container starts.

**Used for:**

Setup tasks  
Dependency checks  
Initialization  

### Key Characteristics of Init Container
- Runs sequentially
- Runs only once
- Must complete successfully
- Main container starts only after Init container succeeds

#### Important Behavior
If Init container fails:   

❌ Main application container will NOT start

### Common Use Cases of Init Containers

| Use Case            | Description             |
| ------------------- | ----------------------- |
| Wait for DB         | Ensure DB is ready      |
| Download Config     | Fetch configuration     |
| Data Initialization | Preload data            |
| Dependency Check    | Verify API availability |

### How Init Container Works
1. Init container starts
2. Performs setup tasks
3. Completes successfully
4. Main container starts

## Real-World Microservices Example

| Container       | Role             |
| --------------- | ---------------- |
| App Container   | Main application |
| Log Collector   | Sidecar          |
| DB Check Script | Init Container   |


## Init Container vs Sidecar

| Feature                    | Init Container | Sidecar Container |
| -------------------------- | -------------- | ----------------- |
| Runs Before Main App       | ✅              | ❌                 |
| Runs Continuously          | ❌              | ✅                 |
| Used for Setup             | ✅              | ❌                 |
| Used for Support Services  | ❌              | ✅                 |
| Must Complete Successfully | ✅              | ❌                 |


## Important Kubernetes Concepts
### Shared Networking
Containers inside same Pod communicate using:
```bash
localhost
```
because they share the same network namespace.

### Shared Storage

Containers can share data using Volumes.

**Example:**

- Sidecar writes logs
- Main app reads logs  

## Summary
- Multi-container Pods allow containers to work together
- Sidecar containers support the main application
- Init containers perform setup tasks before app startup
- Containers inside a Pod share:
  - Network
  - Storage
  - Lifecycle

## Key Takeaway

👉 Init Containers prepare the environment  
👉 Sidecar Containers support the running application  
