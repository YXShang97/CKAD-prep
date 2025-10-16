# CKAD Pod Commands Quick Reference

## Create Pods

```bash
kubectl run nginx --image=nginx                              # Basic pod
kubectl run nginx --image=nginx --port=80                    # With port
kubectl run nginx --image=nginx --env="VAR1=value1"         # With env vars
kubectl run nginx --image=nginx --labels="app=nginx"        # With labels
kubectl run nginx --image=nginx --dry-run=client -o yaml    # Generate YAML
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml  # Save to file
```

## Get Pod Info

```bash
kubectl get pods                           # List pods
kubectl get pods -o wide                   # More details
kubectl get pods --show-labels             # Show labels
kubectl get pods -l app=nginx              # Filter by label
kubectl get pods --watch                   # Watch changes
kubectl describe pod nginx                 # Detailed info
```

## Pod Logs & Debugging

```bash
kubectl logs nginx                         # Get logs
kubectl logs -f nginx                      # Follow logs
kubectl logs nginx -c container-name      # Specific container
kubectl logs nginx --previous             # Previous instance
kubectl logs nginx --tail=50              # Last 50 lines
```

## Execute Commands

```bash
kubectl exec -it nginx -- /bin/bash       # Interactive shell
kubectl exec nginx -- ls /                # Single command
kubectl exec -it nginx -c container -- sh # Specific container
```

## Port Forward & File Copy

```bash
kubectl port-forward nginx 8080:80                    # Port forward
kubectl cp nginx:/path/file /local/path               # Copy from pod
kubectl cp /local/path nginx:/path/file               # Copy to pod
```

## Manage Pods

```bash
kubectl delete pod nginx                   # Delete pod
kubectl delete pods --all                  # Delete all pods
kubectl delete pods -l app=nginx          # Delete by label
kubectl edit pod nginx                     # Edit pod
kubectl apply -f pod.yaml                 # Apply from file

# Extract pod definition to file
kubectl get pod <pod-name> -o yaml > pod-definition.yaml
# Then edit the file to make necessary changes, delete, and re-create the pod
```

## Troubleshooting

```bash
kubectl get events                                    # All events
kubectl get events --field-selector involvedObject.name=nginx  # Pod events
kubectl top pod nginx                                # Resource usage
kubectl get pod nginx -o jsonpath='{.status.phase}' # Pod status
```

## Multi-container Pods

```bash
kubectl logs nginx -c sidecar              # Container logs
kubectl exec -it nginx -c sidecar -- sh    # Access container
kubectl get pod nginx -o jsonpath='{.spec.containers[*].name}'  # List containers
```

## Alias Usage Examples

```bash
# Instead of kubectl, use k (pre-configured in exam)
k run nginx --image=nginx                    # Create pod
k get po                                     # List pods
k describe po nginx                          # Describe pod
k logs nginx -f                              # Follow logs
k exec -it nginx -- bash                    # Execute into pod
k delete po nginx                            # Delete pod
k get po -o wide                             # More details
k get po -l app=nginx                        # Filter by label
```

## CKAD Exam Tips

- Use `--dry-run=client -o yaml` for templates
- Use `k` alias (pre-configured): `k get po`, `k describe po nginx`, `k logs nginx`
- Use resource shortcuts: `po`, `svc`, `deploy`
- Practice imperative commands
- Master label selectors
