# Ingress Troubleshooting Guide - CKAD

## Common Ingress Issues

### 1. Ingress Not Getting External IP
**Problem**: Ingress shows `<pending>` or no address

**Investigation**:
```bash
k get ing <ingress-name>
k describe ing <ingress-name>
k get svc -n ingress-nginx
```

**Common Causes**:
- Ingress controller not installed
- Ingress controller service misconfigured
- Cloud provider load balancer issues

**Solutions**:
```bash
# Check ingress controller
k get pods -n ingress-nginx
k get svc ingress-nginx-controller -n ingress-nginx

# For local testing, use NodePort
k patch svc ingress-nginx-controller -n ingress-nginx -p '{"spec":{"type":"NodePort"}}'
```

### 2. 404 Not Found Errors
**Problem**: Ingress returns 404 for valid paths

**Investigation**:
```bash
k describe ing <ingress-name>
k get svc <backend-service>
k get endpoints <backend-service>
```

**Common Causes**:
- Backend service doesn't exist
- Service selector doesn't match pods
- Wrong service port in ingress
- Path configuration issues

**Solutions**:
```bash
# Verify backend service
k get svc <service-name>
k describe svc <service-name>
k get endpoints <service-name>

# Check pod labels
k get pods --show-labels
k get pods -l <service-selector>
```

### 3. SSL/TLS Certificate Issues
**Problem**: HTTPS not working or certificate errors

**Investigation**:
```bash
k describe ing <ingress-name>
k get secrets <tls-secret-name>
k describe secret <tls-secret-name>
```

**Common Causes**:
- TLS secret missing or invalid
- Certificate doesn't match hostname
- Wrong secret format

**Solutions**:
```bash
# Check TLS secret
k get secret <tls-secret> -o yaml

# Create TLS secret
k create secret tls <secret-name> --cert=tls.crt --key=tls.key

# Verify certificate
openssl x509 -in tls.crt -text -noout
```

### 4. Ingress Controller Issues
**Problem**: Ingress controller not responding

**Investigation**:
```bash
k get pods -n ingress-nginx
k describe pod <controller-pod> -n ingress-nginx
k logs <controller-pod> -n ingress-nginx
```

**Common Causes**:
- Controller pod not running
- RBAC permissions missing
- ConfigMap issues
- Resource constraints

**Solutions**:
```bash
# Restart controller
k rollout restart deployment nginx-ingress-controller -n ingress-nginx

# Check RBAC
k get clusterrole | grep nginx
k get clusterrolebinding | grep nginx

# Check resources
k top pods -n ingress-nginx
```

## CKAD Troubleshooting Workflow

### Step 1: Check Ingress Status
```bash
k get ing
k describe ing <ingress-name>
k get ing <ingress-name> -o yaml
```

### Step 2: Verify Ingress Controller
```bash
k get pods -n ingress-nginx
k get svc -n ingress-nginx
k logs -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx
```

### Step 3: Check Backend Services
```bash
k get svc
k describe svc <backend-service>
k get endpoints <backend-service>
k get pods -l <service-selector>
```

### Step 4: Test Connectivity
```bash
# Test ingress directly
curl -H "Host: <hostname>" http://<ingress-ip>/

# Test backend service
k port-forward svc/<service-name> 8080:80
curl http://localhost:8080/

# Test from inside cluster
k run test --image=curlimages/curl --rm -it -- sh
curl http://<service-name>.<namespace>/
```

### Step 5: Check Events and Logs
```bash
k get events --sort-by=.lastTimestamp
k get events --field-selector involvedObject.name=<ingress-name>
k logs -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx --tail=50
```

## Quick Fixes

### Fix Missing Ingress Controller
```bash
# Install NGINX ingress controller (simplified)
k apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml

# Or use local setup
k apply -f ingress-controller-setup.yaml
```

### Fix Service Issues
```bash
# Check if service exists
k get svc <service-name> || echo "Service not found"

# Create missing service
k expose deployment <deployment-name> --port=80 --target-port=8080

# Fix service selector
k patch svc <service-name> -p '{"spec":{"selector":{"app":"correct-label"}}}'
```

### Fix Path Issues
```bash
# Add rewrite annotation for path-based routing
k annotate ing <ingress-name> nginx.ingress.kubernetes.io/rewrite-target=/

# Fix path type
k patch ing <ingress-name> -p '{"spec":{"rules":[{"http":{"paths":[{"pathType":"Prefix"}]}}]}}'
```

### Fix TLS Issues
```bash
# Create self-signed certificate for testing
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key -out tls.crt -subj "/CN=example.com"

# Create TLS secret
k create secret tls tls-secret --cert=tls.crt --key=tls.key

# Update ingress with TLS
k patch ing <ingress-name> -p '{"spec":{"tls":[{"hosts":["example.com"],"secretName":"tls-secret"}]}}'
```

## Testing Strategies

### Local Testing with Port-Forward
```bash
# Port-forward ingress controller
k port-forward -n ingress-nginx svc/ingress-nginx-controller 8080:80

# Test with curl
curl -H "Host: example.com" http://localhost:8080/
```

### Testing with Test Pod
```bash
# Create test pod
k run test --image=curlimages/curl --restart=Never --rm -it -- sh

# Inside pod, test ingress
curl -H "Host: example.com" http://ingress-nginx-controller.ingress-nginx/
```

### External Testing
```bash
# Get ingress IP
INGRESS_IP=$(k get ing <ingress-name> -o jsonpath='{.status.loadBalancer.ingress[0].ip}')

# Test with curl
curl -H "Host: example.com" http://$INGRESS_IP/
```

## CKAD Exam Tips

### Quick Validation Commands
```bash
# Check ingress exists and has address
k get ing -o wide

# Verify backend service has endpoints
k get endpoints

# Quick connectivity test
k run test --image=nginx --rm -it -- curl -H "Host: example.com" http://ingress-nginx-controller.ingress-nginx/
```

### Common Exam Scenarios
1. **Ingress not working** → Check controller, service, endpoints
2. **404 errors** → Verify service exists and has correct selector
3. **Wrong backend** → Check ingress rules and service names
4. **TLS not working** → Verify secret exists and matches hostname
5. **Path routing issues** → Check pathType and rewrite annotations

### Time-Saving Commands
```bash
# Quick ingress status
k get ing,svc,endpoints

# Quick controller check
k get pods,svc -n ingress-nginx

# Quick test
echo "Testing ingress..." && curl -H "Host: test.local" http://$(k get ing test-ing -o jsonpath='{.status.loadBalancer.ingress[0].ip}')/
```