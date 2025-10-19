# Resource Requirements, LimitRange & ResourceQuota

## Resource Requests vs Limits

### Resource Requests

- **Minimum** resources guaranteed to the container
- Used by scheduler to find suitable nodes
- Container might use more if available
- **Always specify requests** for production workloads

### Resource Limits

- **Maximum** resources container can use
- Container is killed if it exceeds memory limit
- Container is throttled if it exceeds CPU limit
- Prevents resource starvation of other containers

```yaml
resources:
  requests: # Minimum guaranteed
    memory: "128Mi"
    cpu: "100m" # 0.1 CPU core
  limits: # Maximum allowed
    memory: "256Mi"
    cpu: "200m" # 0.2 CPU core
```

## CPU Units

- **1 CPU** = 1 vCPU/Core/Hyperthread
- **100m** = 0.1 CPU (100 millicores)
- **500m** = 0.5 CPU
- **1000m** = 1 CPU

## Memory Units

- **Ki** = 1024 bytes (Kibi)
- **Mi** = 1024 Ki (Mebi)
- **Gi** = 1024 Mi (Gibi)
- **K** = 1000 bytes (Kilo)
- **M** = 1000 K (Mega)
- **G** = 1000 M (Giga)

## LimitRange

**Purpose**: Enforce resource limits at the **namespace level**

- Sets default requests/limits for pods/containers
- Enforces min/max resource constraints
- Applies to individual pods/containers

### What LimitRange Controls:

- Default CPU/memory requests and limits
- Minimum and maximum CPU/memory per container
- CPU/memory ratios
- Storage requests

### When LimitRange is Applied:

- When pods are **created** (not existing pods)
- If pod doesn't specify resources, defaults are applied
- If pod exceeds limits, creation is rejected

## ResourceQuota

**Purpose**: Limit total resource consumption at the **namespace level**

- Controls aggregate resource usage across all pods
- Prevents namespace from consuming too many cluster resources
- Can limit number of objects (pods, services, etc.)

### What ResourceQuota Controls:

- Total CPU/memory requests and limits across namespace
- Number of pods, services, secrets, etc.
- Storage requests
- Persistent volume claims

### ResourceQuota Enforcement:

- Pod creation fails if quota would be exceeded
- **Requires** pods to have resource requests when quota exists

## Key Differences

| Aspect           | LimitRange                            | ResourceQuota                  |
| ---------------- | ------------------------------------- | ------------------------------ |
| **Scope**        | Individual pods/containers            | Entire namespace               |
| **Purpose**      | Set defaults & enforce limits per pod | Control total resource usage   |
| **When Applied** | Pod creation time                     | Pod creation time              |
| **Failure**      | Pod rejected if exceeds limits        | Pod rejected if quota exceeded |

## Commands

```bash
# View resource usage
kubectl top nodes
kubectl top pods

# Check LimitRange
kubectl get limitrange
kubectl describe limitrange

# Check ResourceQuota
kubectl get resourcequota
kubectl describe resourcequota

# Check pod QoS class
kubectl get pod <pod-name> -o jsonpath='{.status.qosClass}'
```
