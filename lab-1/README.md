# Lab 1: Pending Pods, Taints, and Rolling Updates
**Cluster Context:** `kind-k8s-lab` | **Namespace:** `practice`

## Directory Structure
- [`01-pending-pods/`](file:///d:/docker-workspace/interview-prep/lab-1/01-pending-pods/) - Resource requests, capacity limits, scheduler predicates, and prevention via Quotas & LimitRanges.
- `02-taints-tolerations/` - Node taints (`NoSchedule`, `NoExecute`), tolerations, and dedicated node scheduling.
- `03-rolling-updates/` - Rolling update strategies, broken deployments, readiness probes, and rollback mechanics.
- `04-crashloop-and-probes/` - Pod lifecycle, CrashLoopBackOff (Exit 1), OOMKilled (Exit 137), Liveness restarts vs Readiness EndpointSlice traffic gating (27 Oct 2026).
