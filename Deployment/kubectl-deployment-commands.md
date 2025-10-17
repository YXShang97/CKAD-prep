# CKAD Deployment Commands Quick Reference

## Deployment → ReplicaSet → Pods Relationship

**Deployment** manages **ReplicaSets**, which manage **Pods**:

```
Deployment (manages) → ReplicaSet (manages) → Pods
```

- **Deployment**: Provides declarative updates, rolling updates, rollback capability
- **ReplicaSet**: Ensures desired number of pod replicas are running
- **Pods**: The actual running containers

When you update a Deployment, it creates a new ReplicaSet and gradually replaces the old one (rolling update).

## Create Deployments

```bash
kubectl create deployment nginx --image=nginx                    # Basic deployment
kubectl create deployment nginx --image=nginx --replicas=3      # With replicas
kubectl create deployment nginx --image=nginx --port=80         # With port
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml  # Generate YAML
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > deployment.yaml  # Save to file

# Create from YAML
kubectl apply -f deployment.yaml
```

## Get Deployment Info

```bash
kubectl get deployments                        # List deployments
kubectl get deploy                             # Short form
kubectl get deploy -o wide                     # More details
kubectl get deploy --show-labels               # Show labels
kubectl get deploy -l app=nginx                # Filter by label
kubectl describe deployment nginx              # Detailed info

# Check rollout status
kubectl rollout status deployment/nginx
kubectl rollout history deployment/nginx
```

## Scale Deployments

```bash
kubectl scale deployment nginx --replicas=5    # Scale to 5 replicas
kubectl scale deploy nginx --replicas=0        # Scale down to 0
kubectl scale --replicas=3 deployment/nginx    # Alternative syntax

# Autoscaling
kubectl autoscale deployment nginx --min=2 --max=10 --cpu-percent=80
```

## Update Deployments (Rolling Updates)

```bash
# Update image (triggers rolling update)
kubectl set image deployment/nginx nginx=nginx:1.21

# Update with patch
kubectl patch deployment nginx -p '{"spec":{"template":{"spec":{"containers":[{"name":"nginx","image":"nginx:1.21"}]}}}}'

# Edit deployment directly
kubectl edit deployment nginx

# Replace from file
kubectl replace -f deployment.yaml

# Apply updates from file
kubectl apply -f deployment.yaml
```

## Rollback Deployments

```bash
# View rollout history
kubectl rollout history deployment/nginx

# Rollback to previous version
kubectl rollout undo deployment/nginx

# Rollback to specific revision
kubectl rollout undo deployment/nginx --to-revision=2

# Pause/resume rollout
kubectl rollout pause deployment/nginx
kubectl rollout resume deployment/nginx
```

## Manage Deployments

```bash
kubectl delete deployment nginx                # Delete deployment
kubectl delete deploy --all                    # Delete all deployments
kubectl delete deploy -l app=nginx            # Delete by label

# Extract deployment definition to file
kubectl get deployment <deploy-name> -o yaml > deployment-definition.yaml
# Then edit the file to make necessary changes and apply
```

## Deployment and ReplicaSets

```bash
# Get ReplicaSets managed by deployment
kubectl get rs
kubectl get rs -l app=nginx                    # ReplicaSets for nginx deployment

# Get pods managed by deployment
kubectl get pods -l app=nginx
kubectl get pods --selector=app=nginx

# See the relationship
kubectl get deploy,rs,pods -l app=nginx
```

## Troubleshooting

```bash
kubectl describe deployment nginx              # Check deployment events
kubectl get events --sort-by=.metadata.creationTimestamp  # All events
kubectl rollout status deployment/nginx       # Check rollout status
kubectl get rs -l app=nginx                   # Check ReplicaSets
kubectl logs -l app=nginx                     # Logs from all pods
kubectl top deployment nginx                  # Resource usage
```

## CKAD Exam Tips

- Deployments are the preferred way to manage pods (not ReplicaSets directly)
- Practice rolling updates and rollbacks - commonly tested
- Understand the Deployment → ReplicaSet → Pod hierarchy
- Use `--record` flag for rollout history (deprecated but may appear in older exams)
- Master scaling and autoscaling commands
- Use `k` alias (pre-configured): `k get deploy`, `k scale deploy nginx --replicas=5`
