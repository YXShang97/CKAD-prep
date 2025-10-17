# Kubernetes Cluster Components

## Cluster Architecture Overview

```
Master Node (Control Plane)          Worker Nodes
┌─────────────────────────┐          ┌──────────────────────┐
│  API Server             │          │  kubelet             │
│  etcd                   │   ◄────► │  kube-proxy          │
│  Controller Manager     │          │  Container Runtime   │
│  Scheduler              │          │  Pod                 │
└─────────────────────────┘          └──────────────────────┘
```

## Master Node Components (Control Plane)

### API Server (`kube-apiserver`)

- **Purpose**: Front-end for Kubernetes control plane
- **Function**: Handles all REST operations, authentication, authorization
- **Interaction**: All components communicate through API Server

### etcd

- **Purpose**: Distributed key-value store
- **Function**: Stores all cluster data (configuration, state, metadata)
- **Characteristics**: Consistent, highly-available

### Controller Manager (`kube-controller-manager`)

- **Purpose**: Runs controller processes
- **Function**:
  - Node Controller: Monitors node health
  - Replication Controller: Maintains desired pod replicas
  - Endpoints Controller: Manages service endpoints
  - Service Account Controller: Creates default accounts

### Scheduler (`kube-scheduler`)

- **Purpose**: Assigns pods to nodes
- **Function**:
  - Watches for newly created pods with no assigned node
  - Selects optimal node based on resources, constraints, policies

## Worker Node Components

### kubelet

- **Purpose**: Node agent
- **Function**:
  - Communicates with API Server
  - Manages pods and containers on the node
  - Reports node and pod status
  - Executes container probes

### kube-proxy

- **Purpose**: Network proxy
- **Function**:
  - Maintains network rules for services
  - Handles load balancing for services
  - Implements service abstraction

### Container Runtime

- **Purpose**: Runs containers
- **Options**: Docker, containerd, CRI-O
- **Function**: Pulls images, starts/stops containers

## Add-on Components

### DNS (CoreDNS)

- **Purpose**: Cluster DNS service
- **Function**: Provides name resolution for services and pods

### Dashboard

- **Purpose**: Web-based UI
- **Function**: Cluster management interface

### Ingress Controller

- **Purpose**: Manages external access
- **Function**: HTTP/HTTPS routing to services

## Component Communication Flow

1. **kubectl** → **API Server** (user requests)
2. **API Server** → **etcd** (store/retrieve data)
3. **Controller Manager** → **API Server** (watch resources)
4. **Scheduler** → **API Server** (pod placement decisions)
5. **kubelet** → **API Server** (register node, report status)
6. **kube-proxy** → **API Server** (watch services/endpoints)

## CKAD Relevance

### What You Need to Know:

- **Pod lifecycle**: kubelet manages pods
- **Service networking**: kube-proxy handles service routing
- **Resource management**: Controllers maintain desired state
- **Scheduling**: How pods get placed on nodes

## Key Concepts for Exam

### Master Node = Control Plane

- Makes global decisions (scheduling, scaling)
- Stores cluster state
- Provides API interface

### Worker Nodes = Compute

- Run application workloads (pods)
- Execute containers
- Handle networking

### Controllers Ensure Desired State

- Deployments → ReplicaSets → Pods
- Services maintain network abstraction
- ConfigMaps/Secrets provide configuration

## Quick Reference

| Component          | Location | Primary Function       |
| ------------------ | -------- | ---------------------- |
| API Server         | Master   | REST API frontend      |
| etcd               | Master   | Data store             |
| Scheduler          | Master   | Pod placement          |
| Controller Manager | Master   | Maintain desired state |
| kubelet            | Worker   | Pod lifecycle          |
| kube-proxy         | Worker   | Service networking     |
| Container Runtime  | Worker   | Run containers         |
