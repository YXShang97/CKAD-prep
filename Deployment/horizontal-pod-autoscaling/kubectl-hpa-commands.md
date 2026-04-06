# HPA quick commands

Apply resources:

```bash
kubectl apply -f Deployment/horizontal-pod-autoscaling/deployment.yaml
kubectl apply -f Deployment/horizontal-pod-autoscaling/hpa.yaml
```

Quick create with `kubectl autoscale` (shorthand):

```bash
kubectl autoscale deployment php-apache --cpu=50% --min=1 --max=10
```

Inspect and debug:

```bash
kubectl get hpa
kubectl describe hpa hpa-demo
kubectl get deploy hpa-demo
kubectl get pods -l app=hpa-demo
```

Test autoscaling (example):

- Option A: use an image that can burn CPU (e.g., `vish/stress`), exec into a pod and trigger load.
- Option B: replace the container with a CPU-busy command for a short test.

Example: exec and run a busy loop (not for production):

```bash
kubectl exec -it $(kubectl get pods -l app=hpa-demo -o name | head -n1) -- /bin/sh -c "while true; do yes > /dev/null & sleep 10; done"
```

Then watch:

```bash
kubectl get hpa -w
kubectl get pods -l app=hpa-demo
```

Cleanup:

```bash
kubectl delete -f Deployment/horizontal-pod-autoscaling/hpa.yaml
kubectl delete -f Deployment/horizontal-pod-autoscaling/deployment.yaml
```
