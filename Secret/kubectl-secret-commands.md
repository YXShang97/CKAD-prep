# CKAD Secret Commands Quick Reference

## What are Secrets?

Secrets store sensitive data (passwords, tokens, keys) in base64 encoded format. Similar to ConfigMaps but for confidential data.

## Create Secrets

```bash
# From literal values
kubectl create secret generic app-secret --from-literal=username=admin --from-literal=password=secret123

# From files
kubectl create secret generic app-secret --from-file=username.txt --from-file=password.txt

# From directory
kubectl create secret generic app-secrets --from-file=secrets/

# Docker registry secret
kubectl create secret docker-registry regcred --docker-server=myregistry.com --docker-username=user --docker-password=pass --docker-email=user@example.com

# TLS secret
kubectl create secret tls tls-secret --cert=path/to/tls.cert --key=path/to/tls.key

# From YAML
kubectl apply -f secret.yaml

# Generate YAML
kubectl create secret generic app-secret --from-literal=key=value --dry-run=client -o yaml > secret.yaml
```

## Get Secret Info

```bash
kubectl get secrets                          # List all secrets
kubectl get secret                           # Short form
kubectl describe secret app-secret           # Detailed info (no values shown)
kubectl get secret app-secret -o yaml        # View YAML (base64 encoded)
kubectl get secret app-secret -o jsonpath='{.data.username}' | base64 -d  # Decode value
```

## Use Secrets in Pods

```bash
# Three ways to inject Secret data:
# 1. Environment variables (individual keys)
# 2. Environment variables (all keys)
# 3. Volume mounts (as files)
```

## Manage Secrets

```bash
kubectl delete secret app-secret             # Delete secret
kubectl delete secret --all                  # Delete all secrets
kubectl edit secret app-secret               # Edit secret (base64 values)

# Extract secret definition
kubectl get secret app-secret -o yaml > secret-definition.yaml
```

## CKAD Exam Tips

- Secrets are base64 encoded, NOT encrypted
- Use Secrets for sensitive data, ConfigMaps for non-sensitive
- Docker registry secrets are automatically used for image pulls
- TLS secrets are used with Ingress resources
- Secret data appears as plain text files when mounted as volumes
- Use `k` alias: `k create secret generic app-secret --from-literal=key=value`
