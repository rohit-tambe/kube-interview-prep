# Interview Readiness Tracker

Rohit · Started 22 Oct 2026 · Target role: Tech Lead (DevOps / Cloud / Platform) · Daily plan: 90-min main session + Golang DSA track

This is your revision book. After every question-and-answer session, the questions, your answers, the score and the feedback are added here, each with an example and a diagram. Read it top to bottom before an interview.

## How to use this doc

1. Read the **Scoreboard** to see where you stand.
2. Read each day's entries: Question, Your answer, Score, What was good, What was missing, Model answer, Example, Remember it.
3. Before an interview, read only the **Remember it** lines (the last section collects them).

## Day index

| Day | Date | Focus | Status |
| --- | --- | --- | --- |
| Day 0 | 22 Oct 2026 | Baseline diagnostic, 9 questions | Done |
| Day 1 | 26 Oct 2026 | AKS Pending pods, Golang goroutine leak, first DSA problem | Done |
| Day 2 | 27 Oct 2026 | Pod lifecycle, CrashLoopBackOff, OOMKilled, Probes | In progress |

## Master schedule (30 days, 26 Oct to 24 Nov 2026)

Schedule shifted on 26 Oct 2026: 23 to 25 Oct were missed, so Day 1 starts on 26 Oct. The plan assumes a 90-minute main session plus 30 minutes of DSA each day. Main session: revision (10), core topic (30), Golang coding (25), scenario (15), feedback (10). Labs are done during or after the core topic. Re-test days check your baseline weak spots. Start applying to roles around Day 14. The plan adjusts to your results.

| Day | Date | Core topic (30 min) | Golang coding (25 min) | DSA (30 min) |
| --- | --- | --- | --- | --- |
| 1 | Mon 26 Oct | AKS Pending pods, taints, rolling updates + Lab 1 | Goroutine leaks | Baseline problem (arrays, hash maps) |
| 2 | Tue 27 Oct | AKS: CrashLoopBackOff, OOMKilled, probes + lab | Context and timeouts | Arrays and hash maps |
| 3 | Wed 28 Oct | AKS networking: Services, DNS, ingress | Worker pool | Two pointers |
| 4 | Thu 29 Oct | Requests, limits, HPA, cluster autoscaler | Channel patterns | Sliding window |
| 5 | Fri 30 Oct | Azure: Key Vault, workload identity, RBAC | Mutex, WaitGroup, race detector | Stack and queue |
| 6 | Sat 31 Oct | Azure networking: SNAT, NAT Gateway, NSG, private endpoints | HTTP client reuse | Linked list |
| 7 | Sun 1 Nov | Review + scenario mock. Re-test Q1, Q4, Q5, Q6 | Review | Review |
| 8 | Mon 2 Nov | Database: connection pools, slow queries, locks | database/sql pooling | Binary search |
| 9 | Tue 3 Nov | Database HA, replication, failover | Transactions and cancellation | Sorting and intervals |
| 10 | Wed 4 Nov | Redis: cache stampede, TTL, eviction | Singleflight and cache with expiry | Heap and top-K |
| 11 | Thu 5 Nov | IAM: OAuth2, OIDC, JWT, JWKS, key rotation, clock skew | JWT validation middleware | Tree BFS |
| 12 | Fri 6 Nov | IAM: RBAC vs ABAC, service-to-service auth, mTLS | Middleware and rate limiter | Tree DFS |
| 13 | Sat 7 Nov | Observability: logs, metrics, traces, SLOs, alerts | pprof profiling | Graph BFS |
| 14 | Sun 8 Nov | Review + mock. Re-test Q2, Q3, Q7. Start applying | Review | Review |
| 15 | Mon 9 Nov | Payment gateway design 1: idempotency, state machine | Idempotent handler | Graph DFS, union-find |
| 16 | Tue 10 Nov | Payment design 2: timeouts, reconciliation, outbox, queues | Outbox worker | Dynamic programming intro |
| 17 | Wed 11 Nov | Multi-region, HA, DR, RTO and RPO | Graceful shutdown | Dynamic programming |
| 18 | Thu 12 Nov | Release safety: rolling, blue-green, canary, feature flags | Table-driven tests | Backtracking basics |
| 19 | Fri 13 Nov | Terraform, IaC, GitOps, drift | Config and CLI tooling | Mixed practice |
| 20 | Sat 14 Nov | Security for banking: secrets, encryption, audit | Concurrency review | Timed mixed set |
| 21 | Sun 15 Nov | System design mock. Re-test Q8 | Review | Review |
| 22 | Mon 16 Nov | Leadership: incident command, RCA (Q9) | Timed coding mock 1 | Timed online assessment 1 |
| 23 | Tue 17 Nov | Tech Lead scenarios: priorities, conflict, mentoring | Timed coding mock 2 | Timed online assessment 2 |
| 24 | Wed 18 Nov | Your 3 production incident stories (STAR) | Fix mock mistakes | Weak topics |
| 25 | Thu 19 Nov | Full mock interview 1: troubleshooting + Go | Review | Weak topics |
| 26 | Fri 20 Nov | Full mock interview 2: system design + leadership | Review | Weak topics |
| 27 | Sat 21 Nov | Full mock interview 3: mixed and timed | Review | Timed set |
| 28 | Sun 22 Nov | Gap fixing from the mocks | Gap fixing | Gap fixing |
| 29 | Mon 23 Nov | Final readiness test (the 9 baseline questions + new ones) | Final coding test | Final timed set |
| 30 | Tue 24 Nov | Light revision, application plan | Light review | Light review |

**Score targets (overall, adjust as we go):**

| Checkpoint | Target |
| --- | --- |
| Day 7 | 40 |
| Day 14 | 52 |
| Day 21 | 62 |
| Day 29 | 70 or higher |

## Scoreboard

| Area | Baseline (22 Oct) | Latest | Target |
| --- | --- | --- | --- |
| Golang (production) | 20 | - | 71+ |
| Database | 25 | - | 71+ |
| IAM | 40 | - | 71+ |
| Azure | 30 | - | 71+ |
| AKS / Kubernetes | 10 | 73 | 71+ |
| Networking | 5 | - | 71+ |
| Redis | 35 | - | 71+ |
| System Design | 35 | - | 71+ |
| Leadership | 50 | - | 71+ |
| Golang DSA | not tested | - | 71+ |

Overall baseline: about 26/100. Bands: 0-40 Needs Improvement, 41-70 Developing, 71-85 Interview Ready, 86-100 Strong Hire.

## The 8-step troubleshooting framework (use it in every answer)

1. Impact: who is affected, how badly, since when?
2. What changed: deployment, config, certificate, infra, traffic?
3. Which layer: client, network, app, database, platform, identity?
4. Evidence: logs, metrics, traces, events. Never guess without data.
5. Hypotheses: 2 or 3, ranked.
6. Validate: one quick test per hypothesis.
7. Mitigate: restore service first (rollback, scale, failover).
8. Prevent: alert, test, automation, runbook.

---

# Day 0: Baseline Diagnostic (22 Oct 2026)

Q1 (Golang) and Q5 (AKS) get their full lessons in the Day 1 session below, because they are today's topics.

## Q1 Golang: goroutine leak (20/100)

**Question:** Your payment service's memory grows steadily from 300 MB to 2 GB over 6 hours, then gets OOMKilled. CPU is normal. `runtime.NumGoroutine()` keeps climbing. What's your investigation path, the likely causes, the fix and the prevention?

**Your answer:** I will check AKS logs, then memory and processor consumption. It might error not handle in gorutine.

**Full lesson:** added in the Day 1 session below.

## Q2 Database: connection pool timeouts (25/100)

**Question:** At month-end, your payment API's p99 latency jumps from 200 ms to 12 s. DB CPU is only 40%, but the app logs show `timeout waiting for connection from pool`. What do you check, in what order?

**Your answer:** My hypotheses are database connection pool exhaust, and network related issue.

**What was good:** You named pool exhaustion.

**What was missing:** Why the pool is exhausted, and an investigation order. "Network" was a guess with no evidence.

**Model answer:**

The clue is **DB CPU 40% but the app waits for a connection**. The database is not busy; connections are being held and not given back. Check in this order:

1. Pool metrics: wait time, in-use vs idle, pool max size.
2. Pool size vs the database's max connections (all app pods together).
3. What holds connections: active sessions, long transactions, `idle in transaction`, slow queries.
4. Month-end jobs (reports, batches) competing with API traffic on the same pool or database.
5. Connection leaks: rows or transactions not closed or committed in code.
6. Locks: one blocked query can hold many connections.

Fix: index or rewrite slow queries, shorten transactions, always release connections (`defer rows.Close()`), size the pool deliberately, move reports to a read replica or a separate pool, add statement timeouts. Prevent: alert on pool wait time, not only on DB CPU.

**Example:** A pool of 20 connections. A month-end report opens a transaction and holds 15 connections for 30 seconds while it waits on a lock. The API has only 5 left, so requests queue and p99 jumps to 12 s, yet DB CPU stays low because nothing is computing.

```
API requests --> [ POOL: 20 connections ] --> Database (CPU 40%)
                    |  15 held by report job (waiting on lock)
                    |   5 left for the API
                    v
            new requests wait in queue  -->  "timeout waiting for connection"
```

**Remember it:** Low DB CPU plus pool timeouts means connections are held, not that the DB is busy. Find who holds them.

## Q3 IAM: intermittent 401 (40/100)

**Question:** Users report intermittent `401 Unauthorized`. SSO login works, but about 5% of API calls with valid-looking JWTs fail. It started after last night's deployment. How do you investigate? Top 3 hypotheses.

**Your answer:** 1) Compare my latest code with my last version. 2) Check the logs in App Insights. 3) Check those 5% API calls have valid audience or not, because APIs allow only valid audience.

**What was good:** The audience check is a real hypothesis, and App Insights is the right place for evidence.

**What was missing:** The clue **5% intermittent** was not used. It means some instances or tokens behave differently from the rest. Code comparison is slow and is not first in an incident.

**Model answer:**

First group the failing requests by pod, by token issuer and by token issue time. Then read the 401 reason in the logs: invalid audience, invalid issuer, invalid signature, or expired. Top 3 hypotheses:

1. **Partial rollout or config drift:** some pods run the new version or a wrong setting (audience, issuer, authority URL).
2. **Signing key rotation and a stale JWKS cache:** pods cached the old public keys, so tokens signed with the new key fail on those pods only.
3. **Clock skew:** `exp` or `nbf` checks fail on nodes whose clocks are off.

Mitigate: roll back or restart the stale pods, then prevent with JWKS refresh-on-unknown-key and a config check in the pipeline.

**Example:** The identity provider rotated its signing key at midnight. Pods A and B refreshed their key cache; pod C cached the old keys for 24 hours. Any request landing on pod C with a new token gets 401, which looks like "5% random failures".

```
Client --> Load balancer --> Pod A (new keys)  --> 200 OK
                         --> Pod B (new keys)  --> 200 OK
                         --> Pod C (OLD cached keys) --> 401  (about 1 in 3 requests)
```

**Remember it:** "Intermittent" means find what differs between the pods or requests that fail.

## Q4 Azure: Key Vault 403 (30/100)

**Question:** Your AKS app suddenly gets `403 Forbidden` from Key Vault. It worked yesterday, and nobody admits to changing anything. Investigation, causes, fix, prevention.

**Your answer:** I will check is that secret key expired, does someone change the RBAC of key vault or any type in key vault name.

**What was good:** RBAC changes are a valid check.

**What was missing:** An expired secret does not cause a 403. You did not use the hint "nobody admits to changing anything", which points to the Activity Log.

**Model answer:**

1. **Azure Activity Log** on the Key Vault and its resource group: who changed what, and when. Start here.
2. **Identity path:** is the workload identity or managed identity still valid? Check the federated credential, the service account annotation and the client ID.
3. **Permission model:** RBAC role assignment (for example Key Vault Secrets User) vs access policies. Was the vault switched from one model to the other, or a role removed?
4. **Network:** Key Vault firewall or private endpoint. A network rule can produce a 403 even when permissions are correct.
5. **Time-limited access:** a role assignment that expired (for example through PIM) or an Azure Policy change.

Fix: restore the assignment or rule. Prevent: alerts on RBAC and firewall changes, infrastructure as code with drift detection.

**Example:** Someone tightened the Key Vault firewall to "selected networks" and forgot the AKS subnet. Identity and RBAC are fine, but every call from the cluster returns 403.

```
Pod --> service account token --> Entra ID (federation) --> access token
   --> Key Vault: [1] network/firewall check  [2] RBAC check  --> secret
            403 can come from step 1 OR step 2
```

**Remember it:** 403 means "I know who you are, but you are not allowed". Check the Activity Log, then the network rules, then the roles.

## Q5 AKS: Pending pods (10/100)

**Question:** After a deployment, new pods are stuck in `Pending` while old pods are still serving traffic. Business is asking for an ETA. What's your investigation order? Give 4 distinct causes.

**Your answer:** 1) Old image not purge. 2) New image not built and ACR not reload.

**Full lesson:** added in the Day 1 session below.

## Q6 Networking: intermittent partner API hangs (5/100)

**Question:** Intermittently, the app can't reach an external partner API: some requests hang for 30 s, others work. The app runs in an AKS pod and the partner whitelists one IP. Where do you look?

**Your answer:** I dont know.

**What was good:** An honest "I don't know" is better than a confident wrong answer.

**Model answer:**

"Some hang, some work" plus "AKS outbound" points first to **SNAT port exhaustion**. Check in this order:

1. **SNAT:** Azure Load Balancer or NAT Gateway metrics for SNAT connections and failed connections.
2. **Connection behaviour in the app:** is a new HTTP client or connection created per request? Is keep-alive reused?
3. **Outbound IP:** is all traffic really leaving via the whitelisted IP? A second path means the partner blocks some requests.
4. **DNS:** CoreDNS latency or failures from the pod.
5. **Partner side:** rate limiting or its own timeouts.

Fix: reuse connections (one shared HTTP client), use a **NAT Gateway with a static public IP** (this also solves the whitelisting), set timeouts, retry with backoff.

**Example (Go):** Creating `&http.Client{}` inside the request handler opens a new connection every call, which burns outbound ports. At peak, ports run out and new connections wait about 30 seconds until they time out.

```
Pods --> Node --> NAT Gateway / Load Balancer --> Internet --> Partner
                       |
               limited outbound ports per IP
   many short connections --> ports run out --> new connections hang ~30 s
```

**Remember it:** Intermittent outbound hangs on AKS: suspect SNAT exhaustion first, and fix with connection reuse plus a NAT Gateway.

## Q7 Redis: cache stampede (35/100)

**Question:** You use Redis cache-aside for account balance lookups. At 9 AM a popular key expires and the DB gets hammered. What's happening, how do you prevent it, what are the trade-offs?

**Your answer:** 1) Investigate setup of the redis key TTL. 2) Make sure when key expired then key insertion/update will happen by cron job.

**What was good:** Checking TTL is right, and refreshing keys in advance is a real pattern.

**What was missing:** The problem has a name: **cache stampede** (thundering herd). Trade-offs were not discussed.

**Model answer:**

When a hot key expires, thousands of requests miss the cache at the same moment and all hit the database together.

| Fix | How it works | Trade-off |
| --- | --- | --- |
| TTL jitter | Add a random amount to each TTL so keys do not expire together | Slightly inconsistent freshness |
| Single-flight or lock | Only one request rebuilds the key; others wait | Waiting adds latency; lock must expire safely |
| Stale-while-revalidate | Serve the old value while one request refreshes | Users may see slightly old data (check this is acceptable for balances) |
| Pre-warm by cron (your idea) | Refresh popular keys before expiry | Single point of failure; does not scale to many keys |

**Example (Go):** `golang.org/x/sync/singleflight` makes 1,000 simultaneous requests for the same key share one database call.

```
Before: 1000 requests --> cache MISS --> 1000 DB queries  (DB overloaded)
After:  1000 requests --> cache MISS --> 1 DB query, 999 wait for its result
```

**Remember it:** Hot key expires, DB melts: name it a cache stampede, then give jitter, single-flight and stale-while-revalidate, with trade-offs.

## Q8 System Design: payment gateway (35/100)

**Question:** Design a payment gateway: 2,000 TPS peak, never double-charge, survive a regional outage. Give components, data flow, how you guarantee idempotency, and what happens when the bank times out after debiting the customer.

**Your answer:** 1) Pods will auto-increase if load comes. 2) Load balancing in place by region. 3) Every transaction should have unique status/id, and before creating a new transaction we check the time and status of the transaction. 4) Automatic retry mechanism should be in place to prevent debiting customer twice.

**What was good:** Unique transaction IDs, status checks and autoscaling are the right instincts.

**What was missing:** Point 4 is backwards: retries cause double debits unless the operation is idempotent. There was no data flow and no answer to the bank timeout.

**Model answer:**

Components: API gateway and load balancer, payment service (stateless, autoscaled), idempotency store, transaction database with a state machine, message queue with the outbox pattern, bank connector, reconciliation job, and a second region.

**Idempotency:** the client sends an `Idempotency-Key`. The service stores it with the request result in one atomic step. A repeated key returns the stored result and never debits again.

**Transaction states:** `INITIATED -> SENT_TO_BANK -> SUCCESS | FAILED | UNKNOWN`.

**Bank timeout after debit:** a timeout does not mean failure. Mark the payment `UNKNOWN`, do **not** create a new payment, query the bank status API using the same reference, and let a reconciliation job settle anything still unknown. Only then respond success or failure, or retry with the **same** key.

**Regional outage:** replicate the database across regions, use a global traffic manager, and make queues durable so in-flight payments resume after failover.

**Example:** A customer pays 5,000 rupees. The bank debits the account but the response times out. The system marks the payment `UNKNOWN`, asks the bank for the status, learns it was debited, marks `SUCCESS` and never sends a second debit.

```
Client --key K1--> Gateway --> Payment service --> check key K1 in DB
                                  | (new)               | (seen) --> return stored result
                                  v
                         state INITIATED --> bank call
                                  | timeout
                                  v
                         state UNKNOWN --> status query / reconciliation
                                  |
                         SUCCESS or FAILED (single final result for K1)
```

**Remember it:** Retries are safe only with an idempotency key. A timeout is UNKNOWN, not failed.

## Q9 Leadership: P1 at 2 AM (50/100)

**Question:** P1 incident at 2 AM, customers cannot pay. Your best engineer wants a rollback, the product manager wants a hotfix to keep a promised feature, the CTO pings every 5 minutes. How do you lead, what happens in the first 15 minutes and after?

**Your answer:** 1) Payment is basic and essential service, we cannot compromise, so we will identify how many customer complaints. 2) Instead of hot fix we will rollback if all customer impact PR and make system stable first. 3) For CTO I will give timely update and action item to stop issue.

**What was good:** Stability first and choosing rollback is a sound decision. Planning CTO updates is right.

**What was missing:** Named roles, a single communication channel, a time-boxed decision, how to answer the product manager, and the entire post-incident part.

**Model answer:**

```
0-5 min    Declare P1. Name an incident commander (IC). Open one war room channel.
5-10 min   Measure impact. Ask "what changed?" Decide: rollback or hotfix, time-boxed.
10-15 min  Execute the rollback. Send the first update. Fix an update interval.
After      Verify stability, then a blameless RCA with owners and dates.
```

CTO: stop 5-minute pings by promising a fixed update rhythm (for example every 15 minutes) with facts only. Product manager: "A rollback restores payments now and is the lowest-risk path. We ship the feature again after a tested fix." After the incident: blameless RCA within 48 hours, action items with owners, and a safer release plan for the feature.

**Example:** At 2:10 AM payments fail after a release. The IC sets a rule: if error rates do not drop within 15 minutes of the hotfix attempt, roll back. The hotfix does not work, so the team rolls back at 2:25 and payments recover at 2:30. The feature ships again two days later behind a feature flag.

**Remember it:** Restore service first, communicate on a schedule, and always finish with the RCA.

---

# Day 1 (26 Oct 2026)

**Plan:** Revision (10 min), core topic: AKS Pending pods (30 min), Golang coding: goroutine leak (25 min), production scenario (15 min), feedback (10 min).

**Status:** Step 1, revision question 1 (Pending vs ImagePullBackOff): waiting for your answer. Entries for the core topic, the coding task and the scenario are added here as we finish each one.

## Day 1 note: schedule shift and repo status (26 Oct 2026)

**Schedule shift:** 23 to 25 Oct were missed, so Day 1 starts on Mon 26 Oct 2026 and Day 30 is Tue 24 Nov 2026. Day 0 (the baseline) stays 22 Oct. The Master schedule above has the new dates.

| Checkpoint | New date | Target |
| --- | --- | --- |
| Day 7 re-test | Sun 1 Nov | 40 |
| Day 14 re-test, start applying | Sun 8 Nov | 52 |
| Day 21 re-test | Sun 15 Nov | 62 |
| Day 29 final test | Mon 23 Nov | 70 or higher |

**Repo review (kube-interview-prep): done from the uploaded zip; the repo became readable (public) on 26 Oct and matches the zip.** The repo could not be opened: a page fetch failed and a clone asked for a GitHub login, which usually means the repo is private or the path differs. Nothing in it has been reviewed, and the lab results you added have not been checked.

**To unblock it, do one of these:**

1. Zip the repo folder (or just the `.md` files with your results) and upload it in the chat.
2. Paste the file list (`ls` or `tree`) plus 2 or 3 result files.
3. Make the repo public for a while.

**What happens once I can read it:**

1. Each lab result is checked against the Events and output it should have shown, and wrong readings are flagged.
2. The repo's scenarios are compared with the checklist below, and the gaps are listed.
3. The verified results are logged in this Day 1 section.

**Kubernetes scope checklist (tick what the repo already covers):**

| Area | Scenarios to have done |
| --- | --- |
| Scheduling | Pending pods: insufficient resources, taints, affinity, unbound volume |
| Pod failures | CrashLoopBackOff, OOMKilled, ImagePullBackOff, failing probes (liveness, readiness, startup) |
| Rollouts | maxSurge and maxUnavailable, rollout undo, stuck rollout |
| Networking | Service not reaching pods (wrong selector), DNS failure, Ingress, NetworkPolicy blocking traffic |
| Scaling | HPA not scaling, cluster autoscaler, requests vs limits |
| Config and secrets | ConfigMap and Secret mounted wrong, missing env var |
| Storage | PVC stuck Pending, StorageClass |
| Nodes | Node NotReady, cordon, drain, NoExecute taint |
| Security | RBAC and ServiceAccount "forbidden" |
| AKS-specific | Workload identity to Key Vault, node pool limits, subnet IP exhaustion, SNAT, upgrades |

The rows for pod failures and rollouts are the most common interview scenarios. The AKS row matters most for Azure roles and cannot be practised on a local kind cluster, so it needs theory plus a short Azure lab.

**Self-test (answered, see the next section):** A pod shows CrashLoopBackOff. What are the first three commands you type, in order, and what is each one looking for?

## Self-test: CrashLoopBackOff first three commands (35/100)

**Question:** A pod shows CrashLoopBackOff. What are the first three commands you type, in order, and what is each one looking for?

**Your answer:**

1. `kubectl get all -n practice -o wide` to see the container status of each pod
2. `kubectl describe deployment/payments-service -n practice` to get the exact error from the container
3. `kubectl rollout status deployment/payments-service -n practice` to get the status of the container

**What was good:** Step 1 is reasonable, and you always include the namespace.

**What was wrong:**

- Step 2 looks at the wrong object. `describe deployment` shows replica counts and the update strategy, not why a container crashed. The container error lives on the **pod**.
- Step 3 shows rollout progress, not container health, and it just waits if the rollout is stuck. It does not diagnose anything.
- Nothing reads the logs, and the logs are where a crash reason usually is. Your own repo note says it: start narrow at the broken object, then go wide.

**Model answer:**

1. `kubectl get pods -n practice -o wide`: confirm STATUS `CrashLoopBackOff`, read the RESTARTS count, and see which node.
2. `kubectl describe pod <pod> -n practice`: read **Last State: Terminated** (Reason and **Exit Code**) and the Events at the bottom.
3. `kubectl logs <pod> -n practice --previous`: the output of the container that just crashed. Plain `logs` often shows nothing because the new container only just started.

| Exit code or reason | Usual meaning |
| --- | --- |
| 1 (Error) | The app crashed: bad config, missing env var, failed dependency |
| 137 / OOMKilled | The container hit its memory limit |
| 127 or 126 | Command not found or not executable (bad entrypoint) |
| Liveness probe failed (in Events) | Kubernetes keeps killing a container that is alive but failing its probe |

```
container starts --> crashes --> kubelet restarts it
        ^                               |
        |   wait 10s, then 20s, 40s ... up to 5 min  (the "BackOff")
        +-------------------------------+
STATUS flips between Running / Error / CrashLoopBackOff
```

**Example:** A payments pod needs `DB_URL`, but the Secret is missing the key. The app logs `DB_URL not set` and exits with code 1. `kubectl logs --previous` shows that line. The fix is the Secret, not the image.

**Remember it:** get pods, describe pod, logs --previous. Work at the pod, not at the deployment.

## Repo review: kube-interview-prep, Lab 1 (26 Oct 2026)

**Overall:** A strong start. The notes use real outputs, good analogies, an investigation path and interview answer templates. The Q&A with scores is exactly the right habit.

**What is in the repo:** Labs 1.1 (Pending pods), 1.2 (taints and tolerations), 1.3 (rolling updates, bad image, readiness probe).

**Fixes to make:**

| # | Issue | Fix |
| --- | --- | --- |
| 1 | `01-pending-pods/01-broken-resource-request.yaml` contains the **fixed** values (250m, 128Mi), not the broken ones (10 CPU, 32Gi) | Save two files: a broken one and a fixed one, so the lab can be repeated |
| 2 | The notes and README name `04-fixed-readiness.yaml`, but the repo has `04-broken-readiness.yaml` with the fixed probe inside | Rename it to `04-fixed-readiness.yaml` |
| 3 | Lab 1.3 Step 1 says "to be filled", yet the notes end "Complete and Verified" | Paste the real output, or remove the claim |
| 4 | Lab 1.3 Step 2 shows `get pods` but not the `describe pod` Events for the bad image | Add the Events (ErrImagePull, then ImagePullBackOff). They are the key learning |
| 5 | The Lab 1.1 table says nodes have 8 CPU and 7.6 Gi, but the text says about 2 CPUs and 8 GB | Paste the real `describe node` Allocatable block. On kind, every node reports the host's resources |
| 6 | Lab 1.1 shows no output after the fix | Add the `get pods` output showing Running |
| 7 | Several files use Windows line endings | Convert them to LF (for example `dos2unix`) or add a `.gitattributes` |
| 8 | Dates in the notes all say 23 Oct | Use the real date of each lab. This tracker counts Day 1 from 26 Oct |

**Technical corrections:**

- **Pod anti-affinity diagram (Lab 1.2, Q4).** With `requiredDuringSchedulingIgnoredDuringExecution` on hostname and only 2 eligible nodes, 3 replicas leave the **third pod Pending**. The diagram showing 2 pods on one node contradicts the rule. For "spread but do not block", use `topologySpreadConstraints` (maxSkew 1, `whenUnsatisfiable: ScheduleAnyway`) or a **preferred** anti-affinity.
- **"Rollout paused".** A rollout with a bad image **stalls**; it is not paused. `kubectl rollout pause` is a separate feature. After `progressDeadlineSeconds` (default 600 s) the Deployment reports `ProgressDeadlineExceeded`, and it still never rolls back by itself.
- **Lab 1.3 Q3** says the bad update gives CrashLoopBackOff, but your lab produced ErrImagePull and ImagePullBackOff. Those are different failures. CrashLoopBackOff means the container starts and then exits.
- **AKS system pool taint.** The notes say AKS auto-taints the system pool with `CriticalAddonsOnly`. As far as I know, that taint is recommended and you add it yourself, while spot pools do get their taint automatically. Check Microsoft's AKS docs to confirm before you say it in an interview.
- **Toleration does not attract.** A toleration only allows a pod onto a tainted node. Add that line to the "Key mistakes" table.
- **Privacy:** the repo contains `.claude-plugin/` and `skills/interview-ready/` with personal background details. If the repo is public, consider removing them or adding them to `.gitignore`.

**Coverage against the Kubernetes checklist:**

| Area | Status |
| --- | --- |
| Scheduling: Pending, resources, taints | Done (Labs 1.1, 1.2) |
| Rollouts: bad image, undo, readiness probe | Done (Lab 1.3) |
| Pod failures: CrashLoopBackOff, OOMKilled, liveness probe | **Missing** (only a liveness vs readiness table) |
| Networking: Service selector, DNS, Ingress, NetworkPolicy | Missing |
| Nodes: cordon, drain, PodDisruptionBudget, anti-affinity or spread as a real lab | Missing (anti-affinity is theory only) |
| Scaling: HPA, requests vs limits | Missing |
| Config, secrets, storage: ConfigMap, Secret, PVC | Missing |
| Security: RBAC, ServiceAccount | Missing |
| AKS-specific | Missing (theory plus a short Azure lab) |

**Next labs, in order:**

1. **Lab 1.4:** CrashLoopBackOff and OOMKilled (and a failing liveness probe)
2. **Lab 1.5:** Service not reaching pods (wrong selector) and DNS
3. **Lab 1.6:** cordon, drain and PodDisruptionBudget, with a topology spread of replicas
4. **Lab 1.7:** HPA, ConfigMap and Secret mistakes

## Day 1 assignments (26 Oct 2026): do on your own, then share the results

### Pending for Day 1

| Item | Status | Time |
| --- | --- | --- |
| Revision (3 questions) | Done (30, 50, 65) | - |
| Core topic: Pending pods, taints, rolling updates | Done (check question 35) | - |
| Labs 1.1 to 1.3 in the repo | Done | - |
| CrashLoopBackOff self-test | Done (35) | - |
| Lab 1.4: CrashLoopBackOff, 3 commands | Pending | 20 min |
| Command drill: type 5 commands from memory | Pending | 5 min |
| Golang assignment | Pending | 45 min |
| DSA assignment (2 problems) | Pending | 50 min |

### Golang assignment: find and fix a goroutine leak

**Part A, in 4 lines:** What is a goroutine, and what can make one never finish?

**Part B, run this program** (`go mod init leak` then `go run .`):

```go
package main

import (
	"fmt"
	"runtime"
	"time"
)

func fetchPrice(id int) <-chan int {
	ch := make(chan int) // unbuffered
	go func() {
		time.Sleep(time.Duration(id%3) * 300 * time.Millisecond)
		ch <- id * 10 // blocks forever if nobody receives
	}()
	return ch
}

func getPrice(id int) (int, error) {
	select {
	case p := <-fetchPrice(id):
		return p, nil
	case <-time.After(200 * time.Millisecond):
		return 0, fmt.Errorf("timeout")
	}
}

func main() {
	for i := 0; i < 300; i++ {
		_, _ = getPrice(i)
	}
	time.Sleep(time.Second)
	fmt.Println("goroutines:", runtime.NumGoroutine())
}
```

**Tasks:**

1. **Predict** the printed goroutine count before you run it, and say why.
2. Run it and record the output.
3. Explain the leak in 3 lines: which goroutines are stuck, where, and why.
4. Fix it in **two different ways** (for example a buffered channel, and a `context` with cancellation). Run again and record the output.
5. Interview version: explain the problem and the fix in 60 seconds, as if to an interviewer.
6. **Stretch:** add `net/http/pprof`, run the leaking version and open `/debug/pprof/goroutine?debug=1` to see where they are parked.

### DSA assignment: two baseline problems (25 minutes each, Go, no looking up solutions)

**Problem 1 (Easy), Two Sum.** Given a slice of transaction amounts and a target refund amount, return the indices of two different transactions that add up to the target. Exactly one answer exists.

- Example: `[2, 7, 11, 15]`, target 9, returns `[0, 1]`
- Required: write the brute force first and state its complexity, then an O(n) solution
- Test: `[3, 3]` with target 6, negative numbers, and what you return when there is no pair

**Problem 2 (Easy to Medium), Group Anagrams.** Given a slice of strings, group the ones that are anagrams of each other. Order does not matter.

- Example: `["eat","tea","tan","ate","nat","bat"]` returns `[["eat","tea","ate"],["tan","nat"],["bat"]]`
- Required: state the time and space complexity of your key choice
- Test: empty slice, a single string, the empty string, duplicates

### What to send back

1. Your Go code for each problem
2. The `go test -v` output (use a table-driven test)
3. Time and space complexity, in one line each
4. How long each took, and where you got stuck
5. For the Golang assignment: the before and after goroutine counts and your explanations

## Revision Q1: Pending vs ImagePullBackOff (30/100)

**Question:** A pod is stuck in `Pending` and another is stuck in `ImagePullBackOff`. What's the difference, and which component handles each?

**Your answer:** ImagePullBackOff = Image not load from ACR. Pending = Not in readiness state.

**What was good:** ImagePullBackOff means the image could not be pulled from the registry. That part is right.

**What was missing:** "Pending = not in readiness state" is wrong. Readiness is a different thing: a pod that is Running but shows `0/1` READY is failing its readiness probe. You also did not name the components.

**Model answer:**

- **Pending (no node assigned):** the pod exists, but the **kube-scheduler** has not placed it on a node. Typical reasons: not enough CPU or memory, taints, node selector or affinity rules, an unbound volume, or a quota.
- **ImagePullBackOff:** the pod is already on a node, and the **kubelet** (with the container runtime) cannot pull the image. Typical reasons: wrong image name or tag, no permission on ACR, or a network problem to the registry.
- **Fast check:** `kubectl get pod <name> -o wide`. If the NODE column is `<none>`, it is a scheduling problem. If a node is shown, it is a node-level problem such as an image pull.

**Example:** You deploy a pod that requests 4 CPU, but every node has only 2 CPU free. Output: `NODE <none>`, and the Events say `0/3 nodes are available: 3 Insufficient cpu`. That is a scheduler problem, and a new image will not fix it. If instead the Events say `Failed to pull image ... unauthorized`, the pod is on a node and the kubelet cannot authenticate to ACR.

```
kubectl apply
   |
   v
API server stores pod  -->  kube-scheduler picks a node
                               |
               no node fits    |   node found
                   |           v
                   v      kubelet on that node
             PENDING           |  pulls image from ACR
         (NODE = none)         |       |
                               |   fails: ImagePullBackOff
                               v
                          container starts --> Running
                               |
                      readiness probe passes --> READY 1/1
```

**Remember it:** NODE `<none>` means scheduler problem (Pending). A node is shown but the image fails means kubelet problem (ImagePullBackOff). Readiness is a separate step after the pod is Running.

## Revision Q2: The 8 troubleshooting steps (50/100)

**Question:** Write the 8 steps of the troubleshooting framework, in order, from memory.

**Your answer:** 1) What is breaking 2) Impact of the bug 3) Log analysis 4) Execute the rollback plan which already defined 5) Blameless RCA

**What was good:** You remembered Impact, Evidence (log analysis), Mitigate (rollback) and the closing step (RCA). These come from real experience.

**What was missing:** What changed, Which layer, Hypotheses, and Validate. You went straight from logs to rollback. Impact should also come first: measure who is affected before you diagnose.

**Model answer:**

| # | Step | Question to ask |
| --- | --- | --- |
| 1 | Impact | Who is affected, how badly, since when? |
| 2 | What changed | Deployment, config, certificate, infra, traffic? |
| 3 | Which layer | Client, network, app, database, platform, identity? |
| 4 | Evidence | What do logs, metrics, traces and events say? |
| 5 | Hypotheses | What are the top 2 or 3 causes, ranked? |
| 6 | Validate | What quick test proves or kills each one? |
| 7 | Mitigate | Can I restore service now (rollback, scale, failover)? |
| 8 | Prevent | What alert, test, automation or runbook stops a repeat? |

**Memory trick:** I Can Locate Every Hidden Visible Major Problem (Impact, Change, Layer, Evidence, Hypotheses, Validate, Mitigate, Prevent).

**Note:** In a P1 you may do step 7 early. If a recent release is the clear cause, roll back first and investigate afterwards.

**Example (your Q2, pool timeouts at month-end):**

1. Impact: 12 s p99, payments slow, since month-end started.
2. What changed: month-end batch job started at 00:00.
3. Which layer: database connections, not app CPU.
4. Evidence: pool wait metric high, DB CPU only 40%.
5. Hypotheses: report job holds connections; pool too small; slow query.
6. Validate: check active sessions and long transactions.
7. Mitigate: pause the batch job or move it to a read replica.
8. Prevent: alert on pool wait time, separate pool for reports.

```
Impact -> Change -> Layer -> Evidence
                              |
                              v
            Hypotheses -> Validate -> Mitigate -> Prevent
                  ^           |
                  +-----------+  (not proven? try the next hypothesis)
```

**Remember it:** Measure impact first, find what changed, narrow the layer, prove with evidence, then fix, then prevent.

## Revision Q3: Critique of your Day 0 Q5 answer (65/100)

**Question:** If you were the interviewer, what is the biggest thing wrong with your Q5 answer (old image not purged, new image not built)?

**Your answer:** I jump on the action rather than analysis.

**What was good:** Strong self-awareness. You named your main pattern correctly, and that is the hardest part to see.

**What was missing:** The specific technical error. The answer blamed images, but `Pending` means the scheduler could not place the pod, so images are not the cause. A first step like `kubectl describe pod` would have shown this in seconds.

**Remember it:** Describe first, act second. Read the pod Events before you touch anything.

---

## Core topic: Why pods stay Pending (source: TechOps Examples #393 for placement rules; resources, autoscaler and IP parts are added by the coach)

**The scheduler works in two stages:** it first **filters out** nodes that cannot run the pod, then **scores** the remaining nodes and picks the best one. If the filter leaves zero nodes, the pod stays Pending and the Events say why.

```
Pod (requests: 2 CPU, 4 GB, nodeSelector, tolerations)
        |
        v
  FILTER: remove nodes that fail
    - not enough CPU / memory
    - taint the pod does not tolerate
    - nodeSelector / affinity not matching
    - volume cannot attach
        |
   0 nodes left          some nodes left
        |                       |
        v                       v
     PENDING           SCORE -> pick best node -> kubelet starts the pod
```

**Investigation order**

1. `kubectl describe pod <name>` and read the Events at the bottom. They name the reason.
2. `kubectl get pod <name> -o wide`: is NODE `<none>`?
3. `kubectl describe node <node>`: compare allocatable with allocated requests.
4. Check taints, node selector and affinity.
5. Check PVC status (`kubectl get pvc`).
6. Check quotas and, on AKS, the autoscaler and the node pool limits.

**Common causes**

| Cause | What the Event says | Typical fix |
| --- | --- | --- |
| Not enough CPU or memory | `Insufficient cpu` or `Insufficient memory` | Lower the requests, add nodes, scale the node pool |
| Taint not tolerated | `had untolerated taint` | Add a toleration, or use a different node pool |
| nodeSelector or affinity mismatch | `didn't match Pod's node affinity/selector` | Fix the labels or the rule |
| Volume not bound | `unbound immediate PersistentVolumeClaims` | Fix the storage class or the PVC |
| Quota exceeded | `exceeded quota` | Raise the quota or free resources |
| Node pool at max, or no free IPs (AKS, Azure CNI) | Autoscaler cannot add a node | Raise the max node count or enlarge the subnet |

**Example:** A rolling update asks for 3 new pods with 2 CPU each. The 3 nodes have 1.5 CPU free each, and the old pods still hold their CPU while they serve traffic. The new pods stay Pending with `Insufficient cpu`. The autoscaler would add a node, but the node pool is already at its maximum, so nothing happens. Fix: raise the pool max, or lower the requests.

**Remember it:** Pending equals the scheduler found zero fitting nodes. Run describe, read the Events, and the reason is written there.

## Core check: reading a Pending event (35/100)

**Question:** Pods are Pending after a deployment. The Events say: `0/4 nodes are available: 2 Insufficient memory, 2 node(s) had untolerated taint.` What does it tell you, and what do you do next, in order?

**Your answer:** 4 nodes should be running but 2 have insufficient memory because CPU is not available, old node still using CPU. Next: 1) check the node information 2) kill old pods one by one after a specific interval. Note: I do not remember kube commands because of less knowledge and practice.

**What was good:** "Check the node information" is the right first move, and you connected the problem to old pods still holding resources. You were also honest about the command gap.

**What was missing:**

- The message says **memory**, not CPU. Mixing the two up in an interview costs points.
- You ignored the second half: **2 nodes have a taint the pod does not tolerate**. That is a different problem from memory and needs a different fix.
- Killing old pods one by one removes serving capacity in production and does not fix the taint half.

**Model answer:**

The cluster has 4 nodes. The scheduler rejected all of them: 2 because the pod's memory request does not fit in their free memory, and 2 because they carry a taint (for example a dedicated node pool) and the pod has no matching toleration.

```
0/4 nodes available
   |-- Node 1, Node 2: memory request > free memory   -> resource problem
   |-- Node 3, Node 4: taint, no toleration           -> placement problem
```

Next steps, in order:

1. `kubectl describe node` on nodes 1 and 2: compare **Allocatable** with **Allocated resources** for memory.
2. `kubectl describe node` on nodes 3 and 4: read the **Taints** line. Should this pod run there? If yes, add a toleration. If not, leave it.
3. Check the pod's memory request. Is it higher than needed? Lower it if so.
4. If real capacity is short, scale the node pool (check the autoscaler and the pool's max count).
5. Prevent: set `maxSurge` and `maxUnavailable` on the rolling update, keep capacity headroom, and alert on pods Pending for more than 5 minutes.

**Example:** A pod needs 6 GB of memory. Nodes 1 and 2 have 4 GB free because old pods still run during the rollout. Nodes 3 and 4 belong to a `gpu` pool tainted for GPU workloads only. Neither pair fits, so the pod is Pending. The fix is to lower the request to what the app really uses, or add a node to the normal pool.

**Remember it:** Read each part of the Event separately. Every number is a different reason, and each reason has its own fix.

## Command cheat sheet (the ones to know cold)

| Command | What it tells you |
| --- | --- |
| `kubectl get pods -o wide` | Pod status and which node it is on (NODE `<none>` means unscheduled) |
| `kubectl describe pod NAME` | Events at the bottom explain why it is Pending or failing |
| `kubectl describe node NAME` | Taints, allocatable vs allocated CPU and memory |
| `kubectl get nodes` | Node list and Ready status |
| `kubectl top nodes` / `kubectl top pods` | Live CPU and memory use |
| `kubectl get events --sort-by=.lastTimestamp` | Recent cluster events in time order |
| `kubectl logs NAME` | Application logs (add `--previous` after a crash) |
| `kubectl get pvc` | Whether storage claims are Bound |
| `kubectl rollout status deployment/NAME` | Rollout progress |
| `kubectl rollout undo deployment/NAME` | Roll back to the previous version |

## Concept notes: taints, describe node, rolling updates

### 1. Taint and toleration

**Idea:** A **taint** is a "reserved" sign on a node: only pods with a matching permit may run there. A **toleration** is the permit on the pod. No permit means the scheduler skips the node, and the Event says `had untolerated taint`.

**Example:** The node pool `payments` is reserved for payment pods.

```
kubectl taint nodes NODE workload=payments:NoSchedule     (the sign on the node)

tolerations:                                                (the permit in the pod spec)
- key: "workload"
  operator: "Equal"
  value: "payments"
  effect: "NoSchedule"
```

| Effect | Meaning |
| --- | --- |
| NoSchedule | New pods without a permit are not placed here |
| PreferNoSchedule | The scheduler avoids it if it can, but may still use it |
| NoExecute | Pods without a permit are not placed, and running ones are evicted |

**Important:** A toleration only **allows** a pod onto a tainted node. It does not **attract** it there. To force pods onto that pool, also add a `nodeSelector` or node affinity.

**On AKS:** Pools are often tainted on purpose, for example a system pool with `CriticalAddonsOnly=true:NoSchedule` or a spot pool with a spot taint.

```
Pod (no permit) ----> [Node: taint workload=payments] X rejected
Pod (has permit) ---> [Node: taint workload=payments] allowed
Pod (no permit) ----> [Node: no taint]               allowed
```

### 2. What to read in `kubectl describe node`

This is a trimmed, illustrative example, not from your cluster:

```
Name:               aks-userpool-12345678-vmss000001
Labels:             agentpool=userpool
Taints:             workload=payments:NoSchedule        <-- (A) who is allowed here
Unschedulable:      false                                <-- (B) true = cordoned, nothing new is placed
Conditions:
  MemoryPressure    False                                <-- (C) all pressure False and Ready True = healthy
  DiskPressure      False
  Ready             True
Capacity:
  cpu:     4
  memory:  16393292Ki
Allocatable:                                             <-- (D) what pods can actually use
  cpu:     3860m
  memory:  12880Mi
  pods:    110
Allocated resources:                                     <-- (E) what pods have already RESERVED
  cpu       3500m (90%)
  memory    11500Mi (89%)
Events:     <none>                                       <-- (F) node-level problems show here
```

| Line | What to ask |
| --- | --- |
| A Taints | Does my pod have a matching toleration? |
| B Unschedulable | Is the node cordoned? |
| C Conditions | Is the node healthy (Ready True, no pressure)? |
| D minus E | How much is free? Here memory is 12880 - 11500 = 1380 Mi free |
| F Events | Any node-level warnings? |

**Key point:** The scheduler counts **requests**, not real usage. A pod asking for 2 Gi will not fit on this node even if the node's real memory use is low. The Event would read `Insufficient memory`.

### 3. maxSurge and maxUnavailable

**Idea:** These two numbers control how a rolling update replaces old pods with new ones.

- **maxSurge:** how many **extra** pods may exist above the desired count during the update.
- **maxUnavailable:** how many pods may be **missing** (not ready) during the update.

```
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
```

**Example:** 4 replicas, maxSurge 1, maxUnavailable 0.

```
Start:   [old][old][old][old]            4 serving
Step 1:  [old][old][old][old][NEW]       start 1 new pod (needs spare room on a node)
Step 2:  new pod becomes Ready           only now remove one old pod
Step 3:  [old][old][old][NEW]            4 serving again
...repeat until all 4 are new            capacity never drops below 4
```

| Setting | Effect | Trade-off |
| --- | --- | --- |
| maxUnavailable 0, maxSurge 1 | Capacity never drops. Safest for payments | Needs spare room for 1 extra pod; if there is none, the new pod is Pending and the rollout waits |
| maxUnavailable 1, maxSurge 0 | No extra room needed | Capacity drops by 1 during the update |

**How this prevents Pending problems:** Spare room needed = maxSurge x the pod's requests. Plan that headroom (or autoscaler capacity) before you roll out.

**Remember it:** Taint = the lock, toleration = the key. Allocatable minus Allocated = free room. maxSurge = extra pods allowed, maxUnavailable = missing pods allowed.

## Kubernetes basics in plain words

Read this once before the labs. Each term has a one-line meaning and a real-life picture.

| Term | Plain meaning | Picture |
| --- | --- | --- |
| Container | An app packaged with everything it needs to run | A lunch box with the meal inside |
| Docker | The tool that runs containers on one computer | The kitchen that heats the lunch boxes |
| Pod | The smallest thing Kubernetes runs; wraps one or more containers | A tray holding the lunch box |
| Node | One computer (physical or virtual) that runs pods | One table in the restaurant |
| Cluster | A group of nodes managed together | The whole restaurant |
| Control plane | The brain: stores the state and decides where pods go (the scheduler lives here) | The manager |
| Worker node | A node that runs your pods | The tables where food is served |
| kubectl | The command-line tool you type to talk to the cluster | The remote control |
| kind | "Kubernetes IN Docker": a free tool that makes a practice cluster on your laptop, where each node is really a Docker container | A toy restaurant on your desk |

```
Your laptop
  +-- Docker
        +-- container = control-plane node (lab-control-plane)   <- the brain
        +-- container = worker node 1     (lab-worker)           <- runs pods
        +-- container = worker node 2     (lab-worker2)          <- runs pods

You type kubectl ...  -->  talks to the control plane  -->  pods run on the workers
```

### Lab 0 commands, explained

| Command | What it does |
| --- | --- |
| `docker --version` | Shows that Docker is installed (creates nothing) |
| `kubectl version --client` | Shows that kubectl is installed (creates nothing) |
| `kind version` | Shows that kind is installed (creates nothing) |
| `cat > kind-3nodes.yaml <<EOF ... EOF` | Writes the lines between the two EOF markers into a text file named kind-3nodes.yaml |
| `kind create cluster --name lab --config kind-3nodes.yaml` | Uses kind to build a cluster named `lab`, with the shape described in the file |
| `kubectl get nodes` | Lists the nodes in the cluster |

The file, line by line:

```
kind: Cluster            # this file describes a cluster ("kind" here is just the field name for "type of thing")
apiVersion: kind.x-k8s.io/v1alpha4   # the file format version; required, just copy it
nodes:                   # the list of machines to create
- role: control-plane    # 1 brain
- role: worker           # worker machine 1
- role: worker           # worker machine 2
```

**Remember it:** kind builds a toy cluster inside Docker. kubectl is the remote control. The cluster has a brain (control plane) and workers that run the pods.

### What a cluster runs by itself (from your own `kubectl get pods -A`)

| Pod in kube-system | Job | Picture |
| --- | --- | --- |
| kube-apiserver | The front door. Every kubectl command goes through it | Reception desk |
| etcd | The database holding the cluster's state | The manager's notebook |
| kube-scheduler | Chooses a node for each new pod. **This is what leaves a pod Pending** | The seating host |
| kube-controller-manager | Keeps reality equal to what you asked for (for example 4 replicas) | The supervisor |
| kubelet (runs on every node, not as a pod) | Starts the pods on its node and pulls images | The waiter at each table |
| kube-proxy (one per node) | Network rules so Services reach pods | The hallway signs |
| coredns | DNS: turns service names into IP addresses | The phone book |
| kindnet | Pod networking (a CNI plugin) | The wiring |
| local-path-provisioner | Gives pods storage | The storage room |

**Reading the columns:** READY `1/1` means 1 of 1 containers is ready. RESTARTS `14 (30m ago)` means the container restarted 14 times, the last 30 minutes ago (here, when Docker or WSL was last restarted). AGE is how long the pod has existed.

**Remember it:** The scheduler is just a pod in kube-system. When a pod is Pending, the scheduler could not find a node for it.

## Lab guide 1: Pending pods, taints and rolling updates

**Goal:** Break things on purpose on a free local cluster, read the Events, and fix them. Total time: about 45 minutes. Do one lab at a time and write what you saw in the results table at the end.

**You need:** Docker running on your laptop, plus `kind` and `kubectl` installed. Alternative: `minikube start --nodes 3`, or a free browser Kubernetes playground (check that it is still available).

**Your cluster (found on 26 Oct):** You already have a working 3-node kind cluster named `k8s-lab` (nodes `k8s-lab-control-plane`, `k8s-lab-worker`, `k8s-lab-worker2`). Use it instead of creating `lab`. Wherever the labs say `lab-worker` or `lab-worker2`, type `k8s-lab-worker` or `k8s-lab-worker2`. Always run `kubectl config current-context` first (expected: `kind-k8s-lab`) and do the labs in a separate namespace named `practice`.

### Lab 0: Create a 3-node cluster

```
cat > kind-3nodes.yaml <<EOF
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
EOF

kind create cluster --name lab --config kind-3nodes.yaml
kubectl get nodes
```

**You should see:** 3 nodes named `lab-control-plane`, `lab-worker`, `lab-worker2`, all `Ready`.

### Lab 1: A pod that asks for too much memory

```
cat > big.yaml <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: big
spec:
  containers:
  - name: app
    image: nginx
    resources:
      requests:
        memory: "500Gi"
EOF

kubectl apply -f big.yaml
kubectl get pod big -o wide
kubectl describe pod big
```

**You should see:** STATUS `Pending`, NODE `<none>`. At the bottom of describe, an Event similar to `0/3 nodes are available: 1 node(s) had untolerated taint {node-role.kubernetes.io/control-plane: }, 2 Insufficient memory.`

**Learn:** This is the same pattern as your check question. The control-plane node is tainted by default (the taint half). The two workers cannot fit the 500 Gi request (the memory half). Read each number separately.

### Lab 2: Taint the workers, no toleration

```
kubectl delete pod big
kubectl taint nodes lab-worker workload=payments:NoSchedule
kubectl taint nodes lab-worker2 workload=payments:NoSchedule
kubectl run web --image=nginx
kubectl get pod web -o wide
kubectl describe pod web
```

**You should see:** `web` is `Pending`. The Event says all 3 nodes had untolerated taints: 1 for the control-plane taint and 2 for `{workload: payments}`.

### Lab 3: Add the toleration (the permit)

```
cat > web2.yaml <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: web2
spec:
  tolerations:
  - key: "workload"
    operator: "Equal"
    value: "payments"
    effect: "NoSchedule"
  containers:
  - name: nginx
    image: nginx
EOF

kubectl apply -f web2.yaml
kubectl get pods -o wide
```

**You should see:** `web2` is `Running` on `lab-worker` or `lab-worker2`, while `web` stays Pending. Same cluster, same nodes: the toleration is the only difference.

### Lab 4: Read a real describe node

```
kubectl describe node lab-worker | grep -A2 Taints
kubectl describe node lab-worker | sed -n '/Allocated resources/,/Events/p'
```

**You should see:** the `workload=payments:NoSchedule` taint, and the CPU and memory **requests** already reserved on that node. Compare Allocatable with Allocated and work out the free room.

### Lab 5: Watch a rolling update (maxSurge 1, maxUnavailable 0)

First clean up: remove the taints and the test pods.

```
kubectl delete pod web web2
kubectl taint nodes lab-worker workload=payments:NoSchedule-
kubectl taint nodes lab-worker2 workload=payments:NoSchedule-

kubectl create deployment shop --image=nginx:1.25 --replicas=4
kubectl patch deployment shop -p '{"spec":{"strategy":{"rollingUpdate":{"maxSurge":1,"maxUnavailable":0}}}}'
```

Open a second terminal and run `kubectl get pods -w`. In the first terminal:

```
kubectl set image deployment/shop nginx=nginx:1.26
kubectl rollout status deployment/shop
```

**You should see:** a 5th pod appears first, becomes Ready, and only then an old pod terminates. The count of Ready pods never drops below 4.

### Lab 6: A bad image (Pending vs ImagePullBackOff, side by side)

```
kubectl set image deployment/shop nginx=nginx:9.99
kubectl get pods -o wide
kubectl describe pod <the new pod name>
kubectl rollout undo deployment/shop
```

**You should see:** the new pod shows `ErrImagePull` then `ImagePullBackOff`, and it **has a NODE** (so the scheduler did its job). The 4 old pods stay `Running`, because maxUnavailable is 0. The rollout is stuck but users are safe. `rollout undo` brings it back.

**Learn:** Compare this with Lab 1, where there was no node. That is the whole Pending vs ImagePullBackOff difference, seen on screen.

### Clean up

```
kind delete cluster --name lab
```

### Your results

| Lab | What I saw (Event or status) | What I understood |
| --- | --- | --- |
| 1 Too much memory |  |  |
| 2 Taint, no toleration |  |  |
| 3 With toleration |  |  |
| 4 Describe node |  |  |
| 5 Rolling update |  |  |
| 6 Bad image |  |  |

---

# Remember-it cheat sheet

- DB: low DB CPU plus pool timeouts means connections are held, so find who holds them.
- IAM: intermittent means find what differs between failing and passing requests.
- Azure: 403 means allowed identity but denied action. Activity Log first, then network rules, then roles.
- Networking: intermittent outbound hangs on AKS mean suspect SNAT exhaustion; fix with connection reuse and a NAT Gateway.
- Redis: hot key expires and the DB melts is a cache stampede. Use jitter, single-flight, stale-while-revalidate.
- Design: retries are safe only with an idempotency key. A bank timeout is UNKNOWN, not failed.
- Leadership: restore service, communicate on a schedule, finish with a blameless RCA.

---

# Day 1 results: Golang and DSA (26 Oct 2026)

**Source:** your repo README (`lab1/`). Scores recorded there: Part A goroutine theory 42, Part B leak fix 78 (attempts: WaitGroup only 28, buffered channel 72, `context.WithTimeout` 78), 60-second explanation 48, Two Sum 68, Group Anagrams 55. Overall 60.8, Developing.

**What was strong:** predictions before running, a recorded attempt history, and the lesson "fix the leak at the source, in the goroutine that can block".

**Corrections and gaps:**

- Group Anagrams: `count[c-'a']` assumes lowercase a-z, and any other character causes an index-out-of-range panic. State the assumption, or use a sorted-runes string key (O(n k log k)) for general input. Expect this follow-up.
- Go map iteration order is random, so tests must sort the groups before comparing.
- Missing from the brief: `go test -v` output (table-driven) and time taken per problem. Prove `[3, 3]` and the no-pair case with tests.

**Still open for Day 1:** Lab 1.4 (CrashLoopBackOff outputs), the 5-command drill, and your daily hours.

---

# Process change: concept first (from 27 Oct)

The tracker is updated only when you say "save".

| Step | Min | What happens |
| --- | --- | --- |
| Evening before | 5 | Tomorrow's concept card (read only) |
| 1. Concept primer | 25 | Taught first: plain words, diagram, example. No questions |
| 2. Guided lab | 20 | Concept and commands, hand in hand |
| 3. Golang | 15 | Applied to what was just learned |
| 4. Design pattern | 15 | One pattern: problem, diagram, trade-offs |
| 5. Recall quiz | 15 | Today's concept plus yesterday's (spaced repetition) |

DSA stays separate at 30 minutes or more. You are free to spend extra time; coverage matters more than the clock.

---

# Kubernetes module plan (K1 to K16)

Sources: the 12-topic notes you shared, plus gap topics interviews ask. Each module runs as concept card, lab, Q&A ladder (L1 define, L2 how it works, L3 scenario, L4 follow-ups), then a recall test the next day. A module closes at 80% or more.

| Module | From your notes | Added beyond them | Lab |
| --- | --- | --- | --- |
| K1 Fundamentals | Notes 1 | Declarative model, reconciliation loop | Delete a pod, watch self-heal |
| K2 Architecture | Notes 2-3 | What happens on `kubectl apply`, CRI | Watch the event chain |
| K3 Pods and probes | Notes 4 | Liveness, readiness, startup, init containers, sidecars, graceful shutdown | Lab 1.4 CrashLoop, probes |
| K4 Deployments | Notes 4 | Strategies, history, `progressDeadlineSeconds`, canary | Done (Lab 1.3) plus extension |
| K5 Services and traffic | Notes 5 | DNS, EndpointSlices, headless, Gateway API | Wrong selector, DNS lookup |
| K6 Org and config | Notes 6 | ResourceQuota, LimitRange, Secret encryption, Key Vault CSI | Missing env var |
| K7 Storage | Notes 7 | Access modes, reclaim, CSI, Azure Disk vs Files | PVC stuck Pending |
| K8 Workload types | Notes 8 | StatefulSet ordering, Job parallelism, CronJob policies | One lab each |
| K9 Scheduling | Notes 9 | Topology spread, priority, preemption | Spread vs anti-affinity |
| K10 Resources and scaling | gap | Requests, limits, QoS, HPA, PDB, cordon and drain | OOMKilled, HPA |
| K11 Security | Notes 10 | `auth can-i`, Pod Security, policy engines | RBAC forbidden |
| K12 kubectl and troubleshooting | Notes 11 | The error map below | Timed break-fix |
| K13 Delivery | Notes 12 | Helm, Kustomize, GitOps, image digests | Helm chart |
| K14 Observability and ops | gap | Metrics, Prometheus, upgrades, etcd backup | Alert on Pending |
| K15 AKS specifics | gap | Node pools, CNI, workload identity, SNAT | Azure lab |
| K16 Capstone | all | 10 timed scenarios, no hints | Exam |

---

# Knowledge base

## Corrections to study sources

| Source | Says | Reality |
| --- | --- | --- |
| Notes: Ingress | NGINX as the ingress example | The ingress-nginx project was retired in March 2026. The Ingress API is frozen and Gateway API is the successor. Expect "Ingress vs Gateway API" and "how do you migrate" |
| Notes: runtime | Docker as a runtime | Kubernetes uses containerd or CRI-O. Your kind log showed `containerd.sock` |
| Notes: StorageClass | `aws-ebs` example | CSI drivers. On Azure use Azure Disk and Azure Files CSI |
| Notes: NetworkPolicy | Policy objects only | Needs a network plugin that enforces them. On AKS you choose it at cluster creation |
| TechOps OOMKilled | Lowest `oom_score` is killed first | Reversed. The highest score is killed first. Guaranteed pods score lowest, BestEffort highest |
| TechOps OOMKilled | Restart depends on the node's restart policy | It is the pod's `restartPolicy` |
| TechOps OOMKilled | One cause: limit exceeded or unavailable node resources | Two events: a container hitting its own limit is OOMKilled (exit 137); node memory pressure makes the kubelet evict pods (status Evicted) |
| TechOps ImagePullBackOff | Kubernetes stops retrying after the retry fails | The kubelet keeps retrying with growing back-off, up to about 5 minutes |
| TechOps articles | The Docker daemon pulls the image | containerd on modern clusters |
| TechOps Node Not Ready | `dockershim.sock` on v1.31 | dockershim was removed long before that version |

## Kubernetes error map

| Status | Stage | Responsible | First thing to check |
| --- | --- | --- | --- |
| Pending (NODE `<none>`) | Scheduling | scheduler | `describe pod` Events |
| ErrImagePull / ImagePullBackOff | Image pull | kubelet and registry | Wrong tag, no auth, network. On AKS: ACR attach, AcrPull role |
| CreateContainerConfigError | Before container creation | kubelet | Missing ConfigMap, Secret or key |
| CreateContainerError | Container creation | container runtime | Events: bad mount, runtime error |
| RunContainerError | Starting the process | container runtime | Command not found, permission, wrong binary |
| CrashLoopBackOff | App starts then exits | your app | `logs --previous`, exit code |
| OOMKilled (137) | Memory limit hit | kernel cgroup | Last State in `describe`; raise the limit or fix the leak |
| Evicted | Node pressure | kubelet | `describe pod`, node conditions |
| Node NotReady | Kubelet stops reporting | node, kubelet, network | `describe node` Conditions |

The last three stages for the middle rows are typical causes, not verbatim from the articles. Of the seven TechOps articles, three were read in full (ImagePullBackOff, OOMKilled, Node Not Ready); the other four are covered from general knowledge.

**Node NotReady and taints:** when a node stops reporting, Kubernetes adds `not-ready` and `unreachable` NoExecute taints. Pods carry a default toleration of about 300 seconds before they are evicted. Your cluster's `kube-node-lease` namespace holds the node heartbeat objects.

**Break-fix lab ideas:** a 500Gi request (Pending), a bad tag (ImagePullBackOff), `exit 1` (CrashLoopBackOff), a memory-hungry container (OOMKilled), a missing ConfigMap reference (CreateContainerConfigError), a nonexistent command (RunContainerError, verify the exact status), and `docker stop k8s-lab-worker2` (Node NotReady).

## DSA: the 25-problem pattern roadmap

Two Sum is done. Plan 2 to 3 problems a day, each with a table-driven Go test. Later add timed mixed sets, since 25 problems is a pattern base and not enough for every assessment.

| Pattern | Problems |
| --- | --- |
| Arrays and hashing | Two Sum, Product of Array Except Self, Longest Consecutive Sequence |
| Two pointers | 3Sum, Container With Most Water, Trapping Rain Water |
| Sliding window | Longest Substring Without Repeating Characters, Longest Repeating Character Replacement, Minimum Window Substring |
| Binary search | Search in Rotated Sorted Array, Koko Eating Bananas, Find Minimum in Rotated Sorted Array |
| Linked list | Reverse Linked List, Linked List Cycle, Merge Two Sorted Lists |
| Trees | Level Order Traversal, Diameter of Binary Tree, Lowest Common Ancestor |
| Backtracking | Subsets, Combination Sum |
| Dynamic programming | Climbing Stairs, House Robber, Coin Change |
| Graphs | Number of Islands, Course Schedule |

## System design: 12 core concepts

For each, answer four questions: what problem does it solve, why is it needed, what are the trade-offs, where is it used.

| Concept | How we study it | Link to your world |
| --- | --- | --- |
| Consistent hashing | Code a ring in Go | Distributed caches and databases |
| Rendezvous hashing | Concept card | Node selection for a key |
| Rate limiting (token bucket, leaky bucket, fixed and sliding window) | Code token bucket and sliding window in Go | Payment API protection, Redis counters |
| LRU cache | Code in Go (map plus linked list) | Caching, Redis eviction |
| Bloom filter | Code in Go | Skip needless database lookups |
| Count-Min Sketch | Code in Go | High-volume analytics |
| Leader election | Concept card | Kubernetes scheduler and controller manager use Lease objects |
| Consensus (Raft, Paxos) | Concept card | etcd uses Raft |
| Gossip protocol | Concept card | Cluster membership, service discovery |
| Merkle tree | Concept card | Replication and storage comparison |
| MapReduce | Concept card | Parallel batch processing |
| Top-K / heavy hitters | Code with a heap | Trending, analytics |

Roadmap order: algorithms, data structures, caching, load balancing, databases, distributed systems, queues and streaming, scalability, fault tolerance, complete designs.

## Google-style question bank (attempt first, then review)

Paraphrased prompts. Treat the "Google pays 65-80 LPA" headline as marketing. The questions are good practice either way.

1. A Postgres query on a huge table goes from 80 ms to 14 s after a routine deploy. What do you check first?
2. Redis hit rate drops from 92% to 31% in a flash sale and the database gets 60k QPS. What happened?
3. A payment succeeds, then the inventory service times out and you do not know if it committed. How do you settle it without 2PC? (Your home ground: saga, outbox, reconciliation.)
4. At 80k events a second you see duplicate event IDs and the team blames UUID v4 collisions. Are they right?
5. One new index makes writes drop 40% for 6 minutes with no errors in the logs. What happened?
6. A gRPC service has p50 42 ms and p99 1.8 s with healthy CPU and memory. What do you test first?
7. Keep shortest paths fresh when traffic changes every 30 seconds across 50 million road segments.
8. Count billions of video plays a day while showing an approximate count.
9. Search autocomplete under 100 ms worldwide, and a query trending in one region.
10. Two users edit the same file offline and reconnect together. Merge without losing data.
11. Fast search over a very large user base where no two users share an index.
12. A video call user's bandwidth drops. Lower quality without dropping the call.

## System design pattern per day (30-day plan)

Each day gets a pattern card: problem, diagram, trade-offs, where it appears in payments and Azure, and one interview line. Drill days combine the week's patterns.

| Day | Date | Pattern |
| --- | --- | --- |
| 1 | Mon 26 Oct | Idempotency key |
| 2 | Tue 27 Oct | Retry with backoff and jitter, health endpoint monitoring |
| 3 | Wed 28 Oct | API gateway: routing and offloading |
| 4 | Thu 29 Oct | Rate limiting and bulkhead |
| 5 | Fri 30 Oct | Valet key, federated identity |
| 6 | Sat 31 Oct | Circuit breaker and timeouts |
| 7 | Sun 1 Nov | Drill: resilient call path (timeout, retry, breaker, bulkhead) |
| 8 | Mon 2 Nov | Database per service, read replicas |
| 9 | Tue 3 Nov | Leader election, sharding |
| 10 | Wed 4 Nov | Cache-aside and single-flight |
| 11 | Thu 5 Nov | Gatekeeper, token validation |
| 12 | Fri 6 Nov | Sidecar and ambassador |
| 13 | Sat 7 Nov | Correlation IDs, distributed tracing |
| 14 | Sun 8 Nov | Drill: design a read-heavy balance service |
| 15 | Mon 9 Nov | Payment state machine, double-entry ledger |
| 16 | Tue 10 Nov | Transactional outbox, saga |
| 17 | Wed 11 Nov | Deployment stamps, active-active vs active-passive |
| 18 | Thu 12 Nov | Strangler fig, canary and blue-green |
| 19 | Fri 13 Nov | External configuration store, IaC drift |
| 20 | Sat 14 Nov | Zero trust, encryption, audit trail |
| 21 | Sun 15 Nov | Drill: full payment gateway with all patterns |
| 22 | Mon 16 Nov | CQRS and event sourcing (when not to use) |
| 23 | Tue 17 Nov | Queue-based load leveling, competing consumers |
| 24 | Wed 18 Nov | Priority queue, pipes and filters |
| 25 | Thu 19 Nov | Mock: notification system |
| 26 | Fri 20 Nov | Mock: rate limiter service |
| 27 | Sat 21 Nov | Mock: merchant payout system |
| 28 | Sun 22 Nov | Gap fixing |
| 29 | Mon 23 Nov | Final design test |
| 30 | Tue 24 Nov | Light revision |

**Key payment-design rules to remember:** write the idempotency key in the same transaction as the effect; write outbox events in the same transaction as the state change and publish after commit; a saga gives eventual consistency, not ACID, and no isolation, so say that trade-off aloud; keep the synchronous payment path separate from asynchronous settlement and reconciliation; wrap payment-provider calls in retries with backoff and jitter plus a circuit breaker.


---

# Day 2: Pod Lifecycle, CrashLoopBackOff, OOMKilled & Probes (27 Oct 2026)

## 1. Concept Primer & Error Map

### Pod Lifecycle & Failure Classification

```
[ 1. Scheduling ] -------> Failure: Pending (NODE: <none>, insufficient CPU/mem, taints)
       ???
       ???
[ 2. Image Pull ] -------> Failure: ErrImagePull / ImagePullBackOff (bad tag, no ACR auth)
       ???
       ???
[ 3. Pre-Start Config ] -> Failure: CreateContainerConfigError (missing Secret or ConfigMap)
       ???
       ???
[ 4. Process Start ] ----> Failure: RunContainerError (binary not found, permissions)
       ???
       ???
[ 5. App Running ] ------> Crashes immediately? --> CrashLoopBackOff (App error, exit 1)
       ???               ???
       ???               ????????? Exceeds memory limit? -> OOMKilled (Exit 137, kernel cgroup kill)
       ???               ???
       ???               ????????? Node memory exhausted? -> Evicted (kubelet eviction under pressure)
       ???               ???
       ???               ????????? Liveness probe fails? -> Kubelet restarts container
       ???               ???
       ???               ????????? Readiness probe fails? -> Pod IP pulled from Service endpoints
       ???
[ 6. Termination ] ------> SIGTERM sent -> App drains requests -> Exit 0 / SIGKILL (exit 137/143)
```

### CrashLoopBackOff vs OOMKilled vs Evicted

| Feature | `CrashLoopBackOff` | `OOMKilled` (Exit 137) | `Evicted` |
| :--- | :--- | :--- | :--- |
| **Who terminates it?** | The application itself (process exits) | Linux kernel cgroup OOM killer | The Node Kubelet |
| **Root Cause** | Uncaught panic, missing config, failed DB boot | Container memory hit `limits.memory` | Node disk/memory pressure reached eviction threshold |
| **Exit Code** | Usually `1` (or custom app code) | `137` (128 + 9 `SIGKILL`) | None (pod phase set to Failed) |
| **Where to verify** | `kubectl logs <pod> --previous` | `kubectl describe pod` (Last State: Terminated / Reason: OOMKilled) | `kubectl describe pod` (Status: Failed, Reason: Evicted) |
| **Resolution** | Fix code panic, boot configs, connection strings | Raise memory limit or profile Go memory leak (`pprof`) | Rebalance node workloads, scale cluster/node pool |

### Probe Architecture: Startup vs Liveness vs Readiness

1. **Startup Probe**:
   * Protects slow-starting applications (cache warm-up, data migrations).
   * Disables Liveness and Readiness until it succeeds once.
2. **Liveness Probe (`/healthz/live` - Shallow)**:
   * **Rule:** Internal process health only. Detects deadlocks and hung event loops.
   * **Golden Rule:** **NEVER check external dependencies (DB, Redis, downstream APIs) in a Liveness Probe.** If a DB slows down, restarting pods triggers a catastrophic thundering herd.
3. **Readiness Probe (`/healthz/ready` - Deep)**:
   * **Rule:** Determines if the pod is eligible to receive traffic.
   * **Action on failure:** Does **not** restart the container. It pulls the pod's IP out of the `EndpointSlice` / Service routing, sparing the database while keeping the pod alive.

---

## 2. Production Incident Drill: Banking Payment Authorizer

### Scenario
* **Service:** `payment-authorizer` (Golang microservice on AKS)
* **Impact:** 18% spike in HTTP 502 / 504 Gateway errors during peak morning transactions (08:45 AM).
* **Cluster State:**
  * Some pods in `CrashLoopBackOff` (`0/1`, 9-12 restarts).
  * Some pods in `OOMKilled` (`0/1`, Exit Code 137).
  * Few remaining pods in `Running` (`1/1`) with CPU/memory pinned near limits.
  * Release `v2.4.1` deployed overnight at 02:00 AM.

---

### Candidate Response & Evaluation

#### Evaluation: Initial Troubleshooting
* **Score:** **52 / 100** (Developing / Borderline for 15+ years Lead)
* **What was strong:**
  * Immediate instinct to restore stability via `kubectl rollout undo` before deep debugging.
  * Correct identification that `CrashLoopBackOff` in Go often points to code panics.
* **Critical Interview Gaps & Red Flags Identified:**
  1. **CPU does NOT cause OOMKilled:** OOMKilled is strictly enforced by the Linux kernel memory cgroup when `limits.memory` is exceeded. High CPU causes **CFS throttling** or latency spikes, never an OOM kill.
  2. **Command precision:** `kubectl logs <pod-name> -n payments --previous` is required for crashed containers. Events and exit codes require `kubectl describe pod`.
  3. **Understanding the Cascade Mechanism:** Pods getting `OOMKilled` dump all traffic onto survivors. The survivors thrash and starve resources. When dead pods reboot, they enter `CrashLoopBackOff` either because the DB connection pool is exhausted or because an aggressive probe kills them during boot.
  4. **Senior Risk Awareness:** Before rolling back (`rollout undo`), verify whether the release ran a non-backward-compatible database migration.

---

### Model Answer (Interview-Ready Script)

> **1. Immediate Mitigation:**  
> "First, verify if the 02:00 AM release ran any non-backward-compatible DB migrations. If safe, execute `kubectl rollout undo deployment/payment-authorizer -n payments` to revert to the known stable image. Simultaneously, temporarily scale up deployment replicas to absorb the morning peak traffic and prevent surviving pods from collapsing."
>
> **2. Investigation & Signal Gathering:**  
> * For OOMKilled pods: `kubectl describe pod <pod> -n payments` -> inspect `Last State: Terminated`, `Exit Code: 137`, and compare memory limits against Azure Monitor metrics.  
> * For CrashLoopBackOff pods: `kubectl logs <pod> -n payments --previous` -> check for Go stack trace panics or boot handshake timeouts.  
> * For Cluster Events: `kubectl get events -n payments --sort-by='.metadata.creationTimestamp'` -> check for failing probes or node pressure.
>
> **3. Root Cause Hypothesis:**  
> "Morning volume exposed an unbuffered payload or memory leak in `v2.4.1`, tripping the kernel cgroup memory limit (`Exit Code 137`). Surviving pods took excess traffic, causing widespread connection saturation. Restarting pods failed health checks under load, entering `CrashLoopBackOff`."

---

## 3. Follow-Up Twist: Health Check Probe Architecture

### Twist
`kubectl describe pod` on the crashing pod reveals:
```text
Events:
  Warning  Unhealthy  1m  kubelet  Liveness probe failed: HTTP probe failed with statuscode: 503
  Normal   Killing    1m  kubelet  Container app failed liveness probe, will be restarted
```
Logs: `{"level":"error","msg":"Health check failed: PostgreSQL query timed out after 2s"}`

### Candidate Response & Evaluation
* **Score:** **78 / 100** (Solid Hire)
* **Strong Answer:** Identified that liveness probes must never check database health, as restarting pods multiplies database connection pool pressure and locks the service in a restart loop.

### Architectural Solution
* **Liveness Probe (`/healthz/live`):** Shallow in-process check. Validates the Go HTTP runtime is responsive and not deadlocked. Fast execution (< 50ms). Never calls DB or Redis.
* **Readiness Probe (`/healthz/ready`):** Deep component check. Pings the DB connection pool and cache state. If DB times out, pod fails readiness: Kubelet pulls the pod IP from `EndpointSlice` to prevent sending user traffic to an unready pod, **without restarting the container**.
* **Next Topic Link:** Day 3 (Module K5) covers the internal routing side of this failure: how `EndpointSlice` controller updates endpoints and how `kube-proxy` programs `iptables/IPVS`.

---

## Scoreboard Update

| Area | Baseline | Previous | Today's Drill | Current Status |
| :--- | :--- | :--- | :--- | :--- |
| **AKS / Kubernetes** | 10 | - | **65 / 100** | ?????? (+55 from baseline, Band: Developing) |

---

## 4. Lab 1.4 Hands-On Results: Pod Lifecycle & Probes (Verified on v1.35 Cluster)

**Source:** Live Kind cluster (`k8s-lab-control-plane`, `k8s-lab-worker`, `k8s-lab-worker2`), namespace `practice`.

### Verified Test Results

| Experiment | Manifest | Live Verified Telemetry | Outcome |
| :--- | :--- | :--- | :--- |
| **01. App Boot Failure** | `01-crashloop-exit1.yaml` | `Last State: Error`, `Exit Code: 1`, logs retrieved via `--previous` | **Pass** (`CrashLoopBackOff`) |
| **02. Memory Ceiling Kill**| `02-oomkilled-137-fix.yaml` | `Last State: Terminated`, `Reason: OOMKilled`, `Exit Code: 137` | **Pass** (Proved $128 + 9 = 137$) |
| **03. Liveness Failure** | `03-broken-liveness.yaml` | `RESTARTS: 4`, Kubelet killing container every cycle | **Pass** (Escalated to CrashLoop) |
| **04. Readiness Failure** | `04-broken-readiness.yaml`| `RESTARTS: 0`, `0/1 Running`, `EndpointSlice Ready: false`, `Endpoints: <none>` | **Pass** (Traffic cut, no restart) |

### Key Candidate Discoveries During Lab
1. **`RunContainerError` vs `OOMKilled`:** Discovered that memory limits `<64Mi` kill the container during `runc` runtime init (`RunContainerError`), whereas `OOMKilled` (137) occurs when a running process crosses the limit.
2. **The `/dev/shm` tmpfs Trap:** Discovered that default container `/dev/shm` is hard-capped at 64MB (`No space left on device`), proving real heap leaks require process heap allocation.
3. **Traffic Isolation Evaluation:**
   * *Question:* Does client traffic hit a `0/1 Running` pod?  
     *Answer:* **100/100** ("No, because readiness failed, so traffic routes to old containers to maintain service stability").
   * *Question:* What command proves routing state?  
     *Answer:* Learned that logs cannot prove routing; must inspect `kubectl describe endpointslice` (`Conditions: Ready: false`) and Service (`Endpoints: <none>`).
   * *v1.35 Discovery:* Witnessed live deprecation warning for `v1 Endpoints` and verified `discovery.k8s.io/v1 EndpointSlice`.

* **Lab Hands-on Score:** **88 / 100** (Strong Hire execution, verified on live cluster)

---

## Updated Master Scoreboard (Day 2 Summary)

| Area | Baseline | Incident Drill | Architecture | Hands-on Lab | Running Score | Band |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **AKS / Kubernetes** | 10 | 52 | 78 | 88 | **73 / 100** | ???? **Interview Ready** (71-85) |
