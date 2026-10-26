# Lab 1: Pending Pods, Taints & Rolling Updates
**Cluster:** `kind-k8s-lab` | **Namespace:** `practice` | **Date:** 23 Oct 2026

> [!IMPORTANT]
> This is a living document. Commands and outputs are added as the lab progresses. Use this before interviews to recall what you actually did — not just theory.

---

## 🧠 Key Concepts First

### Cluster Context vs Namespace

| Concept | Value | Analogy |
|---|---|---|
| **Cluster context** | `kind-k8s-lab` | The **building** — which cluster kubectl talks to |
| **Namespace** | `practice` | The **room** — logical partition inside the cluster |

### Capacity vs Allocatable (Node Resources)

| Term | Meaning |
|---|---|
| **Capacity** | Total physical resources the node has (CPU + RAM from the OS) |
| **Allocatable** | What's actually available for pods = Capacity minus what the OS and system daemons (kubelet, kube-proxy, etc.) need |

> [!TIP]
> In interviews: Always say **Allocatable**, not Capacity, when talking about pod scheduling. The scheduler uses Allocatable, not raw Capacity.

---

## ⚙️ Essential kubectl Commands — Explained

### Verify cluster context
```bash
kubectl config current-context
# Expected output: kind-k8s-lab
# Why: Always confirm you're on the right cluster before doing anything
```

### Create namespace safely (idempotent)
```bash
kubectl create namespace practice --dry-run=client -o yaml | kubectl apply -f -
# --dry-run=client  → generates the YAML without actually creating anything
# -o yaml           → outputs that YAML
# | kubectl apply   → pipes it to apply (create or update safely)
# Why: Using apply instead of create means it won't error if namespace already exists
```

### Deploy a manifest
```bash
kubectl apply -f 01-broken-resource-request.yaml
# apply  → create if not exists, update if exists (declarative)
# -f     → read from a file
```

### Watch pods in real time
```bash
kubectl get pods -n practice -w
# -n practice  → only show pods in the practice namespace
# -w           → watch/stream live updates (like tail -f)
# Ctrl+C to stop
```

### Check node capacity
```bash
# Option 1 — Custom table (clean output)
kubectl get nodes -o custom-columns="NODE:.metadata.name,CPU:.status.capacity.cpu,MEMORY:.status.capacity.memory"

# Option 2 — Full detail with grep filter
kubectl describe nodes | grep -A 6 "Capacity:"

# Option 3 — See allocatable (what scheduler actually uses)
kubectl describe nodes | grep -A 6 "Allocatable:"
```

### Describe a pod (most important for debugging)
```bash
kubectl describe pod <pod-name> -n practice
# Look at the Events: section at the bottom — this is the smoking gun
# It tells you in plain English WHY the pod is stuck
```

---

## 🔥 Lab 1.1 — Impossible Resource Request

### The Broken Manifest

```yaml
# File: 01-pending-pods/01-broken-resource-request.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: payment-processor
  namespace: practice
  labels:
    app: payment-processor
spec:
  replicas: 2
  selector:
    matchLabels:
      app: payment-processor
  template:
    metadata:
      labels:
        app: payment-processor
    spec:
      containers:
      - name: processor
        image: nginx:alpine
        resources:
          requests:
            cpu: "10"      # ← 10 CPU cores per pod — impossible on kind
            memory: "32Gi" # ← 32GB RAM per pod — impossible on kind
```

**Why it breaks:** Kind worker nodes have ~2 CPUs and ~8GB RAM total. Requesting 10 CPUs + 32Gi per pod means **no node can ever satisfy this request**.

### What Happened — Step by Step

**Step 1: Deploy**
```bash
kubectl apply -f 01-broken-resource-request.yaml
# Output:
deployment.apps/payment-processor created
```

**Step 2: Watch pods**
```bash
kubectl get pods -n practice -w
# Output:
NAME                                 READY   STATUS    RESTARTS   AGE
payment-processor-54f98dbd76-5q4vp   0/1     Pending   0          22s
payment-processor-54f98dbd76-p2lpf   0/1     Pending   0          22s
```

> [!WARNING]
> `Pending` status = pod has been accepted by the API server but **the scheduler could not find a node** to place it on. The container has NOT started yet.

### Investigation Path (what to do when you see Pending)

```
1. kubectl get pods -n practice              ← confirm Pending, get pod names
2. kubectl describe pod <name> -n practice   ← read Events: section
3. kubectl describe nodes                    ← check Allocatable vs what was requested
4. kubectl get events -n practice            ← alternative event view, sorted by time
```

### The Events section output — Actual lab output

```
Events:
  Type     Reason            Age                From               Message
  ----     ------            ----               ----               -------
  Warning  FailedScheduling  57s (x3 over 11m)  default-scheduler  
           0/3 nodes are available: 
           1 node(s) had untolerated taint(s),   ← control-plane taint
           2 Insufficient cpu,                    ← worker nodes lack CPU
           2 Insufficient memory.                 ← worker nodes lack memory
           preemption: 0/3 nodes are available: 3 Preemption is not helpful.
```

**Reading this message — 3 separate problems:**

| Node | Problem | Why |
|---|---|---|
| `k8s-lab-control-plane` (1 node) | Untolerated taint | Has `node-role.kubernetes.io/control-plane:NoSchedule` taint by default |
| `k8s-lab-worker` (2 nodes) | Insufficient cpu + memory | Our request (10 CPU, 32Gi) far exceeds node Allocatable |

> [!NOTE]
> `FailedScheduling (x3 over 11m)` means the scheduler retried 3 times. Kubernetes keeps retrying pending pods — it doesn't give up immediately.

---

## 🔍 Diagnosis Framework for Pending Pods

```
Pending Pod
    │
    ├─► kubectl describe pod → check Events:
    │       │
    │       ├─► Insufficient cpu/memory  → resource request too high
    │       ├─► node(s) had taint        → pod needs a toleration
    │       ├─► didn't match node selector → wrong nodeSelector/affinity
    │       └─► no nodes available       → cluster out of capacity
    │
    └─► kubectl describe nodes → check Allocatable vs Requests
```

---

## 📋 Results Table

| Lab | Break Injected | Symptom Observed | Root Cause | Fix Applied | Prevention |
|---|---|---|---|---|---|
| 1.1 | cpu: "10", memory: "32Gi" | 2 pods stuck in Pending | Resource request exceeds all node Allocatable (8 CPU, 7.6Gi) | Reduced to cpu: "250m", memory: "128Mi" with limits | ResourceQuota + LimitRange |
| 1.2 | *(taints — coming next)* | | | | |
| 1.3 | *(rolling update — coming next)* | | | | |

---

## 💬 Interview Answer Templates

### Q: "A pod is stuck in Pending — walk me through your investigation."

> "First I'd run `kubectl get pods -n <namespace>` to confirm the state and get the pod name. Then `kubectl describe pod <name> -n <namespace>` and go straight to the **Events** section at the bottom — that's the fastest signal. It'll tell me whether it's a resource constraint, a taint/toleration mismatch, a node selector issue, or a capacity problem. If it's resource-related, I'd then `kubectl describe nodes` to compare Allocatable resources against what the pod is requesting. Based on that, I'd either fix the resource request, add a toleration, or scale the cluster."

### Q: "What's the difference between Capacity and Allocatable on a node?"

> "Capacity is the total physical resources the node hardware has. Allocatable is what's actually available for pods — it's Capacity minus the reserved resources for the OS, kubelet, and system daemons. The scheduler always uses Allocatable, never raw Capacity, when deciding where to place a pod."

---

---

## 🏷️ Core Concept: Taints & Tolerations (From Scratch)

> [!NOTE]
> Rohit's baseline: No prior hands-on or conceptual Kubernetes knowledge. All concepts built from ground up.

### Real-world analogy

Think of a **hospital** with different wards:

| Ward | Sign on door | Who can enter |
|---|---|---|
| ICU | ⚠️ "Authorized staff only" | Only doctors with ICU badge |
| General ward | No sign | Anyone |

- **Taint** = the sign on the door → *"Don't come here unless you're authorized"*
- **Toleration** = the badge → *"I'm allowed in despite the sign"*

### In Kubernetes

```
NODE = the ward (the machine)
POD  = the person trying to enter

TAINT      = a label on the NODE: "don't schedule pods here"
TOLERATION = a pass on the POD:  "I'm allowed despite the taint"
```

### Why control-plane has a taint

```
Your 3-node cluster:

k8s-lab-control-plane  ← THE BRAIN
│   Runs: etcd              (cluster database)
│         kube-apiserver    (the API kubectl talks to)
│         kube-scheduler    (decides where pods go)
│         kube-controller-manager (watches & fixes things)
│
k8s-lab-worker         ← Runs YOUR app pods
k8s-lab-worker2        ← Runs YOUR app pods
```

If user pods ran on the control-plane and consumed its memory → **etcd crashes → kube-apiserver crashes → entire cluster is dead**.

So Kubernetes auto-applies a **"keep out"** taint:
```
node-role.kubernetes.io/control-plane:NoSchedule
```

Your `payment-processor` pod has no toleration → gets rejected from control-plane.

### The 3 taint effects

| Effect | Meaning |
|---|---|
| `NoSchedule` | Don't schedule new pods here (existing pods stay) |
| `PreferNoSchedule` | Try to avoid, but allow if no other choice |
| `NoExecute` | Don't schedule new pods AND evict existing ones |

### Verify the taint on your cluster

```bash
kubectl describe node k8s-lab-control-plane | grep -A 3 Taints
# Output:
# Taints: node-role.kubernetes.io/control-plane:NoSchedule
```

### What a toleration looks like in a pod YAML

```yaml
spec:
  tolerations:
  - key: "node-role.kubernetes.io/control-plane"
    operator: "Exists"
    effect: "NoSchedule"
  # Now this pod CAN be scheduled on the control-plane
```

> [!TIP]
> In AKS (Azure): You never see the control-plane nodes at all — Azure manages them fully. Your node pools are worker nodes only. But the concept of taints/tolerations still applies to your own node pools (e.g., GPU nodes, spot nodes).

---

## 🎤 Q&A — Interview Questions & Answers (Live Session)

---

### Q1: What is the very first kubectl command you run when pods are Pending?

**Rohit's answer:** `kubectl describe node`

**Score: 35/100** | **Verdict: Borderline**

**Why it's not ideal:** Describing a node when you don't yet know the cause jumps too wide too fast. The pod's own Events section tells you exactly why it's stuck — in one command.

**Better answer:**
```bash
kubectl get pods -n practice                    # confirm Pending, get pod names
kubectl describe pod <pod-name> -n practice     # read Events: — this is the smoking gun
```
> In an interview say: *"I start narrow — at the broken object — then go wide. The Events section in `kubectl describe pod` gives me the root cause in plain English within 30 seconds."*

---

### Q2: The Events say `0/3 nodes are available: 1 node(s) had untolerated taint(s)`. Which node has the taint and why?

**Rohit's answer:** `k8s-lab-control-plane` has the untolerated taint ✅

**Score: 60/100** — Correct identification, but WHY needs more depth.

**Full answer:**

The `k8s-lab-control-plane` node has this taint applied automatically by Kubernetes:

```bash
# Verify with:
kubectl describe node k8s-lab-control-plane | grep -A 3 Taints
# Output:
# Taints: node-role.kubernetes.io/control-plane:NoSchedule
```

**Why Kubernetes does this:**
- The control-plane runs critical system components: `etcd`, `kube-apiserver`, `kube-scheduler`, `kube-controller-manager`
- If user pods ran on the control-plane and consumed all resources, the entire cluster could go down
- The `NoSchedule` taint means: *"Don't schedule any pod here unless it explicitly tolerates this taint"*
- Only system pods (like `CoreDNS`) have the matching toleration

**Effect on our pod count:**
- 3 nodes total, 1 (control-plane) is off-limits → effectively only **2 worker nodes** available for user pods
- Both worker nodes failed on resources → result: `0/3 nodes available`

> [!TIP]
> In AKS: control-plane nodes are fully managed by Azure — you never see them. Your worker node pool is what you scale. Same concept, but Azure abstracts the control-plane taint away from you.

---

*More Q&A added as lab progresses. Next: Lab 1.2 — Taints & Tolerations*
