# CKAD ReplicaSet Commands Quick Reference

## Create ReplicaSet

```bash
# Create ReplicaSet from YAML file
kubectl apply -f replicaset.yaml

# Generate ReplicaSet YAML template
kubectl create replicaset nginx-rs --image=nginx --replicas=3 --dry-run=client -o yaml > replicaset.yaml

# Create ReplicaSet imperatively (limited options)
kubectl create replicaset nginx-rs --image=nginx --replicas=3
```

## Get ReplicaSet Info

```bash
kubectl get replicasets                        # List all ReplicaSets
kubectl get rs                                 # Short form
kubectl get rs -o wide                         # More details
kubectl get rs --show-labels                   # Show labels
kubectl get rs -l app=nginx                    # Filter by label
kubectl describe rs nginx-rs                   # Detailed info
```

## Scale ReplicaSet

```bash
kubectl scale rs nginx-rs --replicas=5         # Scale to 5 replicas
kubectl scale rs nginx-rs --replicas=0         # Scale down to 0
kubectl scale --replicas=3 rs/nginx-rs         # Alternative syntax
kubectl scale --replicas=6 -f replicaset-definition.yaml

# Note: Scaling only changes replica count, does NOT update existing pods
# To update pod template, you must delete and recreate the ReplicaSet
```

## Manage ReplicaSet

```bash
kubectl delete rs nginx-rs                     # Delete ReplicaSet
kubectl delete rs nginx-rs --cascade=orphan    # Delete RS but keep pods
kubectl delete rs --all                        # Delete all ReplicaSets
kubectl edit rs nginx-rs                       # Edit ReplicaSet
kubectl replace -f replicaset.yaml             # Replace from file (recreates)
kubectl apply -f replicaset.yaml               # Apply from file
```

## ReplicaSet and Pods

```bash
kubectl get pods -l app=nginx                  # Get pods managed by RS
kubectl get rs nginx-rs -o jsonpath='{.status.replicas}'        # Current replicas
kubectl get rs nginx-rs -o jsonpath='{.status.readyReplicas}'   # Ready replicas
kubectl get rs nginx-rs -o jsonpath='{.spec.replicas}'          # Desired replicas
```

## Troubleshooting

```bash
kubectl describe rs nginx-rs                   # Check events and status
kubectl get events --sort-by=.metadata.creationTimestamp  # All events
kubectl logs -l app=nginx                      # Logs from all pods
kubectl get pods -l app=nginx -o wide          # Pod distribution
```

## Alias Usage Examples

```bash
# Instead of kubectl, use k (pre-configured in exam)
k get rs                                         # List ReplicaSets
k describe rs nginx-rs                           # Describe ReplicaSet
k scale rs nginx-rs --replicas=5                 # Scale ReplicaSet
k get po -l app=nginx                            # Get managed pods
k delete rs nginx-rs                             # Delete ReplicaSet
```

## CKAD Exam Tips

- ReplicaSets are rarely created directly (use Deployments instead)
- Focus on scaling and troubleshooting existing ReplicaSets
- Understand the relationship between ReplicaSet and Pods
- Practice label selectors and pod template modifications
- Know how to orphan pods when deleting ReplicaSet
- **Important**: ReplicaSet scaling only changes replica count, NOT pod templates
- To update pod image/config, you must delete and recreate the ReplicaSet
