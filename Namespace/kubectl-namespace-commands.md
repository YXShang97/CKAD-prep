# CKAD Namespace Commands Quick Reference

## What are Namespaces?

Namespaces provide virtual clusters within a physical cluster for resource isolation and organization.

## Create Namespaces

```bash
kubectl create namespace dev                          # Basic namespace
kubectl create ns prod                                # Short form
kubectl create namespace test --dry-run=client -o yaml > ns.yaml  # Generate YAML
kubectl apply -f namespace.yaml                      # From YAML file
```

## Get Namespace Info

```bash
kubectl get namespaces                               # List all namespaces
kubectl get ns                                      # Short form
kubectl describe namespace dev                       # Detailed info
kubectl get ns --show-labels                        # Show labels
```

## Work with Namespaces

```bash
# Set default namespace for current context
kubectl config set-context --current --namespace=dev

# View current namespace
kubectl config view --minify | grep namespace

# Run commands in specific namespace
kubectl get pods -n dev                             # Get pods in dev namespace
kubectl get pods --namespace=prod                   # Alternative syntax
kubectl get pods -A                                 # All namespaces
kubectl get pods --all-namespaces                   # Alternative syntax

# Create resources in specific namespace
kubectl run nginx --image=nginx -n dev
kubectl apply -f pod.yaml -n prod
```

## Cross-Namespace Service Access

### DNS Resolution Format

```bash
# Same namespace
service-name

# Different namespace
service-name.namespace-name.svc.cluster.local

# Examples
mysql                                   # mysql service in same namespace
mysql.default                           # mysql service in default namespace
mysql.database.svc.cluster.local        # mysql service in database namespace (full FQDN)
```

### Testing Cross-Namespace Connectivity

```bash
# Create test pod to test connectivity
kubectl run test-pod --image=busybox -it --rm -- sh

# Inside the pod, test connectivity
nslookup mysql.database.svc.cluster.local
wget -qO- http://web-service.frontend.svc:80
curl http://api-service.backend:3000/health

# Test from specific namespace
kubectl exec -it test-pod -n dev -- nslookup mysql.database.svc
```

## Manage Namespaces

```bash
kubectl delete namespace dev                         # Delete namespace (and all resources)
kubectl delete ns --all                             # Delete all namespaces (dangerous!)
kubectl edit namespace dev                          # Edit namespace
```

## Resource Quotas and Limits

```bash
# Create resource quota
kubectl create quota dev-quota --hard=requests.cpu=2,requests.memory=4Gi -n dev

# Get resource usage
kubectl describe quota -n dev
kubectl top nodes                                   # Node resource usage
kubectl top pods -n dev                            # Pod resource usage in namespace
```

## CKAD Exam Tips

- Default namespaces: `default`, `kube-system`, `kube-public`
- Always check which namespace you're working in
- Use `-n namespace` flag for specific namespace operations
- Use `-A` or `--all-namespaces` to see resources across all namespaces
- Deleting a namespace deletes ALL resources within it
- **DNS Format**: `service-name.namespace.svc.cluster.local` for cross-namespace access
- **Short DNS**: `service-name.namespace` usually works fine
- Use `k` alias: `k get po -n dev`, `k create ns test`
