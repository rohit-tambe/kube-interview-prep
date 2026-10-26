# Lab 1.3: Rolling Updates
**Cluster:** `kind-k8s-lab` | **Namespace:** `practice`

## Files to create (you write these yourself during the lab):

| File | What it does |
|---|---|
| `01-good-deployment.yaml` | Working deployment — baseline |
| `02-broken-image.yaml` | Same deployment with a bad image tag — triggers rollout failure |
| `03-broken-readiness.yaml` | Deployment with a misconfigured readiness probe |
| `04-fixed-readiness.yaml` | Fixed readiness probe — rollout succeeds |

## Key commands you'll use:
- `kubectl rollout status`
- `kubectl rollout history`
- `kubectl rollout undo`
- `kubectl set image`
