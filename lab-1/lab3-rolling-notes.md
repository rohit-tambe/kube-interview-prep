# Lab 1.3: Rolling Updates
**Cluster:** `kind-k8s-lab` | **Namespace:** `practice` | **Date:** 23 Oct 2026

> [!IMPORTANT]
> Living document. Updated as lab progresses. Use before interviews.

---

## 🧠 Core Concepts

### What is a Rolling Update?

When you update a Deployment (new image, new config), Kubernetes doesn't kill all pods at once.
It replaces them **gradually** — one by one — so the service stays available.

```
Before update:          During rolling update:       After update:
pod1 (v1) ✅            pod1 (v1) ✅                pod1 (v2) ✅
pod2 (v1) ✅     →      pod2 (v2) ✅         →      pod2 (v2) ✅
pod3 (v1) ✅            pod3 (terminating)           pod3 (v2) ✅
```

### Rolling Update Strategy (default in Kubernetes)

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1    # max pods that can be down during update
    maxSurge: 1          # max extra pods that can be created during update
```

| Parameter | Meaning | Default |
|---|---|---|
| `maxUnavailable` | How many pods can be unavailable at once | 25% |
| `maxSurge` | How many extra pods can be created temporarily | 25% |

### Key kubectl rollout commands

| Command | What it does |
|---|---|
| `kubectl rollout status deployment/<name> -n practice` | Watch update progress live |
| `kubectl rollout history deployment/<name> -n practice` | See all revision history |
| `kubectl rollout undo deployment/<name> -n practice` | Roll back to previous version |
| `kubectl rollout undo deployment/<name> --to-revision=2 -n practice` | Roll back to specific revision |
| `kubectl set image deployment/<name> container=image:tag -n practice` | Trigger update via CLI |

---

## 🔥 Lab Steps & Actual Outputs

### Step 1 — Deploy working baseline (01-good-deployment.yaml)
```bash
kubectl apply -f 01-good-deployment.yaml
kubectl rollout status deployment/payment-api -n practice
```
**Output:** *(to be filled)*

---

### Step 2 — Break it: update with bad image (02-broken-image.yaml)
```bash
kubectl apply -f 02-broken-image.yaml
kubectl rollout status deployment/payments-deployment -n practice
kubectl get pods -n practice
kubectl describe pod <failing-pod> -n practice
```
**Output:**
```text
NAME                                   READY   STATUS              RESTARTS   AGE
payments-deployment-5b84c598f8-gnnln   0/1     ErrImagePull        0          3s
payments-deployment-5b84c598f8-v4nk7   0/1     ContainerCreating   0          3s
payments-deployment-5dc4499bb9-58fsg   1/1     Running             0          28s
payments-deployment-5dc4499bb9-gjr96   1/1     Running             0          28s
```
*Key observation: The 2 old pods remained `Running` due to `maxUnavailable: 1`. Rollout paused — Kubernetes never auto-rolls back.*

---

### Step 3 — Rollback
```bash
kubectl rollout undo deployment/payments-deployment -n practice
kubectl rollout status deployment/payments-deployment -n practice
kubectl get pods -n practice
```
**Output:**
```text
NAME                                   READY   STATUS    RESTARTS   AGE
payments-deployment-5dc4499bb9-58fsg   1/1     Running   0          80s
payments-deployment-5dc4499bb9-db7n2   1/1     Running   0          3s
payments-deployment-5dc4499bb9-gjr96   1/1     Running   0          80s
```
*Key observation: 3/3 replicas restored to healthy hash `5dc4499bb9`. The third replica `db7n2` spun up in 3s.*

---

### Step 4 — Readiness probe break (03-broken-readiness.yaml)
```bash
kubectl apply -f 03-broken-readiness.yaml
kubectl rollout status deployment/payments-deployment -n practice
kubectl get all -n practice
```
**Output:**
```text
NAME                                       READY   STATUS    RESTARTS   AGE
pod/payments-deployment-54d7f9674b-2bhdf   0/1     Running   0          4s
pod/payments-deployment-54d7f9674b-8wf4f   0/1     Running   0          4s
pod/payments-deployment-5dc4499bb9-qbf92   1/1     Running   0          7m46s
pod/payments-deployment-5dc4499bb9-s6c49   1/1     Running   0          7m46s

NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/payments-deployment   2/3     2            2           7m46s

NAME                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/payments-deployment-54d7f9674b   2         2         0       4s
replicaset.apps/payments-deployment-5dc4499bb9   2         2         2       7m46s
```
*Key observation: The 2 new pods (`54d7f9674b`) are `Running` but `0/1 READY` with `0 RESTARTS`. Failing readiness NEVER restarts pods; it only blocks traffic. Because new pods never become ready, the rollout controller halts and protects the 2 old pods (`5dc4499bb9`) to maintain `AVAILABLE: 2`.*

---

### Step 5 — Fix readiness probe (04-fixed-readiness.yaml)
```bash
kubectl apply -f 04-fixed-readiness.yaml
kubectl rollout status deployment/payments-deployment -n practice
kubectl get pods -n practice
```
**Output:**
```text
deployment "payments-deployment" successfully rolled out

NAME                                   READY   STATUS    RESTARTS   AGE
payments-deployment-75587ddfd8-57rrs   1/1     Running   0          2m17s
payments-deployment-75587ddfd8-cf4rj   1/1     Running   0          2m17s
payments-deployment-75587ddfd8-qt9lm   1/1     Running   0          2m14s
```
*Key observation: Probe succeeded with HTTP 200 on port 80 path `/`. All 3 pods reached `1/1 READY`. The rollout controller sequentially replaced all old replicas and cleanly marked the deployment as successfully rolled out.*

---

## 🧠 Master Mental Models (Quick Recall)

### 1. Nodes vs Pods vs Containers
* **Node:** Physical VM / Server with CPU, RAM, OS, Kubelet.
* **Pod:** Wrapper unit that gets the **IP address**. Holds 1 or more containers. Containers inside a Pod share network (`localhost`) and storage.
* **Container:** Isolated Linux process running the actual code/image (e.g. Nginx, Go binary).

### 2. Liveness vs Readiness (The 2-Word Hook)
```
┌──────────────────┬─────────────────────────────┬────────────────────────────────┐
│ Probe            │ Purpose                     │ Action on Failure              │
├──────────────────┼─────────────────────────────┼────────────────────────────────┤
│ Liveness Probe   │ Heartbeat / Process Health  │ 💥 KILLS & RESTARTS container  │
├──────────────────┼─────────────────────────────┼────────────────────────────────┤
│ Readiness Probe  │ Traffic Routing Gate        │ 🛑 CUTS OFF TRAFFIC            │
│                  │                             │ (Pod is NOT restarted!)        │
└──────────────────┴─────────────────────────────┴────────────────────────────────┘
```

### 3. Taint Effects (The VIP Club Bouncer)
```
┌──────────────────────┬────────────────────────────────────────────────────────┐
│ Taint Effect         │ Behavior                                               │
├──────────────────────┼────────────────────────────────────────────────────────┤
│ NoSchedule           │ Rejects NEW pods. Existing pods STAY RUNNING!          │
├──────────────────────┼────────────────────────────────────────────────────────┤
│ PreferNoSchedule     │ Soft rule: avoid if possible, but allow if needed.     │
├──────────────────────┼────────────────────────────────────────────────────────┤
│ NoExecute            │ Hard eviction: kicks out and KILLS existing pods!      │
└──────────────────────┴────────────────────────────────────────────────────────┘
```

---

## 🎤 Q&A — Master Interview Revision

### Q1: "Why are pods stuck in `Pending` when nodes have 60% free physical memory?"
> **Answer:** "The Kubernetes scheduler places pods based on **Resource REQUESTS**, not real-time physical memory or Limits. Even if runtime memory is only 40%, the node's **Allocatable** memory is already 100% committed by existing pods' reservations on paper:
> $$\sum (\text{Existing Requests}) + \text{New Pod Request} \le \text{Allocatable}$$
> If unreserved Allocatable memory is insufficient, the scheduler rejects the node and leaves the pod in `Pending`."

### Q2: "A node is tainted with `NoSchedule`. What happens to 10 pods already running on it without tolerations?"
> **Answer:** "The 10 existing pods **continue running completely uninterrupted**. `NoSchedule` only applies to the scheduler during future pod placement decisions. To evict existing pods from a node, the **`NoExecute`** taint effect must be used."

### Q3: "What happens when a new Deployment image crashes continuously during a rolling update?"
> **Answer:** "The new pods enter **`CrashLoopBackOff`**. Kubernetes **never automatically rolls back**. Because `RollingUpdate` enforces `maxUnavailable: 1`, the controller halts the rollout and protects the remaining healthy old pods, preventing a total outage. To recover full capacity, an engineer must execute:
> `kubectl rollout undo deployment/<name> -n <namespace>`."

### Q4: "Why did a pod with a broken readiness probe show `Running` with `RESTARTS: 0`?"
> **Answer:** "Because a failing **Readiness Probe never restarts a container**; it only unlinks the Pod IP from the Service endpoints. Container restarts are strictly controlled by the **Liveness Probe** or container exit codes. Because the process is alive, restarts remain 0, but the rollout controller halts because the pod never passes its readiness gate."

---

## 📋 Results Table

| Step | Break Injected | Symptom | Root Cause | Fix | Lesson |
|---|---|---|---|---|---|
| 1 | Baseline | All pods `1/1 Running` | — | — | Establish clean baseline first |
| 2 | Bad image tag | Pods in `ImagePullBackOff` | Tag doesn't exist (`NotFound`) | `kubectl rollout undo` | K8s pauses on failure; never auto-rolls back |
| 3 | Bad readiness probe | Pods `Running` but `0/1 READY` (0 Restarts) | Probe hitting closed port 8080 | Fix probe port/path | Readiness controls traffic routing, not restarts |
| 4 | Fixed readiness probe | All pods `1/1 Ready`, rollout complete | Clean HTTP 200 OK | — | Readiness gate clears rollout progression |

---

## ⚠️ Key Mistakes to Avoid in Senior Interviews

| Mistake | Correct thinking |
|---|---|
| *"Rolling update automatically rolls back on failure"* | **FALSE.** It PAUSES. You must manually invoke `kubectl rollout undo`. |
| *"Readiness probe restarts crashed pods"* | **FALSE.** Readiness cuts off traffic. Liveness restarts the container. |
| *"Scheduler checks current physical memory usage"* | **FALSE.** Scheduler checks `requests` against `Allocatable`. |
| *"NoSchedule evicts existing workloads"* | **FALSE.** `NoSchedule` only blocks new pods. `NoExecute` evicts. |
| *"kubectl rollback is the CLI command"* | **FALSE.** The command is `kubectl rollout undo`. |

---

*Lab 1 (1.1, 1.2, 1.3) Complete & Verified.*
