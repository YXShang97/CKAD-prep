# Horizontal Pod Autoscaling (HPA)

Purpose: concise guide and examples for the CKAD — how to autoscale Deployments using CPU metrics and the Kubernetes HPA.

Contents:

- `deployment.yaml` — sample Deployment with resource requests suitable for HPA
- `hpa.yaml` — HPA targeting the sample Deployment (CPU-based, autoscaling/v1)
- `kubectl-hpa-commands.md` — quick commands to apply, inspect, and test HPA

Requirements / notes:

- A metrics provider (e.g., `metrics-server`) must be installed for CPU-based HPA to work.
- The Deployment must set resource `requests` for CPU (HPA uses requests to compute utilization).

When to use:

- Scale replicas automatically based on observed CPU (or custom) metrics to meet load.

Exam tips:

- Know how to create an HPA, inspect `kubectl get hpa` and `kubectl describe hpa <name>`.
- Practice forcing CPU load in a pod to see replicas increase (e.g., `stress-ng` or a simple busy loop).
