# Pod Observability for CKAD

## Health Probes

### Liveness Probe

**Purpose**: Determines if container is running properly
**Action**: Kubernetes **restarts** container if probe fails
**When to use**: Detect deadlocks, infinite loops, or crashed processes

### Readiness Probe

**Purpose**: Determines if container is ready to serve traffic
**Action**: Kubernetes **removes** pod from service endpoints if probe fails
**When to use**: During startup, dependency checks, or temporary unavailability

### Startup Probe (Kubernetes 1.16+)

**Purpose**: Protects slow-starting containers during initialization
**Action**: Disables liveness/readiness probes until startup succeeds
**When to use**: Legacy apps with long startup times

## Probe Types

### HTTP Probe

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
    httpHeaders:
      - name: Custom-Header
        value: liveness
  initialDelaySeconds: 30
  periodSeconds: 10
```

### TCP Probe

```yaml
readinessProbe:
  tcpSocket:
    port: 5432
  initialDelaySeconds: 5
  periodSeconds: 5
```

### Command Probe

```yaml
livenessProbe:
  exec:
    command:
      - cat
      - /tmp/healthy
  initialDelaySeconds: 30
  periodSeconds: 10
```

## Probe Parameters

- **initialDelaySeconds**: Wait before first probe
- **periodSeconds**: How often to probe
- **timeoutSeconds**: Probe timeout
- **successThreshold**: Min consecutive successes
- **failureThreshold**: Max consecutive failures before action

## Logging

### Container Logs

```bash
# View logs
kubectl logs pod-name
kubectl logs pod-name -c container-name
kubectl logs pod-name -f --tail=100

# Previous container instance
kubectl logs pod-name --previous

# All containers in pod
kubectl logs pod-name --all-containers
```

### Log Locations

- **Container logs**: `/var/log/containers/`
- **Pod logs**: `/var/log/pods/`
- **Application logs**: Custom paths mounted via volumes

## Monitoring

### Resource Usage

```bash
# Pod resource usage
kubectl top pod pod-name
kubectl top pods -n namespace

# Node resource usage
kubectl top nodes
```

### Pod Status

```bash
# Pod details
kubectl describe pod pod-name
kubectl get pod pod-name -o wide

# Pod events
kubectl get events --field-selector involvedObject.name=pod-name
```

## CKAD Exam Tips

### Health Probe Best Practices

- **Always use readiness probes** for services
- **Use liveness probes** for long-running processes
- **Set appropriate timeouts** and thresholds
- **Test endpoints** return proper HTTP codes
- **Avoid heavy operations** in probe endpoints

### Common Scenarios

- Pod stuck in **Pending**: Resource constraints, scheduling issues
- Pod stuck in **CrashLoopBackOff**: Failing liveness probe
- Service has no endpoints: Failing readiness probe
- Pod not receiving traffic: Readiness probe not configured

### Troubleshooting Workflow

1. **Check pod status**: `kubectl get pods`
2. **Describe pod**: `kubectl describe pod`
3. **Check logs**: `kubectl logs pod-name`
4. **Check events**: `kubectl get events`
5. **Check resource usage**: `kubectl top pod`
6. **Exec into pod**: `kubectl exec -it pod-name -- sh`
