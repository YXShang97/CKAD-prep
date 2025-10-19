# CKAD Scheduling Commands Quick Reference

## Node Taints

Taints prevent pods from being scheduled on nodes unless they have matching tolerations.

### Taint Management

```bash
# Add taint to node
kubectl taint nodes node1 key=value:taint-effect
kubectl taint nodes node1 app=frontend:NoSchedule
kubectl taint nodes node1 environment=production:NoExecute

# Remove taint from node (add minus sign)
kubectl taint nodes node1 key=value:NoSchedule-
kubectl taint nodes node1 app=frontend:NoSchedule-

# View node taints
kubectl describe node node1
kubectl get nodes -o json | jq '.items[].spec.taints'
```

### Taint Effects

- **NoSchedule**: Pods without tolerations won't be scheduled
- **PreferNoSchedule**: Kubernetes tries to avoid scheduling (soft constraint)
- **NoExecute**: Existing pods without tolerations are evicted

## Node Selection

### Node Selector

```bash
# Label nodes
kubectl label nodes node1 disktype=ssd
kubectl label nodes node1 environment=production
kubectl label nodes node1 zone=us-west-1a

# Remove labels
kubectl label nodes node1 disktype-

# View node labels
kubectl get nodes --show-labels
kubectl describe node node1
```

### Node Information

```bash
# List all nodes
kubectl get nodes
kubectl get nodes -o wide

# Node details
kubectl describe node node1
kubectl top node node1

# Node capacity and allocatable resources
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.capacity.cpu}{"\t"}{.status.capacity.memory}{"\n"}{end}'
```

## Pod Scheduling

### Check Pod Scheduling

```bash
# View pod node assignment
kubectl get pods -o wide
kubectl get pod pod-name -o jsonpath='{.spec.nodeName}'

# Pod scheduling events
kubectl describe pod pod-name
kubectl get events --field-selector involvedObject.name=pod-name

# Pods stuck in Pending
kubectl get pods --field-selector=status.phase=Pending
```

### Force Pod Rescheduling

```bash
# Delete pod to reschedule
kubectl delete pod pod-name

# Cordon node (prevent new pods)
kubectl cordon node1

# Drain node (move pods and cordon)
kubectl drain node1 --ignore-daemonsets --delete-emptydir-data

# Uncordon node (allow scheduling)
kubectl uncordon node1
```

## CKAD Exam Tips

### Scheduling Concepts

- **NodeSelector**: Simple key-value label matching
- **Affinity**: Complex expressions and preferences
- **Anti-affinity**: Spread pods across nodes/zones
- **Taints/Tolerations**: Node restrictions and exceptions
- **Resource requests**: Scheduler finds nodes with available resources

### Common Scenarios

- Schedule pods on specific nodes (nodeSelector)
- Avoid scheduling on tainted nodes (tolerations)
- Spread pods across availability zones (anti-affinity)
- Schedule pods near other pods (affinity)
- Troubleshoot pending pods (resource constraints, taints)

### Troubleshooting

```bash
# Why is pod pending?
kubectl describe pod pod-name  # Check events section
kubectl get nodes              # Check node availability
kubectl describe nodes         # Check node capacity and taints
```
