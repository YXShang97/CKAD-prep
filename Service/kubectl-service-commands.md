# CKAD Service Commands Quick Reference

## What are Services?

Services provide stable network endpoints for pods. They abstract pod IPs and provide load balancing.

## Three Service Types

### 1. ClusterIP (Default)

- **Purpose**: Internal cluster communication only
- **Access**: Only from within the cluster
- **Use Case**: Database services, internal APIs

### 2. NodePort

- **Purpose**: External access through node ports
- **Access**: `<NodeIP>:<NodePort>` from outside cluster
- **Use Case**: Development, testing, simple external access

### 3. LoadBalancer

- **Purpose**: External access through cloud load balancer
- **Access**: External IP provided by cloud provider
- **Use Case**: Production external services

## Create Services

```bash
# ClusterIP (default)
kubectl expose pod nginx --port=80 --target-port=8080
kubectl create service clusterip nginx --tcp=80:8080

# NodePort
kubectl expose pod nginx --type=NodePort --port=80
kubectl create service nodeport nginx --tcp=80:8080

# LoadBalancer
kubectl expose deployment nginx --type=LoadBalancer --port=80
kubectl create service loadbalancer nginx --tcp=80:8080

# From YAML
kubectl apply -f service.yaml

# Generate YAML
kubectl expose pod nginx --port=80 --dry-run=client -o yaml > service.yaml
```

## Get Service Info

```bash
kubectl get services                                 # List all services
kubectl get svc                                     # Short form
kubectl get svc -o wide                             # More details
kubectl describe service nginx                      # Detailed info
kubectl get endpoints nginx                         # Service endpoints (pod IPs)
kubectl get svc --show-labels                      # Show labels
```

## Test Service Connectivity

```bash
# From within cluster
kubectl run test-pod --image=busybox -it --rm -- sh
# Inside pod: wget -qO- http://service-name:port

# Test specific service
kubectl exec -it test-pod -- wget -qO- http://nginx:80

# Port forward for local testing
kubectl port-forward service/nginx 8080:80
# Then access: http://localhost:8080
```

## Manage Services

```bash
kubectl delete service nginx                        # Delete service
kubectl delete svc --all                           # Delete all services
kubectl edit service nginx                         # Edit service

# Extract service definition
kubectl get service nginx -o yaml > service-definition.yaml
```

## Service Discovery

```bash
# DNS resolution within cluster
service-name                    # Same namespace
service-name.namespace         # Different namespace
service-name.namespace.svc.cluster.local  # Full FQDN

# Environment variables (automatic)
echo $NGINX_SERVICE_HOST       # Service IP
echo $NGINX_SERVICE_PORT       # Service port
```

## CKAD Exam Tips

- **Port vs TargetPort**: `port` is service port, `targetPort` is pod port
- **Default TargetPort**: If not specified, defaults to `port` value
- **NodePort Range**: 30000-32767 (auto-assigned if not specified)
- **Selectors**: Must match pod labels for service to work
- **Endpoints**: Check with `kubectl get endpoints` if service not working
- Use `k` alias: `k get svc`, `k expose pod nginx --port=80`
