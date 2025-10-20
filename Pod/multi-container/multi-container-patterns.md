# Multi-Container Pod Patterns for CKAD

## What are Multi-Container Pods?

Pods that run multiple containers sharing the same network and storage. All containers in a pod:

- Share the same IP address and port space
- Share mounted volumes
- Are scheduled together on the same node
- Start and stop together

## Three Key CKAD Patterns

### 1. Co-located Pattern

**Purpose**: Multiple containers that work together as a single unit
**Characteristics**:

- Containers run simultaneously
- Share network namespace (use localhost)
- Share storage volumes
- Complement each other's functionality
  **Examples**:
- Web server + reverse proxy
- Application + monitoring agent
- Database + backup utility

### 2. Init Container Pattern

**Purpose**: Setup containers that run before main containers
**Characteristics**:

- Run to completion before main containers start
- Can have multiple init containers (run sequentially)
- Share volumes with main containers
- Must succeed for pod to start
  **Examples**:
- Database migration
- Configuration setup
- Dependency checking
- File/data preparation

### 3. Sidecar Pattern

**Purpose**: Helper container that extends main application functionality
**Characteristics**:

- Runs alongside main container
- Provides supporting functionality
- Shares resources with main container
- Enhances or monitors main application
  **Examples**:
- Log collection and forwarding
- Metrics collection
- Configuration sync
- Security scanning

## Container Communication

### Shared Network

- All containers use `localhost` to communicate
- Containers must use different ports
- Network policies apply to entire pod

### Shared Storage

- Volumes mounted to multiple containers
- Common patterns: shared logs, configuration, data
- Use `emptyDir` for temporary shared storage
- Use persistent volumes for permanent shared data

### Shared Process Namespace (Optional)

- Set `shareProcessNamespace: true`
- Containers can see each other's processes
- Useful for debugging and monitoring

## CKAD Exam Tips

### Key Commands

```bash
# View all containers in pod
kubectl get pod <pod-name> -o jsonpath='{.spec.containers[*].name}'

# Logs from specific container
kubectl logs <pod-name> -c <container-name>

# Execute into specific container
kubectl exec -it <pod-name> -c <container-name> -- /bin/bash

# Describe pod (shows all containers)
kubectl describe pod <pod-name>
```

### Common Scenarios

- **Sidecar logging**: App writes to shared volume, sidecar reads and forwards
- **Init containers**: Run before main containers start
- **Ambassador proxy**: Handle database connections or external APIs
- **Adapter transformation**: Convert logs or metrics to standard formats

### Best Practices

- Keep containers single-purpose
- Use init containers for setup tasks
- Share data via volumes, not container dependencies
- Consider container startup order
- Monitor resource usage of all containers
- Use appropriate restart policies

### Troubleshooting

- Check which container is failing: `kubectl describe pod`
- View logs from all containers: `kubectl logs <pod> --all-containers`
- Exec into specific container for debugging
- Verify shared volumes are mounted correctly
- Check port conflicts between containers
