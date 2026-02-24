# kubectl NetworkPolicy Commands for CKAD

## NetworkPolicy Commands

### Create and Manage NetworkPolicies
```bash
# Create from YAML (no imperative command)
k apply -f networkpolicy.yaml

# Get network policies
k get networkpolicies
k get netpol                     # Short name
k get netpol -A                  # All namespaces

# Describe network policy
k describe networkpolicy deny-all
k describe netpol allow-frontend
```

### View NetworkPolicy Details
```bash
# Get specific network policy
k get netpol <policy-name> -o yaml
k get netpol <policy-name> -o wide

# Show policy rules
k describe netpol <policy-name>
```

### Test Network Connectivity
```bash
# Test pod-to-pod connectivity
k exec -it <source-pod> -- wget -qO- --timeout=2 <target-ip>:80
k exec -it <source-pod> -- nc -zv <target-ip> 80
k exec -it <source-pod> -- ping <target-ip>

# Test service connectivity
k exec -it <source-pod> -- curl <service-name>.<namespace>:80
k exec -it <source-pod> -- nslookup <service-name>
```

### Troubleshoot NetworkPolicy
```bash
# Check pod labels (important for selectors)
k get pods --show-labels
k get pods -l app=frontend

# Check namespace labels
k get namespaces --show-labels
k label namespace production tier=prod

# Verify policy is applied to pods
k describe pod <pod-name> | grep -i network
k get pods -o wide
```

### NetworkPolicy Management
```bash
# Delete network policy
k delete networkpolicy <policy-name>
k delete netpol <policy-name>

# Replace policy
k replace -f networkpolicy.yaml

# Patch policy (rare in exam)
k patch netpol <policy-name> -p '{"spec":{"podSelector":{}}}'
```

## CKAD Exam Patterns

### Generate NetworkPolicy Template
```bash
# No imperative command - must write YAML
# Use these templates as starting point

# Basic deny-all template
cat <<EOF > deny-all.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
EOF
```

### Common Test Scenarios
```bash
# 1. Create test pods for connectivity testing
k run test-pod --image=busybox --rm -it -- sh
k run frontend --image=nginx --labels="app=frontend"
k run backend --image=nginx --labels="app=backend"

# 2. Test before applying policy
k exec test-pod -- wget -qO- frontend:80

# 3. Apply network policy
k apply -f networkpolicy.yaml

# 4. Test after applying policy
k exec test-pod -- wget -qO- --timeout=2 frontend:80
```

### Quick NetworkPolicy Validation
```bash
# Check if policy exists
k get netpol -o name | grep <policy-name>

# Verify policy targets correct pods
k get pods -l <selector-from-policy>

# Check policy rules
k describe netpol <policy-name> | grep -A10 "Allowing\|Denying"
```

## Troubleshooting Commands

### Connectivity Issues
```bash
# Test specific ports
k exec <pod> -- nc -zv <target> 80
k exec <pod> -- nc -zv <target> 443
k exec <pod> -- nc -zv <target> 3306

# DNS resolution test
k exec <pod> -- nslookup <service-name>
k exec <pod> -- dig <service-name>

# Network debugging pod
k run netshoot --image=nicolaka/netshoot --rm -it -- bash
```

### Label Verification
```bash
# Check pod labels match policy selector
k get pods --show-labels -l <policy-selector>

# Check namespace labels
k get ns --show-labels
k describe ns <namespace>

# Add missing labels
k label pod <pod-name> app=frontend
k label namespace <ns-name> tier=production
```

## Time-Saving Aliases
```bash
# NetworkPolicy aliases
alias kgnp='kubectl get networkpolicies'
alias kdnp='kubectl describe networkpolicy'
alias kanp='kubectl apply -f'

# Testing aliases
alias ktest='kubectl run test --image=busybox --rm -it --'
alias kwget='kubectl exec -it'
```