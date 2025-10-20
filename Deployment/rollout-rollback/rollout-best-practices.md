# Rolling Update & Rollback Best Practices - CKAD

## Rolling Update Strategy

### Default RollingUpdate Settings

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 25% # Default: 25% of desired replicas
    maxSurge: 25% # Default: 25% above desired replicas
```

### CKAD-Optimized Settings

```yaml
# For high availability (exam scenarios)
rollingUpdate:
  maxUnavailable: 1          # Keep most pods running
  maxSurge: 1               # Minimal resource usage

# For fast updates (when resources allow)
rollingUpdate:
  maxUnavailable: 0          # Zero downtime
  maxSurge: 50%             # Fast rollout
```

## Health Probe Best Practices

### Essential for Safe Rolling Updates

```yaml
readinessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 10 # Allow app startup
  periodSeconds: 5 # Frequent checks during rollout
  failureThreshold: 2 # Quick detection of issues

livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 30 # Longer delay for liveness
  periodSeconds: 30 # Less frequent checks
```

## CKAD Exam Workflow

### 1. Pre-Update Checklist

```bash
# Check current state
k get deploy <name> -o wide
k describe deploy <name>
k get pods -l app=<label>
k get svc <service-name>
```

### 2. Perform Update

```bash
# Add change cause annotation
k annotate deploy <name> deployment.kubernetes.io/change-cause="Updated to version X"

# Update image
k set image deploy/<name> container=image:tag

# Monitor rollout
k rollout status deploy/<name> --timeout=300s
```

### 3. Verification Steps

```bash
# Check rollout completion
k rollout status deploy/<name>

# Verify pods are running
k get pods -l app=<label>

# Test application
k port-forward svc/<service> 8080:80
curl localhost:8080
```

### 4. Rollback if Needed

```bash
# Quick rollback
k rollout undo deploy/<name>

# Rollback to specific revision
k rollout history deploy/<name>
k rollout undo deploy/<name> --to-revision=2
```

## Common CKAD Scenarios

### Scenario 1: Update Stuck

**Problem**: Rollout hangs at "Waiting for rollout to finish"

**Investigation**:

```bash
k describe deploy <name>           # Check conditions
k get pods -l app=<label>         # Check pod status
k describe pod <new-pod-name>     # Check pod events
k get events --sort-by=.lastTimestamp
```

**Common Causes**:

- Missing readiness probe
- Readiness probe failing
- Resource limits exceeded
- Image pull errors

### Scenario 2: Failed Update

**Problem**: New pods in CrashLoopBackOff or ImagePullBackOff

**Quick Recovery**:

```bash
k rollout undo deploy/<name>      # Rollback immediately
k rollout status deploy/<name>    # Verify rollback
```

**Investigation**:

```bash
k logs -l app=<label>            # Check application logs
k describe pod <failing-pod>      # Check pod events
```

### Scenario 3: Partial Rollout

**Problem**: Some old pods still running

**Resolution**:

```bash
# Check replica sets
k get rs -l app=<label>

# Force complete rollout
k rollout restart deploy/<name>

# Or scale down and up
k scale deploy <name> --replicas=0
k scale deploy <name> --replicas=3
```

## Change Management

### Track Changes with Annotations

```bash
# Before each update
k annotate deploy <name> deployment.kubernetes.io/change-cause="Reason for change"

# View change history
k rollout history deploy/<name>
k rollout history deploy/<name> --revision=2
```

### Environment-Specific Updates

```bash
# Development (fast rollout)
k patch deploy <name> -p '{"spec":{"strategy":{"rollingUpdate":{"maxSurge":"100%","maxUnavailable":"50%"}}}}'

# Production (safe rollout)
k patch deploy <name> -p '{"spec":{"strategy":{"rollingUpdate":{"maxSurge":"25%","maxUnavailable":"0"}}}}'
```

## Resource Considerations

### Calculate Resource Requirements

```bash
# Current resource usage
k top pods -l app=<label>

# With maxSurge=50% and 4 replicas:
# Peak usage = 4 + (4 * 0.5) = 6 pods
# Plan resources accordingly
```

### Monitor During Rollout

```bash
# Watch resource usage
k top pods -l app=<label> --watch

# Watch pod status
k get pods -l app=<label> -w

# Watch replica sets
k get rs -l app=<label> -w
```

## CKAD Exam Tips

### Time-Saving Commands

```bash
# Quick status check
k rollout status deploy/<name> && echo "SUCCESS" || echo "FAILED"

# One-liner rollback
k rollout undo deploy/<name> && k rollout status deploy/<name>

# Quick verification
k get pods -l app=<label> --no-headers | grep Running | wc -l
```

### Troubleshooting Priority

1. **Check rollout status** first
2. **Look at pod status** for obvious issues
3. **Check recent events** for system messages
4. **Examine logs** if pods are running but failing
5. **Rollback quickly** if unsure - time is limited

### Common Mistakes to Avoid

- Forgetting readiness probes (causes hung rollouts)
- Not setting resource limits (causes OOM kills)
- Using `kubectl replace` instead of `apply` (loses history)
- Waiting too long to rollback (wastes exam time)
- Not verifying service endpoints after rollout
