# CKAD ServiceAccount Commands Quick Reference

## What are ServiceAccounts?

ServiceAccounts provide identity for processes running in pods. Used for authentication and authorization with Kubernetes API and external services.

## Create ServiceAccounts

```bash
kubectl create serviceaccount webapp-sa               # Create ServiceAccount
kubectl create sa webapp-sa                          # Short form
kubectl apply -f serviceaccount.yaml                 # From YAML

# Generate YAML
kubectl create serviceaccount webapp-sa --dry-run=client -o yaml > sa.yaml
```

## Get ServiceAccount Info

```bash
kubectl get serviceaccounts                          # List all ServiceAccounts
kubectl get sa                                       # Short form
kubectl describe serviceaccount webapp-sa            # Detailed info
kubectl get sa webapp-sa -o yaml                     # View YAML
```

## Manage ServiceAccounts

```bash
kubectl delete serviceaccount webapp-sa              # Delete ServiceAccount
kubectl delete sa webapp-sa                          # Short form
kubectl patch sa webapp-sa -p '{"automountServiceAccountToken":false}' # Disable token mount
```

## RBAC Integration

```bash
# Create Role
kubectl create role pod-reader --verb=get,list,watch --resource=pods

# Create ClusterRole
kubectl create clusterrole pod-reader --verb=get,list,watch --resource=pods

# Bind Role to ServiceAccount
kubectl create rolebinding webapp-binding --role=pod-reader --serviceaccount=default:webapp-sa

# Bind ClusterRole to ServiceAccount
kubectl create clusterrolebinding webapp-binding --clusterrole=pod-reader --serviceaccount=default:webapp-sa
```

## Use ServiceAccount in Pods

```bash
# In pod spec:
# serviceAccountName: webapp-sa
# automountServiceAccountToken: true/false
```

## Check ServiceAccount Token

```bash
# View mounted token in pod (Kubernetes 1.24+: projected volume token)
kubectl exec -it pod-name -- cat /var/run/secrets/kubernetes.io/serviceaccount/token

# View ServiceAccount from within pod
kubectl exec -it pod-name -- cat /var/run/secrets/kubernetes.io/serviceaccount/namespace
kubectl exec -it pod-name -- cat /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
```

## Token Management (Kubernetes 1.24+)

```bash
# Create temporary token (expires in 1 hour by default)
kubectl create token webapp-sa

# Create token with custom duration
kubectl create token webapp-sa --duration=2h

# Create permanent Secret-based token (legacy approach, not recommended)
kubectl apply -f - <<EOF
apiVersion: v1
kind: Secret
metadata:
  name: webapp-sa-token
  annotations:
    kubernetes.io/service-account.name: webapp-sa
type: kubernetes.io/service-account-token
EOF
```

## CKAD Exam Tips

- Every namespace has a 'default' ServiceAccount
- **Kubernetes 1.24+**: Uses projected volume tokens (auto-rotating, secure)
- **Pre-1.24**: Used permanent Secret-based tokens (security risk)
- ServiceAccount tokens are automatically mounted unless disabled
- Use ServiceAccounts for pod-to-API server authentication
- Combine with RBAC for fine-grained permissions
- Token location in pod: `/var/run/secrets/kubernetes.io/serviceaccount/`
- ServiceAccount name must be DNS subdomain name
- Modern tokens expire and rotate automatically (more secure)
