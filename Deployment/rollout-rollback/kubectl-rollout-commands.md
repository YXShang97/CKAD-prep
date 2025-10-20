# kubectl Rollout & Rollback Commands for CKAD

## Rolling Update Commands

### Trigger Rolling Update

```bash
# Update image (triggers rollout)
k set image deploy/nginx nginx=nginx:1.20
k set image deploy/nginx container-name=nginx:1.20

# Edit deployment directly
k edit deploy nginx

# Apply updated manifest
k apply -f updated-deployment.yaml

# Scale deployment
k scale deploy nginx --replicas=5
```

### Monitor Rollout Status

```bash
# Check rollout status
k rollout status deploy/nginx
k rollout status deploy/nginx --timeout=300s

# Watch rollout progress
k get pods -w
k get rs -w
```

## Rollout Management

### Pause/Resume Rollout

```bash
# Pause rollout (stop mid-update)
k rollout pause deploy/nginx

# Resume paused rollout
k rollout resume deploy/nginx

# Restart rollout (force new rollout)
k rollout restart deploy/nginx
```

## Rollback Commands

### View Rollout History

```bash
# Show rollout history
k rollout history deploy/nginx

# Show specific revision details
k rollout history deploy/nginx --revision=2

# Show history with change-cause
k rollout history deploy/nginx
```

### Rollback Operations

```bash
# Rollback to previous version
k rollout undo deploy/nginx

# Rollback to specific revision
k rollout undo deploy/nginx --to-revision=2

# Check rollback status
k rollout status deploy/nginx
```

## CKAD Exam Patterns

### Quick Rollout Check

```bash
# One-liner status check
k rollout status deploy/nginx && echo "Rollout completed"

# Wait for rollout completion
k rollout status deploy/nginx --watch=true
```

### Emergency Rollback

```bash
# Quick rollback workflow
k rollout undo deploy/nginx              # Rollback
k rollout status deploy/nginx            # Verify
k get pods                               # Check pods
```

### Exam Troubleshooting

```bash
# Rollout stuck? Check these
k describe deploy nginx                  # Check deployment
k get rs                                # Check replica sets
k get events --sort-by=.lastTimestamp   # Check events
k logs -l app=nginx                     # Check pod logs
```

## Change Cause Annotation

### Add Change Cause

```bash
# Update with change-cause
k set image deploy/nginx nginx=nginx:1.20 --record
```
