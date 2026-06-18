# CKAD Exam Prep Guide

> Passed June 2026 · Practical guide focused on what actually matters for the exam — environment, interface quirks, and the mindset shift you need going in. Written for candidates who already know what CKAD is.

---

## Contents

- [Exam Basics](#exam-basics)
- [Preparation Resources](#preparation-resources)
- [Before Exam Day](#before-exam-day)
- [The Exam Environment](#the-exam-environment)
- [Critical Interface Details](#critical-interface-details)
- [The Biggest Mindset Shift](#the-biggest-mindset-shift)
- [Time Management](#time-management)
- [Tips That Actually Matter](#tips-that-actually-matter)
- [Domain Breakdown](#domain-breakdown)

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

The only external resource allowed during the exam is **[kubernetes.io](https://kubernetes.io/docs/home/)**. Getting fast at navigating it is not optional — it's the skill that separates prepared candidates from struggling ones.

> [!WARNING]
> **Don't rely on search.** The in-site search is unreliable during the exam. Learn to navigate manually via the left sidebar and table of contents. `Ctrl+F` within a page is fast and always works once you're in the right section.

The goal during prep: build **muscle memory for where things live**. You want to open Firefox during the exam and go directly to the right page, not spend two minutes hunting.

**Key paths to know:**

| Topic | Path |
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
| kubectl cheatsheet | Reference → kubectl CLI → [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) |

Spend time during preparation just **navigating** the docs — not reading, navigating. Go to a topic, find it manually, go back, find it again faster. That's the practice.

---

### 2. killer.sh — Closest Thing to the Real Exam

Buying the CKAD exam includes **2 free killer.sh simulator sessions**. Use both. This is the most realistic practice available — same interface, same time pressure, harder questions.

> [!TIP]
> The simulator is intentionally harder than the real exam. Scoring 70%+ here is a strong signal you're ready.

- 36 hours of cluster access per session, including full solutions
- Run it under real conditions: timer on, no peeking at solutions mid-way
- Review every solution afterward — even the ones you got right; the explanations teach better patterns

🔗 https://killer.sh/dashboard

---

### Other Resources (Pre-Exam Study)

| Resource | Notes |
|---|---|
| [Udemy – Mumshad Mannambeth's CKAD course](https://www.udemy.com/course/certified-kubernetes-application-developer/) | Standard starting point; covers all domains with KodeKloud labs built in |
| [KodeKloud – CKAD course](https://kodekloud.com/courses/certified-kubernetes-application-developer-ckad) | Browser-based labs; great for reps without local setup |

**O'Reilly** (subscription required):

| Resource | Notes |
|---|---|
| [CKAD, 4th Ed. – Video by Sander van Vugt](https://www.oreilly.com/library/view/certified-kubernetes-application/9780135349700/) | 10+ hours covering all domains; each lesson ends with a graded lab |
| [Video Prep Course by Benjamin Muschko](https://www.oreilly.com/library/view/certified-kubernetes-application/0642572045296/) | Concise walkthrough with exam tips from personal experience |
| [Learning Path: CKAD Prep Course](https://www.oreilly.com/learning-paths/learning-path-certified/9781492061021/) | Bundles video + interactive labs in a guided sequence |
| [CKAD Exam Prep Labs (Playlist)](https://learning.oreilly.com/playlists/2e9fe6dc-2a05-47fe-ae0a-34d6485287cc/) | Standalone hands-on labs per exam domain |

---

## Before Exam Day

### Technical Setup

- **Browser**: Chrome required for the PSI secure browser installation
- **Monitor**: Single monitor only — dual monitors will get you flagged during the room scan
- **Connection**: Wired internet strongly preferred
- Run the **PSI compatibility check** at least a day before — installs the secure browser and verifies webcam/mic
- Disable antivirus and firewall before starting (they can block the PSI browser)
- Plug in your laptop

### Workspace

The proctor does a live 360° scan of your room and desk before the exam.

> [!IMPORTANT]
> Desk must be completely clear — no papers, notebooks, phones, or extra devices. Walls must be free of whiteboards or printed notes (decorative items are fine). No drinks other than clear liquid in a label-free clear bottle or glass.

- Good lighting — your face, hands, and desk must be clearly visible; no strong backlight
- Private room, door closed; public spaces are not allowed
- Government-issued ID ready — name must exactly match your exam registration (passport, driver's license, or national ID)

For the full current list of permitted items, see the [Linux Foundation exam rules](https://docs.linuxfoundation.org/tc-docs/certification/lf-handbook2/exam-rules-and-policies).

---

## The Exam Environment

The exam runs in a **Remote Desktop (VM)** inside the PSI secure browser. You are not working on your local machine.

> [!TIP]
> Familiarise yourself with the exam UI before exam day — the official docs cover the interface in detail:
> - [Exam Tips & Instructions (CKA/CKAD)](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)
> - [Exam UI Documentation](https://docs.linuxfoundation.org/tc-docs/certification/lf-handbook2/exam-user-interface/examui-performance-based-exams)

```
PSI Secure Browser
└── ExamUI
    ├── ReadMe tab       ← question list and instructions
    └── Remote Desktop   ← your workspace (VM)
        ├── Terminal
        ├── Firefox (kubernetes.io only)
        └── VSCodium (optional)
```

**Pre-installed**: `kubectl` (with `k` alias configured), `yq`, `curl`, `wget`, `man`

---

## Critical Interface Details

### Copy-Paste

> [!IMPORTANT]
> **Right-click → Copy / Paste is the most reliable method everywhere in the interface.** Use it by default — it always works.

Keyboard shortcuts also work but differ by context:

| Location | Copy | Paste |
|---|---|---|
| Linux Terminal | `Ctrl+Shift+C` | `Ctrl+Shift+V` |
| Firefox / other apps | `Ctrl+C` | `Ctrl+V` |

Resource names, namespaces, and commands in the question text appear as **blue highlighted text — clicking them copies to clipboard automatically**. Use this every time. Never hand-type something the question has already given you. One typo on a resource name wastes time and fails the task.

---

### `Ctrl+W` is Dangerous

`Ctrl+W` closes a browser tab. Inside the terminal, use **`Ctrl+Alt+W`** instead (e.g. to delete the previous word in the shell).

---

### Pasting YAML from the Docs into vim

The `Insert` key is disabled in the Remote Desktop — use `i` to enter insert mode.

> [!WARNING]
> **Always enable paste mode before pasting YAML into vim.** Without it, vim auto-indents each line as it arrives, silently breaking your YAML indentation. The file looks fine visually but will fail on apply.

```vim
:set paste       ← run this before pasting
i                ← enter insert mode
                 ← paste with Ctrl+Shift+V
<Esc>
:set nopaste     ← restore normal behaviour
```

---

### Breaks

You can pause via the "Pause Exam" button — but **the timer keeps running**. Step away only if you genuinely need to.

---

### Minor but Useful

| | |
|---|---|
| **Lost cursor** | `Ctrl+Alt+K` — locates the cursor on the VM desktop |
| **Font size** | `+` / `-` on the PSI browser toolbar resizes the remote desktop view |

---

## The Biggest Mindset Shift

> **Courses teach concepts one at a time. The exam tests them together.**

During prep — Udemy, KodeKloud, O'Reilly labs — each exercise is scoped to one or two concepts. You practice NetworkPolicy. Then Services. Then Deployments. Each in isolation.

**The exam doesn't work that way.** A single question might give you a broken application stack and ask you to fix it. That requires tracing the relationship between an Ingress, a Service, and a Deployment — identifying where the chain breaks, and resolving it — while holding the full picture in your head.

Similarly, a Deployment question might combine all of the following in one task:

- Modifying the Pod spec (image, env vars, resource limits)
- Checking rollout status
- Inspecting rollout history
- Rolling back with `undo`
- Scaling replicas

If you've only ever practiced each in isolation, the compound question feels unfamiliar even when you know every individual piece.

**What to do about it**: once you've covered the core concepts, stop doing single-topic labs. Start practicing end-to-end scenarios — killer.sh is the best source. When reviewing solutions, ask yourself: *what other objects does this resource depend on, and how would a failure in each one appear?*

---

## Time Management

2 hours, 15–20 questions. Each question displays its percentage weight — use it.

| Strategy | Detail |
|---|---|
| **Skip and flag** | Don't get stuck on low-weight questions; flag and return after the first pass |
| **Hard timebox** | Set a mental cap on multi-step debugging chains — move on if you're not progressing |
| **First-pass target** | Finish with 20–25 minutes remaining for review |

For complex YAML (NetworkPolicy, RBAC, PV/PVC): go straight to the official docs example and adapt it. Don't reconstruct from memory under pressure.

---

## Tips That Actually Matter

### Generate YAML imperatively — never write from scratch

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
kubectl create deployment my-dep --image=nginx --replicas=3 --dry-run=client -o yaml > dep.yaml
kubectl expose pod nginx --port=80 --dry-run=client -o yaml > svc.yaml
kubectl create configmap my-config --from-literal=key=value --dry-run=client -o yaml
kubectl create serviceaccount my-sa --dry-run=client -o yaml
```

### Set up aliases immediately at exam start

```bash
alias k=kubectl
export do="--dry-run=client -o yaml"
export now="--force --grace-period 0"
```

### Use `kubectl explain` when you forget field names

```bash
kubectl explain pod.spec.containers.resources
kubectl explain networkpolicy.spec.ingress
```

### Always verify your work before moving on

> [!NOTE]
> A command that runs without error is not the same as a task that is correct. Always verify.

```bash
kubectl get pod <name> -o wide        # Running? correct node/namespace?
kubectl describe pod <name>           # any warning events?
kubectl logs <name>                   # is the application working?
kubectl exec <name> -- <command>      # test connectivity or behaviour directly
```

### `kubectl apply -f` vs `kubectl edit`

Both are valid approaches:

- **`apply -f`** with a local file is safer and repeatable — if something breaks, fix the file and re-apply
- **`kubectl edit`** modifies the live resource directly; if the save fails (invalid YAML or a rejected field), it drops you back into the editor with the error message inline — useful for pinpointing exactly what's wrong; cross-reference with the official docs to fix it

---

## Domain Breakdown

| Domain | Weight |
|---|---|
| Application Environment, Configuration and Security | **25%** |
| Application Design and Build | **20%** |
| Application Deployment | **20%** |
| Services and Networking | **20%** |
| Application Observability and Maintenance | **15%** |

**Highest ROI topics**: RBAC · ConfigMaps/Secrets · resource limits · liveness/readiness probes · NetworkPolicy · multi-container pods (init + sidecar) · Deployments (rollout · history · undo · scale)
