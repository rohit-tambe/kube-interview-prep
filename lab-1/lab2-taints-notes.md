# Lab 1.2: Taints & Tolerations
**Cluster:** `kind-k8s-lab` | **Namespace:** `practice` | **Date:** 23 Oct 2026

> [!IMPORTANT]
> This is a living document. Updated as the lab progresses. Use this before interviews.

---

## 🧠 Core Concepts

### What is a Taint?

A **taint** is a label on a **node** that says: *"Don't schedule pods here unless they have a matching badge (toleration)."*

```
WITHOUT taint                       WITH taint (NoSchedule)

k8s-lab-worker                      k8s-lab-worker
┌──────────────────┐                ┌──────────────────┐
│                  │                │ ⛔ dedicated=    │
│  Any pod can     │                │    payments      │
│  land here       │                │                  │
│                  │                │  Only pods WITH  │
└──────────────────┘                │  toleration land │
                                    └──────────────────┘
```

### Taint = Node label | Toleration = Pod badge

```
NODE  ──► gets a TAINT      (the "keep out" sign)
POD   ──► gets a TOLERATION (the "I'm allowed" badge)
```

### 3 Taint Effects

| Effect | What happens to pods WITHOUT toleration |
|---|---|
| `NoSchedule` | New pods rejected. Existing pods stay. |
| `PreferNoSchedule` | Try to avoid the node, but allow if no other option |
| `NoExecute` | New pods rejected AND existing pods **evicted** |

### Namespace vs Taint — NOT the same

| | Namespace | Taint |
|---|---|---|
| **Lives on** | Pod / resource | Node |
| **Controls** | Logical grouping | Physical node placement |
| **Example** | `namespace: practice` | `dedicated=payments:NoSchedule` |

---

## 🏢 Taints in AKS — Where You've Already Seen This

```
AKS Cluster
│
├── System node pool  →  AKS auto-taints: CriticalAddonsOnly=true:NoSchedule
│   └── Blocks YOUR app pods — only system pods (CoreDNS etc.) go here
│
├── User node pool    →  No taint by default — your apps land here
│
└── Spot node pool    →  Auto-tainted: kubernetes.azure.com/scalesetpriority=spot:NoSchedule
    └── Only pods that explicitly tolerate spot go here (cheap but evictable)
```

**In Terraform:**
```hcl
resource "azurerm_kubernetes_cluster_node_pool" "gpu" {
  node_taints = ["sku=gpu:NoSchedule"]  # ← taint on node pool
}
```

**Matching toleration in pod YAML:**
```yaml
tolerations:
- key: "sku"
  value: "gpu"
  effect: "NoSchedule"
```

---

## ⚙️ Lab Commands — Explained

### Add a taint to a node
```bash
kubectl taint node k8s-lab-worker dedicated=payments:NoSchedule
# Format: kubectl taint node <node-name> <key>=<value>:<effect>
# Output: node/k8s-lab-worker tainted
```

### Verify taints on a node
```bash
kubectl describe node k8s-lab-worker | grep -A 3 Taints
# Output:
# Taints: dedicated=payments:NoSchedule
```

### Remove a taint (the - at the end removes it)
```bash
kubectl taint node k8s-lab-worker dedicated=payments:NoSchedule-
#                                                              ↑ minus sign = remove
```

### Deploy pods and check which node they land on
```bash
kubectl apply -f 01-no-toleration.yaml
kubectl get pods -n practice -o wide
# -o wide shows NODE column — critical for verifying taint behavior
```

### Check all events (to see FailedScheduling)
```bash
kubectl get events -n practice --sort-by='.lastTimestamp'
```

---

## 🔥 Lab Steps & Actual Outputs

### Step 1 — Taint the worker node
```bash
kubectl taint node k8s-lab-worker dedicated=payments:NoSchedule
```
**Output:** `node/k8s-lab-worker tainted` ✅

**Cluster state after:**
```
k8s-lab-control-plane  ⛔ NoSchedule  (node-role.kubernetes.io/control-plane)
k8s-lab-worker         ⛔ NoSchedule  (dedicated=payments) ← added by us
k8s-lab-worker2        ✅ No taint
```

### Step 2 — Deploy WITHOUT toleration
```bash
kubectl apply -f 01-no-toleration.yaml
kubectl get pods -n practice -o wide
```
**Actual Output:**
```
NAME                           READY   STATUS    NODE
payment-api-85c55756c8-lc25h   1/1     Running   k8s-lab-worker2
payment-api-85c55756c8-rnggs   1/1     Running   k8s-lab-worker2
payment-api-85c55756c8-zdzcm   1/1     Running   k8s-lab-worker2
```
**Observation:** ALL 3 pods went to `k8s-lab-worker2` only. `k8s-lab-worker` = zero pods. Taint worked. ✅

### Step 3 — Add toleration and fix

The `02-with-toleration.yaml` adds this block before `containers:`:
```yaml
      tolerations:
      - key: "dedicated"
        operator: "Equal"
        value: "payments"
        effect: "NoSchedule"
```

```bash
kubectl delete deployment payment-api -n practice
kubectl apply -f 02-with-toleration.yaml
kubectl get pods -n practice -o wide
```
**Actual Output:**
```
NAME                           READY   STATUS    NODE
payment-api-56b686b58f-p8gv7   1/1     Running   k8s-lab-worker2
payment-api-56b686b58f-tcr8h   1/1     Running   k8s-lab-worker2
payment-api-56b686b58f-x7dqc   1/1     Running   k8s-lab-worker   ← tainted node now accepted pod!
```
**Observation:** Pods now spread across BOTH worker nodes. Toleration = key that unlocked `k8s-lab-worker`. ✅

### Step 4 — Clean up
```bash
kubectl delete deployment payment-api -n practice
kubectl taint node k8s-lab-worker dedicated=payments:NoSchedule-
#                                                              ↑ minus sign = removes taint
kubectl get nodes
```
**Output:**
```
node/k8s-lab-worker untainted
NAME                    STATUS   ROLES           AGE    VERSION
k8s-lab-control-plane   Ready    control-plane   297d   v1.35.0
k8s-lab-worker          Ready    <none>          297d   v1.35.0
k8s-lab-worker2         Ready    <none>          297d   v1.35.0
```
Cluster back to clean state. ✅

---

## 🎤 Q&A — Interview Questions & Answers

---

### Q3: You have 3 pods and only 1 untainted node. What happens?

**Rohit's answer:** 2 pods go into Pending state because there is no toleration in the YAML

**Score: 70/100**

**Reasoning is correct:**
- `k8s-lab-control-plane` → ⛔ blocked (control-plane taint)
- `k8s-lab-worker` → ⛔ blocked (dedicated=payments, no toleration in pod)
- `k8s-lab-worker2` → ✅ only free node

**Key insight missed:** Taints control *eligibility* (which nodes a pod can go to). Resources control *capacity* (whether it fits). These are two separate scheduler checks. All 3 pods can go to worker2 if it has enough capacity — so ALL 3 may run on the same node rather than going Pending.

**Better interview answer:**
> *"With 1 untainted node, the scheduler tries to fit all 3 pods there. If the node has enough resources, all 3 run — they pile onto the one available node. If the node is full, the extras go Pending. I'd check `kubectl get pods -o wide` to confirm which node they landed on."*

---

### Q4: All 3 pods are on worker2. worker2 goes down. What happens to your payment service?

**Rohit's answer:** I don't know

**Score: 0/100** — But this is a critical production concept. Learn it.

**Answer:**
```
All 3 pods on worker2
     ↓
worker2 crashes
     ↓
All 3 pods die simultaneously
     ↓
payment-api → 100% DOWN
     ↓
Customers can't pay → P1 incident → revenue loss
```

Having `replicas: 3` means nothing if all replicas are on the **same node**.

**The fix — Pod Anti-Affinity:**

Tells the scheduler: *"Never put 2 pods of the same app on the same node."*

```yaml
spec:
  affinity:
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            app: payment-api        # ← my own app label
        topologyKey: kubernetes.io/hostname  # ← spread by node
```

**Result:**
```
WITHOUT anti-affinity:       WITH anti-affinity:
worker2: pod1, pod2, pod3    worker:  pod1
                             worker2: pod2 + pod3

worker2 dies →               worker2 dies →
ALL pods dead ❌             pod1 still running ✅
service down                 service degraded but alive
```

> [!IMPORTANT]
> In banking/payments this is non-negotiable. You never run all payment replicas on one node. Anti-affinity is a standard production requirement for any critical service.

**In AKS:** Same YAML works. AKS respects pod anti-affinity across availability zones too — use `topologyKey: topology.kubernetes.io/zone` to spread across AZs.

---

## 📋 Results Table

| Step | Action | Expected | Actual | Lesson |
|---|---|---|---|---|
| 1 | Taint k8s-lab-worker | node tainted | ✅ `node/k8s-lab-worker tainted` | Taint is instant — no restart needed |
| 2 | Deploy without toleration | pods avoid worker | ✅ All 3 on worker2 only | Taint = eligibility filter. Pods pile on the only free node |
| 3 | Deploy with toleration | pods spread across nodes | ✅ 2 on worker2, 1 on worker | Toleration = key that unlocks the tainted node |
| 4 | Remove taint | cluster returns to normal | ✅ `node/k8s-lab-worker untainted` | Taint removal is instant, no pod restart needed |

---

## 💬 Interview Answer Templates

### Q: "What is a taint and when would you use one in production?"
> "A taint is a label on a Kubernetes node that prevents pods from being scheduled there unless the pod has a matching toleration. I'd use taints to create dedicated node pools — for example, GPU nodes that should only run ML workloads, spot nodes for batch jobs, or payment nodes for PCI-scoped workloads. In AKS, Azure automatically taints the system node pool so your app pods don't land there."

### Q: "What's the difference between NoSchedule and NoExecute?"
> "NoSchedule blocks new pods but leaves existing pods running. NoExecute is more aggressive — it blocks new pods AND evicts any existing pods on that node that don't have a matching toleration. I'd use NoExecute when I want to drain a node of specific workloads immediately, not just stop new ones from arriving."

### Q: "How do you drain a node for maintenance in production?"
> "`kubectl drain <node>` — it cordons the node (marks it unschedulable) and evicts all pods. Before running it I'd check PodDisruptionBudgets to make sure the drain won't take down too many replicas at once."

---

## ⚠️ Key Mistakes to Avoid in Interviews

| Mistake | Correct thinking |
|---|---|
| "Taint blocks the node completely" | Taint blocks pods WITHOUT matching toleration. Pods WITH toleration can still go there. |
| "3 replicas = high availability" | Only if replicas are on DIFFERENT nodes. Same node = single point of failure. |
| "NoSchedule evicts existing pods" | No — NoSchedule only blocks NEW pods. NoExecute evicts existing ones. |
| Jumping to `kubectl describe node` first | Start narrow: `kubectl describe pod` → Events section. Then go wide. |

---

## ✅ Lab 1.2 Complete

**Next:** Lab 1.3 — Rolling Updates (broken deployments, readiness probes, rollback)
