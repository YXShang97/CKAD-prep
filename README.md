# CKAD Exam Prep Guide

> Passed June 2026. This is a practical guide focused on the exam experience itself — environment, interface quirks, and what actually helps on exam day. Assumes you already know what CKAD is.

---

## Table of Contents

- [Exam Basics](#exam-basics)
- [Preparation Resources](#preparation-resources)
- [Before Exam Day](#before-exam-day)
- [The Exam Environment](#the-exam-environment)
- [Critical Interface Details](#critical-interface-details)
- [Time Management](#time-management)
- [Tips That Actually Matter](#tips-that-actually-matter)

---

## Exam Basics

| | |
|---|---|
| **Format** | 15–20 performance-based tasks |
| **Duration** | 2 hours |
| **Passing score** | 66% |
| **Kubernetes version** | v1.35 |
| **Delivery** | Online, remotely proctored via PSI Bridge |
| **Results** | Within 24 hours by email |
| **Retake** | 1 free retake included |

---

## Preparation Resources

### killer.sh (Most Important)

When you purchase the CKAD exam through Linux Foundation, you get **2 free killer.sh simulator sessions** — use them. This is the closest thing to the real exam environment.

- The simulator is **harder than the real exam** by design — if you can score 70%+ here, you're ready
- Each session gives you 36 hours of access to the simulated cluster and solutions
- Run the session under real exam conditions: timer on, no peeking at solutions mid-way
- Review every solution afterward, including ones you got right — the explanations teach better patterns

> Dashboard: https://killer.sh/dashboard

### Official Kubernetes Docs

The only external resource you're allowed during the exam is **kubernetes.io**. Know how to navigate it fast:

- Bookmark your most-used pages before the exam (bookmarks carry into the exam browser)
- Key sections: Tasks → Configure Pods, Concepts → Workloads, Reference → kubectl CLI
- Use `Ctrl+F` to search within pages — faster than reading top to bottom
- Practice finding things by **searching the docs**, not memorizing syntax

### Other Resources (Pre-Exam Only)

- **[Udemy – Mumshad Mannambeth's CKAD course](https://www.udemy.com/course/certified-kubernetes-application-developer/)**: the standard starting point; covers all domains with built-in KodeKloud labs
- **[KodeKloud – CKAD course](https://kodekloud.com/courses/certified-kubernetes-application-developer-ckad)**: browser-based labs; good for hands-on reps without local setup
- **O'Reilly** (if you have a subscription):
  - [CKAD, 4th Edition – Video by Sander van Vugt](https://www.oreilly.com/library/view/certified-kubernetes-application/9780135349700/): 12+ hours covering all exam domains; each lesson ends with a lab
  - [Video Prep Course by Benjamin Muschko](https://www.oreilly.com/library/view/certified-kubernetes-application/9781492061045/): more concise walkthrough with exam tips from personal experience
  - [Learning Path: CKAD Prep Course](https://www.oreilly.com/learning-paths/learning-path-certified/9781492061021/): bundles video + interactive labs in a guided sequence
  - [CKAD Exam Prep Labs (Playlist)](https://learning.oreilly.com/playlists/2e9fe6dc-2a05-47fe-ae0a-34d6485287cc/): standalone hands-on labs per exam domain
- `kubectl` cheatsheet: https://kubernetes.io/docs/reference/kubectl/cheatsheet/

---

## Before Exam Day

### Technical Setup

- **Browser**: Latest Google Chrome (required)
- **Monitor**: Single monitor only — dual monitors will get you flagged
- **Connection**: Wired internet strongly preferred
- Run the **PSI compatibility check** at least a day before: installs the secure browser, checks webcam/mic
- Disable antivirus and firewall during the exam (they can block the PSI browser)
- Plug in your laptop — 2 hours on battery is risky

### Your Environment

- Clear desk: no papers, devices, water bottles with labels, anything that could look like notes
- Good lighting — proctor needs to see your face and hands clearly
- Private room, door closed
- Government-issued ID ready (name must match your registration exactly)

### Pages Worth Knowing by Heart (or Fast Search)

The exam VM has Firefox for docs access. Know how to quickly navigate to:

- kubectl cheatsheet
- Pod spec reference
- NetworkPolicy examples
- RBAC (Role, RoleBinding, ClusterRole, ClusterRoleBinding)
- PersistentVolume / PVC
- Resource limits / requests
- Liveness & readiness probes

---

## The Exam Environment

The exam runs in a **Remote Desktop (VM)** inside your browser. You're not working on your local machine.

```
Browser (PSI Secure Browser)
└── ExamUI
    ├── ReadMe tab       ← exam instructions, question list
    └── Remote Desktop   ← your actual workspace (VM)
        ├── Terminal emulator
        ├── Firefox (for docs)
        └── VSCodium (optional, has integrated terminal)
```

### What's Pre-installed

- `kubectl`, `k` alias already set up
- `yq`, `curl`, `wget`, `man`
- Multiple clusters — each question tells you which context to switch to

### Switching Contexts

Every question header gives you the exact `kubectl config use-context` command. **Always run it** before starting a question, even if you think you're already on the right cluster. Getting this wrong invalidates everything you do.

```bash
kubectl config use-context <context-name>
```

---

## Critical Interface Details

### Copy-Paste (Do Not Get This Wrong)

This trips people up constantly. The shortcuts differ by where you are:

| Location | Copy | Paste |
|---|---|---|
| **Linux Terminal** | `Ctrl+Shift+C` | `Ctrl+Shift+V` |
| **Firefox / other apps** | `Ctrl+C` | `Ctrl+V` |
| Right-click menu also works in both |

You can also copy question text directly from the exam console and paste it into the terminal with `Ctrl+Shift+V`. This matters for long resource names — don't hand-type them.

### `Ctrl+W` is Dangerous

In Chrome, `Ctrl+W` closes a tab. Inside the exam terminal, use **`Ctrl+Alt+W`** instead (e.g., to delete a word backward in the shell). The exam docs call this out explicitly.

### INSERT Key is Disabled

In `vi`/`vim`, don't use the `Insert` key — it's blocked. Use `i` to enter insert mode as normal.

### Lost Your Cursor?

If your cursor disappears on the VM desktop, press **`Ctrl+Alt+K`** to locate it.

### Font Size

Zoom via the browser toolbar (`Ctrl+` / `Ctrl-`) — this resizes the entire remote desktop view. Adjust early so you're not squinting during the exam.

### Breaks

You can request a break via the "Pause Exam" button — but **the timer does not stop**. Only use it if you genuinely need to step away.

---

## Time Management

2 hours for 15–20 questions = roughly **6–7 minutes per question**. But questions are weighted differently.

- Each question shows its **percentage weight** — skip low-weight questions if stuck, come back later
- Don't spend 15 minutes on a 4% question
- Flag and move on: the exam UI lets you mark questions for review
- Aim to finish a first pass with 20–25 minutes remaining for review

### What to Timebox Hard

- Anything involving multi-step debugging (check logs → find issue → fix manifest → reapply) — set a mental 8-minute cap
- NetworkPolicy questions — the YAML is fiddly; use the docs example as a template

---

## Tips That Actually Matter

**Use `kubectl` imperative commands to generate YAML, don't write from scratch**

```bash
# Generate a pod manifest
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml

# Generate a deployment
kubectl create deployment my-dep --image=nginx --replicas=3 --dry-run=client -o yaml > dep.yaml

# Generate a service
kubectl expose pod nginx --port=80 --dry-run=client -o yaml > svc.yaml

# Create a configmap
kubectl create configmap my-config --from-literal=key=value --dry-run=client -o yaml

# Create a serviceaccount
kubectl create serviceaccount my-sa --dry-run=client -o yaml
```

**Set up your shell aliases immediately at exam start**

```bash
export do="--dry-run=client -o yaml"
export now="--force --grace-period 0"
alias k=kubectl
```

**`kubectl explain` is your friend when you forget field names**

```bash
kubectl explain pod.spec.containers.resources
kubectl explain networkpolicy.spec.ingress
```

**Verify your work before moving on**

```bash
kubectl get pod <name> -o wide          # check status, node, IP
kubectl describe pod <name>             # check events for errors
kubectl logs <name>                     # check application output
```

**Don't edit live resources if you can avoid it** — `kubectl apply -f` with a file is safer and repeatable. If something breaks, you can re-apply.

**For tricky questions, check if there's a similar example in the official docs** — copy the YAML structure, modify it. Faster than reasoning from scratch under time pressure.

---

## Domain Breakdown (for Study Prioritization)

| Domain | Weight |
|---|---|
| Application Design and Build | 20% |
| Application Deployment | 20% |
| Application Observability and Maintenance | 15% |
| Application Environment, Configuration and Security | 25% |
| Services and Networking | 20% |

Highest ROI topics: **RBAC, ConfigMaps/Secrets, resource limits, probes, NetworkPolicy, multi-container pods (sidecar/init), rolling updates**.

---

## Official References

- [Exam Tips (Linux Foundation)](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)
- [Exam UI Documentation](https://docs.linuxfoundation.org/tc-docs/certification/lf-handbook2/exam-user-interface/examui-performance-based-exams)
- [CKAD Curriculum](https://github.com/cncf/curriculum)
- [kubectl Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [killer.sh Dashboard](https://killer.sh/dashboard)
