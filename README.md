# 🚀 Kubernetes Senior Engineering & Interview Readiness Labs
**Cluster Engine:** `kind` (Multi-node: 1 control-plane, 2 workers)  
**Target Roles:** Lead Platform Engineer, DevOps Architect, Staff SRE (15+ Years)  
**Standard:** Problem-Solving, Failure Injection, RCA, and Architecture Trade-offs  

---

## 📂 Lab Directory Structure

### 🔹 [Lab 1: Scheduling, Placement & Rollout Failures](file:///d:/docker-workspace/kube-interview-prep/lab-1/)
Mastering core scheduling constraints, node eligibility, and deployment lifecycle.

* **[01-pending-pods/](file:///d:/docker-workspace/kube-interview-prep/lab-1/01-pending-pods/)**
  * `Allocatable` vs `Capacity` calculation in `kube-scheduler`.
  * Scheduling predicate failures: `Insufficient cpu`, `Insufficient memory`.
  * Real broken manifests (10 CPU / 32Gi) vs right-sized manifests.
  * Notes: [`lab1-notes.md`](file:///d:/docker-workspace/kube-interview-prep/lab-1/lab1-notes.md)

* **[02-taints-tolerations/](file:///d:/docker-workspace/kube-interview-prep/lab-1/02-taints-tolerations/)**
  * Node taints (`NoSchedule`, `PreferNoSchedule`, `NoExecute`) vs Pod tolerations.
  * Node eligibility vs capacity; Pod placement isolation.
  * Production patterns: AKS `CriticalAddonsOnly`, GPU node isolation, Topology Spread.
  * Notes: [`lab2-taints-notes.md`](file:///d:/docker-workspace/kube-interview-prep/lab-1/lab2-taints-notes.md)

* **[03-rolling-updates/](file:///d:/docker-workspace/kube-interview-prep/lab-1/03-rolling-updates/)**
  * RollingUpdate parameters: `maxSurge` & `maxUnavailable`.
  * Stalled rollouts: `ImagePullBackOff` vs `CrashLoopBackOff`.
  * Health check distinction: Liveness (kills/restarts) vs Readiness (traffic gate).
  * Rapid mitigation using `kubectl rollout undo` with zero downtime.
  * Notes: [`lab3-rolling-notes.md`](file:///d:/docker-workspace/kube-interview-prep/lab-1/lab3-rolling-notes.md)

---

## 🗺️ Upcoming Roadmap

* **Lab 2:** Services, Endpoints, Kube-Proxy & CoreDNS Troubleshooting
* **Lab 3:** Ingress Controllers, TLS Termination, and Path-based Routing
* **Lab 4:** Configuration & Secret Management (ConfigMaps, Azure Key Vault CSI)
* **Lab 5:** StatefulSets, Persistent Volumes (PVC, StorageClasses, Azure Disk/File)
* **Lab 6:** Observability, Metrics Server, HPA & Cluster Autoscaler
