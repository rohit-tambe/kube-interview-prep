# Lab 1.4: CrashLoopBackOff, OOMKilled, and Probes (27 Oct 2026)

**Cluster:** `kind-k8s-lab` (Kubernetes v1.35.0) | **Namespace:** `practice`

---

## Overview & Verified Results

This lab tests and proves the 4 core failure modes of the Kubernetes pod lifecycle:
1. Application boot failure (`CrashLoopBackOff`, Exit Code 1).
2. Linux kernel memory ceiling breach (`OOMKilled`, Exit Code 137).
3. Failing Liveness Probe (Kubelet container restart loop).
4. Failing Readiness Probe (Traffic gate closed, zero restarts, EndpointSlice isolation).

---

## 1. CrashLoopBackOff (`01-crashloop-exit1.yaml`)

* **Trigger:** Container command prints boot message, sleeps 3s, then executes `exit 1`.
* **Telemetry Recorded:**
  ```text
  State:          Waiting
    Reason:       CrashLoopBackOff
  Last State:     Terminated
    Reason:       Error
    Exit Code:    1
  Events:
    Warning  BackOff  kubelet  Back-off restarting failed container app
  ```
* **Log Extraction:**
  ```bash
  $ kubectl logs crashloop-demo -n practice
  Starting payment engine...
  Fatal error: database config missing!
  ```
* **Key Learning:** `CrashLoopBackOff` is Kubernetes applying exponential backoff delay (10s, 20s, 40s...) to protect host CPU when an app crashes with a non-zero exit code. Use `kubectl logs <pod> --previous` to inspect previous crash logs.

---

## 2. OOMKilled (`02-oomkilled-137-fix.yaml`)

* **Trigger:** Process heap memory leak in a shell loop (`x='abcdefghij'; while true; do x="$x$x$x$x"; done`) with `limits.memory: 256Mi`.
* **Key Finding During Testing:**
  * When memory limit was set to `<64Mi`, container failed before boot:  
    `RunContainerError` (`runc create failed: container init was OOM-killed`).
  * When using `dd` on `/dev/shm`, default container tmpfs hit `No space left on device` at 64MB without crossing 256Mi limit.
  * True heap allocation exceeded 256Mi, cleanly triggering the Linux kernel OOM Killer.
* **Telemetry Recorded:**
  ```text
  State:          Terminated
    Reason:       OOMKilled
    Exit Code:    137
  ```
* **Key Learning:** Exit Code for fatal signals is `128 + SignalNumber`. `SIGKILL` is signal 9.  
  $$\text{Exit Code} = 128 + 9 = 137$$
  The app cannot catch or recover from this; the Linux kernel cgroup terminates the process immediately.

---

## 3. Broken Liveness Probe (`03-broken-liveness.yaml`)

* **Trigger:** Liveness probe pointing to an invalid endpoint (`/non-existent-health-check` on port 80).
* **Telemetry Recorded:**
  ```text
  NAME                   READY   STATUS             RESTARTS   AGE
  broken-liveness-demo   0/1     CrashLoopBackOff   4          89s
  ```
* **Events Recorded:**
  ```text
  Warning  Unhealthy  kubelet  Liveness probe failed: HTTP probe failed with statuscode: 404
  Normal   Killing    kubelet  Container web failed liveness probe, will be restarted
  ```
* **Key Learning:** A failing Liveness probe forces the Kubelet to kill and restart the container. **Rule:** Never check external dependencies (DB/Redis) in a liveness probe, or a downstream blip will restart all pods simultaneously.

---

## 4. Broken Readiness Probe (`04-broken-readiness.yaml`)

* **Trigger:** Readiness probe pointing to an invalid endpoint (`/non-existent-ready-check` on port 80).
* **Telemetry Recorded:**
  ```text
  NAME                    READY   STATUS    RESTARTS   AGE
  broken-readiness-demo   0/1     Running   0          59s
  ```
* **Traffic Isolation Proof (Kubernetes v1.35):**
  When exposed through a Service (`test-svc`):
  ```bash
  $ kubectl describe service test-svc -n practice
  Endpoints:               <none>   # Empty!

  $ kubectl describe endpointslice test-svc-twm9p -n practice
  Endpoints:
    - Addresses:  10.244.1.14
      Conditions:
        Ready:    false           # Traffic gate closed!
      TargetRef:  Pod/broken-readiness-demo
  ```
* **Key Learning:** Unlike Liveness, a failing Readiness probe **never restarts the container** (Restarts stayed at 0). It signals the `EndpointSlice` controller to mark `Ready: false`, ensuring `kube-proxy` routes zero user traffic to the pod.

---

## Summary Matrix

| Failure Mode | Trigger | Status | Restarts | Exit Code / Condition |
| :--- | :--- | :--- | :--- | :--- |
| **App Crash** | `exit 1` | `CrashLoopBackOff` | Increases | `Exit Code: 1`, `Reason: Error` |
| **Memory Limit** | Heap > 256Mi | `OOMKilled` | Increases | `Exit Code: 137`, `Reason: OOMKilled` |
| **Liveness Fail** | HTTP 404 probe | `CrashLoopBackOff` | **Increases** | Container repeatedly killed by Kubelet |
| **Readiness Fail**| HTTP 404 probe | `Running` (`0/1`) | **0 (Zero)** | `EndpointSlice -> Ready: false`, `Endpoints: <none>` |

---

## 5. Endpoints vs. EndpointSlice Command Reference (v1.33+ & v1.35 Standard)

Kubernetes v1.33+ officially deprecates `v1 Endpoints` in favor of `discovery.k8s.io/v1 EndpointSlice`. Below are the essential commands for senior operations and interviews:

### 1. Verification & Listing
```bash
# List all EndpointSlices in a namespace
kubectl get endpointslices -n <namespace>

# Filter EndpointSlices belonging to a specific Service
kubectl get endpointslices -n <namespace> -l kubernetes.io/service-name=<service-name>

# Traditional command (shows deprecation warning on v1.33+)
kubectl get endpoints <service-name> -n <namespace>
```

### 2. Deep Health & Readiness Inspection
```bash
# Inspect the EndpointSlice to check Ready: true vs Ready: false
kubectl describe endpointslice <slice-name> -n <namespace>

# Inspect Service routing endpoints
kubectl describe service <service-name> -n <namespace>
# Look at: Endpoints (active routing) vs NotReadyAddresses (failed readiness)
```

### 3. Quick Hands-On Test Commands
```bash
# Expose a pod temporarily to create a Service and EndpointSlice
kubectl expose pod <pod-name> --port=80 --name=test-svc -n <namespace>

# Watch EndpointSlices change live as probes fail/pass
kubectl get endpointslices -n <namespace> -w

# Clean up test service
kubectl delete service test-svc -n <namespace>
```

### 4. Advanced JSONPath Filter (Instant check of ready vs unready pod IPs)
```bash
kubectl get endpointslice -n <namespace> -o jsonpath='{range .items[*].endpoints[*]}{.addresses[0]}{" -> Ready="}{.conditions.ready}{" (Pod: "}{.targetRef.name}{")
"}{end}'
```
