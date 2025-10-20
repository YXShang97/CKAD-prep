# CKAD Kubectl Reference

## Pre-configured Alias

The CKAD exam environment has this alias ready:

```bash
alias k='kubectl'
```

## Usage Examples

```bash
k get all                         # Get all resources
k get po                          # Get pods
k describe po nginx               # Describe pod
k edit svc nginx                  # Edit service
k delete deploy myapp             # Delete deployment
k logs nginx                      # Get logs
k exec -it nginx -- bash          # Execute into pod
k apply -f file.yaml              # Apply YAML
```

## Resource Shortcuts

```bash
pods → po          services → svc       deployments → deploy
replicasets → rs   configmaps → cm      secrets → secret
namespaces → ns    nodes → no           ingresses → ing
```

## Useful Flags

```bash
k get po -o wide                  # More details
k get po -A                       # All namespaces
k get po -w                       # Watch changes
k get po -l app=nginx             # Filter by label
k create deploy nginx --image=nginx --dry-run=client -o yaml > mypod.yaml # Generate YAML
```
