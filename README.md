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

### 1. Official Kubernetes Docs (Most Important)

The only external resource allowed during the exam is **kubernetes.io**. This is the single most important thing to get comfortable with before exam day — not memorizing syntax, but knowing how to find what you need quickly.

The docs search (`site:kubernetes.io`) is unreliable during the exam. **Learn to navigate manually** via the left-hand sidebar and table of contents. The bottom-left search box on the site can help, but don't rely on it. `Ctrl+F` within a page always works and is often faster once you're on the right page.

Key paths to know by hand:

| What you need | Where to find it |
|---|---|
| Pod spec, containers, volumes | Concepts → Workloads → Pods |
| ConfigMap / Secret usage | Tasks → Configure Pods and Containers |
| Resource limits & requests | Tasks → Configure Pods and Containers → Assign Memory/CPU Resources |
| Liveness & readiness probes | Tasks → Configure Pods and Containers → Configure Liveness, Readiness |
| NetworkPolicy | Concepts → Services, Load Balancing, and Networking → Network Policies |
| RBAC (Role, RoleBinding, etc.) | Reference → API Access Control → Using RBAC Authorization |
| PersistentVolume / PVC | Concepts → Storage |
| Multi-container patterns (sidecar, init) | Concepts → Workloads → Pods → Init Containers / Sidecar Containers |
| Rolling updates / Deployment strategies | Concepts → Workloads → Deployments |
| kubectl cheatsheet | Reference → kubectl CLI → kubectl Cheat Sheet |

Spend time during preparation just navigating the docs — not reading, navigating. You want muscle memory for where things live, not just search skills.

### 2. killer.sh (Closest to Real Exam)

When you purchase the CKAD exam, you get **2 free killer.sh simulator sessions**. This is the most realistic exam practice available — same environment, same pressure, harder questions.

- The simulator is intentionally harder than the real exam — scoring 70%+ here is a good signal you're ready
- Each session gives you 36 hours of access to the simulated cluster and full solutions
- Run it under real conditions: timer on, no looking at solutions mid-way
- Review every solution afterward, including ones you got right — the explanations teach better patterns

> Dashboard: https://killer.sh/dashboard

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

- **Browser**: Latest Google Chrome (required for the PSI secure browser download)
- **Monitor**: Single monitor only — dual monitors will get you flagged during the room scan
- **Connection**: Wired internet strongly preferred
- Run the **PSI compatibility check** at least a day before — it installs the secure browser and verifies webcam/mic; don't leave this to the last minute
- Disable antivirus and firewall before the exam starts (they can block the PSI browser)
- Plug in your laptop — draining battery mid-exam is a real risk

### Workspace

The proctor will do a live 360° scan of your room and desk before the exam starts. Make sure:

- Desk is completely clear — no papers, notebooks, phones, or extra devices
- Walls are clear of whiteboards or printed notes (decorative items are fine)
- Good lighting so your face, hands, and desk are clearly visible; no strong backlight behind you
- Private room with the door closed; public spaces are not allowed
- Have your government-issued ID ready — the name must exactly match your exam registration

Check PSI's current candidate handbook for the full list of what is and isn't permitted at your desk (e.g. drinks, food) as policies can change.

### ID

Valid, unexpired government-issued ID with your name, photo, and signature. Passport, driver's license, or national ID card all work.

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

- `kubectl` with `k` alias already configured
- `yq`, `curl`, `wget`, `man`
- Multiple clusters — each question specifies which context to use

### Switching Contexts

Each question specifies which cluster context to use. It's widely recommended to always run the context switch command shown in the question before starting — working on the wrong cluster invalidates your answer.

```bash
kubectl config use-context <context-name>
```

---

## Critical Interface Details

### Copy-Paste

**Right-click → Copy / Paste always works** and is the most reliable method regardless of where you are in the interface. When in doubt, use right-click.

In the Linux terminal, keyboard shortcuts differ from what you might expect:

| Location | Copy | Paste |
|---|---|---|
| **Linux Terminal** | `Ctrl+Shift+C` | `Ctrl+Shift+V` |
| **Firefox / other apps** | `Ctrl+C` | `Ctrl+V` |

Resource names, namespaces, and commands are often shown in the question as blue highlighted text — clicking them copies to clipboard automatically. Use this. **Never hand-type a resource name or command that the question has already given you** — typos cost time and points.

### `Ctrl+W` is Dangerous

`Ctrl+W` closes a browser tab. Inside the terminal, use **`Ctrl+Alt+W`** instead (e.g. to delete the previous word in the shell).

### INSERT Key is Disabled

The `Insert` key is disabled in the Remote Desktop for security reasons. In `vi`/`vim`, use `i` to enter insert mode as normal.

When pasting YAML copied from the official docs into vim, run `:set paste` before entering insert mode — this prevents vim from auto-indenting each line as it's pasted, which breaks YAML indentation.

```
:set paste
i
<paste>
<Esc>
:set nopaste
```

### Breaks

You can pause via the "Pause Exam" button — but **the timer does not stop**. Only step away if you genuinely need to.

### Other Interface Notes

- **Lost cursor**: press `Ctrl+Alt+K` to locate it on the VM desktop
- **Font size**: use `+` / `-` on the PSI browser toolbar to resize the remote desktop view

---

## Time Management

2 hours for 15–20 questions = roughly **6–7 minutes per question**, but questions carry different weights.

- Each question shows its **percentage weight** — skip low-weight questions if stuck, come back later
- Don't sink 15 minutes into a 4% question
- The exam UI lets you flag questions for review — use it
- Aim to finish a first pass with 20–25 minutes left for review

### What to Timebox

- Multi-step debugging (check logs → identify issue → fix manifest → reapply) — set a mental 8-minute cap
- For complex YAML like NetworkPolicy, go straight to the official docs example and adapt it rather than writing from scratch — the structure is non-trivial to recall under pressure

---

## Tips That Actually Matter

**Use `kubectl` imperative commands to generate YAML — don't write from scratch**

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

**Set up shell aliases immediately at exam start**

```bash
export do="--dry-run=client -o yaml"
export now="--force --grace-period 0"
alias k=kubectl
```

**`kubectl explain` when you forget field names**

```bash
kubectl explain pod.spec.containers.resources
kubectl explain networkpolicy.spec.ingress
```

**`kubectl apply -f` vs `kubectl edit`**

Both have their place. `apply -f` with a local file is safer and repeatable — if something breaks, you still have the file to fix and re-apply. `kubectl edit` edits the live resource directly; if the save fails (e.g. invalid YAML), it drops you back into the editor with the error message, which can actually help you identify the problem quickly. Cross-reference the error against the official docs to fix it.

**Verify your work before moving on**

```bash
kubectl get pod <name> -o wide          # check status, node, IP
kubectl describe pod <name>             # check events for errors
kubectl logs <name>                     # check application output
```

**Copy YAML from the official docs, don't reason from scratch**

For complex resources (NetworkPolicy, RBAC, PV/PVC), find the relevant example in the docs, copy the structure, and adapt it. Faster and less error-prone than constructing from memory under time pressure.

---

## Domain Breakdown (for Study Prioritization)

| Domain | Weight |
|---|---|
| Application Environment, Configuration and Security | 25% |
| Application Design and Build | 20% |
| Application Deployment | 20% |
| Services and Networking | 20% |
| Application Observability and Maintenance | 15% |

Highest ROI topics: **RBAC, ConfigMaps/Secrets, resource limits, probes, NetworkPolicy, multi-container pods (sidecar/init), rolling updates**.

---

## Official References

- [Exam Tips (Linux Foundation)](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)
- [Exam UI Documentation](https://docs.linuxfoundation.org/tc-docs/certification/lf-handbook2/exam-user-interface/examui-performance-based-exams)
- [CKAD Curriculum (CNCF)](https://www.cncf.io/training/certification/ckad/)
- [kubectl Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [killer.sh Dashboard](https://killer.sh/dashboard)
