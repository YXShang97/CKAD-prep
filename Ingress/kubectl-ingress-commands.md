# kubectl Ingress Commands for CKAD

## Ingress Commands

### Create and Manage Ingress
```bash
# Create ingress from YAML (no imperative command)
k apply -f ingress.yaml

# Get ingress resources
k get ingress
k get ing                        # Short name
k get ing -A                     # All namespaces
k get ing -o wide                # Show hosts and addresses

# Describe ingress
k describe ingress web-ingress
k describe ing api-ingress
```

### View Ingress Details
```bash
# Get ingress configuration
k get ing <ingress-name> -o yaml
k get ing <ingress-name> -o jsonpath='{.spec.rules[*].host}'

# Check ingress status
k get ing <ingress-name> -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

### Ingress Controller Management
```bash
# Check ingress controller pods
k get pods -n ingress-nginx
k get pods -n kube-system -l app=nginx-ingress

# Check ingress controller service
k get svc -n ingress-nginx
k describe svc ingress-nginx-controller -n ingress-nginx

# Check ingress controller logs
k logs -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx
```

### Test Ingress Connectivity
```bash
# Test ingress endpoints
curl -H "Host: example.com" http://<ingress-ip>/
curl -H "Host: api.example.com" http://<ingress-ip>/api

# Test with kubectl port-forward
k port-forward -n ingress-nginx svc/ingress-nginx-controller 8080:80
curl -H "Host: example.com" http://localhost:8080/

# Test from inside cluster
k run test --image=curlimages/curl --rm -it -- sh
curl -H "Host: example.com" http://ingress-nginx-controller.ingress-nginx/
```

### Troubleshoot Ingress
```bash
# Check ingress controller status
k get pods -n ingress-nginx
k describe pod <controller-pod> -n ingress-nginx

# Check ingress events
k get events --field-selector involvedObject.name=<ingress-name>
k describe ing <ingress-name>

# Check backend services
k get svc
k get endpoints
k describe svc <backend-service>
```

## CKAD Exam Patterns

### Generate Ingress Template
```bash
# No imperative command - create template
cat <<EOF > ingress-template.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: web-service
            port:
              number: 80
EOF
```

### Quick Ingress Validation
```bash
# Check if ingress exists
k get ing <ingress-name>

# Verify backend services exist
k get svc <service-name>
k get endpoints <service-name>

# Check ingress address
k get ing <ingress-name> -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

### Common Debugging Commands
```bash
# Check ingress controller configuration
k get configmap -n ingress-nginx
k describe configmap nginx-configuration -n ingress-nginx

# Check ingress controller service account
k get sa -n ingress-nginx
k describe sa nginx-ingress-serviceaccount -n ingress-nginx

# Verify RBAC permissions
k get clusterrole | grep nginx
k get clusterrolebinding | grep nginx
```

## Time-Saving Aliases
```bash
# Ingress aliases
alias kgi='kubectl get ingress'
alias kdi='kubectl describe ingress'
alias kai='kubectl apply -f'

# Ingress controller aliases
alias kgic='kubectl get pods -n ingress-nginx'
alias kdic='kubectl describe pod -n ingress-nginx'
alias klic='kubectl logs -n ingress-nginx'
```