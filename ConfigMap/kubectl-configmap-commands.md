# CKAD ConfigMap Commands Quick Reference

## What are ConfigMaps?

ConfigMaps store non-sensitive configuration data as key-value pairs. Used to decouple configuration from application code.

## Create ConfigMaps

```bash
# From literal values
kubectl create configmap app-config --from-literal=database_host=mysql --from-literal=database_port=3306

# From file
kubectl create configmap nginx-config --from-file=nginx.conf

# From directory (all files in directory)
kubectl create configmap app-configs --from-file=config/

# From environment file (.env format)
kubectl create configmap env-config --from-env-file=app.env

# From YAML
kubectl apply -f configmap.yaml

# Generate YAML
kubectl create configmap app-config --from-literal=key=value --dry-run=client -o yaml > configmap.yaml
```

## Get ConfigMap Info

```bash
kubectl get configmaps                          # List all ConfigMaps
kubectl get cm                                  # Short form
kubectl describe configmap app-config           # Detailed info
kubectl get cm app-config -o yaml               # View YAML
kubectl get cm --show-labels                    # Show labels
```

## Use ConfigMaps in Pods

```bash
# Three ways to inject ConfigMap data:
# 1. Environment variables (individual keys)
# 2. Environment variables (all keys)
# 3. Volume mounts (as files)
```

## Manage ConfigMaps

```bash
kubectl delete configmap app-config             # Delete ConfigMap
kubectl delete cm --all                         # Delete all ConfigMaps
kubectl edit configmap app-config               # Edit ConfigMap

# Extract ConfigMap definition
kubectl get configmap app-config -o yaml > configmap-definition.yaml
```

## CKAD Exam Tips

- ConfigMaps store non-sensitive data (use Secrets for sensitive data)
- Pods can reference ConfigMaps that don't exist yet (use `optional: true`)
- Changes to ConfigMaps don't automatically restart pods
- ConfigMap keys must be valid environment variable names for env injection
- Use volume mounts for configuration files
- Use `k` alias: `k create cm app-config --from-literal=key=value`
