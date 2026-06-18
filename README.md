# CKAD Exam Prep Guide

> Passed June 2026. This is a practical guide focused on what actually matters for the exam — environment, interface quirks, and the mindset shift you need going in. Assumes you already know what CKAD is.

---

## Table of Contents

- [Exam Basics](#exam-basics)
- [Preparation Resources](#preparation-resources)
- [Before Exam Day](#before-exam-day)
- [The Exam Environment](#the-exam-environment)
- [Critical Interface Details](#critical-interface-details)
- [The Biggest Mindset Shift](#the-biggest-mindset-shift)
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

### 1. Official Kubernetes Docs — Your Most Important Tool

The only external resource allowed during the exam is **kubernetes.io**. Getting fast at navigating it is not optional — it's the skill that separates prepared candidates from struggling ones.

**Don't rely on search.** The in-site search is unreliable during the exam. Learn to navigate manually via the left sidebar and table of contents. The search box in the bottom-left of the site can help, but treat it as a fallback. `Ctrl+F` within a page is fast and always works once you're in the right section.

The goal during prep: build **muscle memory for where things live**, not just the ability to search. You want to open Firefox during the exam and go directly to the right page, not spend two minutes hunting.

**Key paths to know by hand:**

| What you need | Path |
|---|---|
| Pod spec, containers, volumes | Concepts → Workloads → Pods |
| ConfigMap / Secret usage | Tasks → Configure Pods and Containers |
| Resource limits & requests | Tasks → Configure Pods and Containers → Assign Memory/CPU Resources |
| Liveness & readiness probes | Tasks → Configure Pods and Containers → Configure Liveness, Readiness |
| NetworkPolicy | Concepts → Services, Load Balancing, and Networking → Network Policies |
| RBAC | Reference → API Access Control → Using RBAC Authorization |
| PersistentVolume / PVC | Concepts → Storage |
| Init / Sidecar containers | Concepts → Workloads → Pods → Init Containers / Sidecar Containers |
| Deployments, rolling updates | Concepts → Workloads → Deployments |
| kubectl cheatsheet | Reference → kubectl CLI → kubectl Cheat Sheet |

### 2. killer.sh — Closest Thing to the Real Exam

Buying the CKAD exam includes **2 free killer.sh simulator sessions**. Use both. This is the most realistic practice available — same interface, same time pressure, harder questions.

- Intentionally harder than the real exam; scoring 70%+ here means you're ready
- 36 hours of access per session, including full solutions
- Simulate real conditions: timer on, no peeking at solutions mid-way
- Review every solution afterward — even the ones you got right

> https://killer.sh/dashboard

### Other Resources (Pre-Exam Study)

- **[Udemy – Mumshad Mannambeth's CKAD course](https://www.udemy.com/course/certified-kubernetes-application-developer/)**: the standard starting point; covers all domains with built-in KodeKloud labs
- **[KodeKloud – CKAD course](https://kodekloud.com/courses/certified-kubernetes-application-developer-ckad)**: browser-based labs; good for hands-on reps without local setup
- **O'Reilly** (subscription required):
  - [CKAD, 4th Edition – Video by Sander van Vugt](https://www.oreilly.com/library/view/certified-kubernetes-application/9780135349700/): 12+ hours covering all exam domains; each lesson ends with a lab
  - [Video Prep Course by Benjamin Muschko](https://www.oreilly.com/library/view/certified-kubernetes-application/9781492061045/): concise walkthrough with exam tips from personal experience
  - [Learning Path: CKAD Prep Course](https://www.oreilly.com/learning-paths/learning-path-certified/9781492061021/): bundles video + interactive labs in a guided sequence
  - [CKAD Exam Prep Labs (Playlist)](https://learning.oreilly.com/playlists/2e9fe6dc-2a05-47fe-ae0a-34d6485287cc/): standalone hands-on labs per exam domain
- [`kubectl` cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

---

## Before Exam Day

### Technical Setup

- **Browser**: Chrome required for the PSI secure browser installation
- **Monitor**: Single monitor only — dual monitors will get you flagged during the room scan
- **Connection**: Wired internet strongly preferred
- Run the **PSI compatibility check** at least a day before — it installs the secure browser and verifies webcam/mic
- Disable antivirus and firewall before starting (they can block the PSI browser)
- Plug in your laptop

### Workspace

The proctor does a live 360° scan of your room and desk before the exam. Prepare:

- Desk completely clear — no papers, notebooks, phones, extra devices
- Walls clear of whiteboards or printed notes (decorative items are fine)
- Good lighting so your face, hands, and desk are clearly visible; no strong backlight
- Private room, door closed
- Government-issued ID ready — name must exactly match your exam registration

> For the current full list of permitted items (drinks, food, etc.), check the [PSI candidate handbook](https://www.linuxfoundation.org/legal/candidates) — policies can change.

---

## The Exam Environment

The exam runs in a **Remote Desktop (VM)** inside the PSI secure browser. You're not working on your local machine.

```
PSI Secure Browser
└── ExamUI
    ├── ReadMe tab       ← question list and instructions
    └── Remote Desktop   ← your workspace (VM)
        ├── Terminal
        ├── Firefox (for kubernetes.io)
        └── VSCodium (optional)
```

**Pre-installed**: `kubectl` (with `k` alias), `yq`, `curl`, `wget`, `man`

---

## Critical Interface Details

### Copy-Paste

> **Right-click → Copy / Paste is the most reliable method everywhere in the interface.** Use it by default.

Keyboard shortcuts work too, but they differ by context:

| Location | Copy | Paste |
|---|---|---|
| Linux Terminal | `Ctrl+Shift+C` | `Ctrl+Shift+V` |
| Firefox / other apps | `Ctrl+C` | `Ctrl+V` |

**One more thing**: resource names, namespaces, and commands in the question are shown as blue highlighted text — click them to copy directly to clipboard. **Never hand-type something the question has already given you.** One typo on a resource name wastes time and fails the task.

### `Ctrl+W` is Dangerous

`Ctrl+W` closes a browser tab. In the terminal, use **`Ctrl+Alt+W`** instead (e.g. to delete the previous word).

### Pasting YAML from the Docs into vim

The `Insert` key is disabled in the Remote Desktop. Always use `i` to enter insert mode.

More importantly: **when pasting YAML copied from the official docs, always enable paste mode first** — otherwise vim auto-indents each line as it arrives, corrupting the indentation.

```vim
:set paste    ← run this first
i             ← enter insert mode
              ← paste your YAML (Ctrl+Shift+V)
<Esc>
:set nopaste  ← restore normal behaviour
```

Skip this step and your YAML will look fine visually but fail silently due to broken indentation.

### Breaks

You can pause via the "Pause Exam" button — but **the timer keeps running**. Step away only if you genuinely need to.

### Minor but Useful

- **Lost cursor**: `Ctrl+Alt+K` to locate it on the VM desktop
- **Font size**: `+` / `-` on the PSI browser toolbar resizes the remote desktop view

---

## The Biggest Mindset Shift

> **Courses teach concepts one at a time. The exam tests them together.**

During prep — whether Udemy, KodeKloud, or O'Reilly labs — each exercise is scoped to one or two concepts. You practice NetworkPolicy. Then you practice Services. Then Deployments. Each isolated.

The exam doesn't work that way. A single question might give you a broken application stack and ask you to fix it — which requires you to trace the relationship between an Ingress, a Service, and a Deployment, identify where the chain breaks, and resolve it. You need to hold the full picture in your head and move fluidly between objects.

Similarly, a Deployment question might not just ask you to update an image — it might combine:
- modifying the Pod spec (image, env vars, resource limits)
- checking rollout status
- inspecting rollout history
- rolling back with `undo`
- scaling replicas

If you've only ever practiced each of these in isolation, the compound question feels unfamiliar even when you know every individual piece.

**What to do about it**: once you've covered the core concepts, stop doing single-topic labs and start practicing end-to-end scenarios. killer.sh is the best source of these. When reviewing solutions, always ask: *what other objects does this resource depend on, and how would a failure in each one show up?*

---

## Time Management

2 hours, 15–20 questions. Each question shows its percentage weight — use it.

- **Skip and flag** low-weight questions you're stuck on; come back after the first pass
- Don't spend 15 minutes on a 4% question
- Aim to finish the first pass with 20–25 minutes remaining for review

**Timebox multi-step tasks.** Debugging chains (check logs → find issue → fix manifest → verify) can eat time silently. Set a mental cap and move on if you're not making progress.

**For complex YAML** (NetworkPolicy, RBAC, PV/PVC): go straight to the official docs example and adapt it. Don't reconstruct from memory.

---

## Tips That Actually Matter

**Generate YAML imperatively — never write from scratch**

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
kubectl create deployment my-dep --image=nginx --replicas=3 --dry-run=client -o yaml > dep.yaml
kubectl expose pod nginx --port=80 --dry-run=client -o yaml > svc.yaml
kubectl create configmap my-config --from-literal=key=value --dry-run=client -o yaml
kubectl create serviceaccount my-sa --dry-run=client -o yaml
```

**Set up aliases at exam start**

```bash
alias k=kubectl
export do="--dry-run=client -o yaml"
export now="--force --grace-period 0"
```

**Use `kubectl explain` when you forget field names**

```bash
kubectl explain pod.spec.containers.resources
kubectl explain networkpolicy.spec.ingress
```

**Always verify your work before moving on**

Don't assume it worked. A task that runs without error isn't necessarily correct.

```bash
kubectl get pod <name> -o wide        # is it Running? on the right node?
kubectl describe pod <name>           # any warning events?
kubectl logs <name>                   # is the app actually working?
kubectl exec <name> -- <command>      # test connectivity or behaviour directly
```

**`kubectl apply -f` vs `kubectl edit`**

Both are valid. `apply -f` with a local file is safer and repeatable — if something breaks, fix the file and re-apply. `kubectl edit` modifies the live resource directly; if the save fails due to invalid YAML or a rejected field, it drops you back into the editor with the error message inline — which is actually useful for diagnosing the problem. Cross-reference the error with the official docs and fix from there.

---

## Domain Breakdown

| Domain | Weight |
|---|---|
| Application Environment, Configuration and Security | 25% |
| Application Design and Build | 20% |
| Application Deployment | 20% |
| Services and Networking | 20% |
| Application Observability and Maintenance | 15% |

**Highest ROI topics**: RBAC, ConfigMaps/Secrets, resource limits, liveness/readiness probes, NetworkPolicy, multi-container pods (init + sidecar), Deployments (rollout, history, undo, scale).
