# md-helm-values

Environment-specific Helm overrides for **Marketing Digest**, one folder
per service and environment.

## Layout

```text
<service>/
  local/values.yaml
  prod/values.yaml
```

Services: `gateway`, `blogs`, `auth`.  
Environments now: `local`, `prod`. (`uat/` later — not created yet.)

## Purpose

| Repository | Owns |
|---|---|
| [md-charts](https://github.com/Yuvraj02/md-charts.git) | Chart templates + safe defaults |
| [md-helm-values](https://github.com/Yuvraj02/md-helm-values.git) | Per-env overrides (this repo) |
| [md-infra](https://github.com/Yuvraj02/md-infra.git) | Argo CD ApplicationSets |

Files here must match the schema of the corresponding chart under
`md-charts/<service>`. Only override what differs from chart defaults.

## Local

Kind cluster (`kind-kind`). Workloads are intended for namespace
`marketing-digest` via Argo CD.

## Production

`prod/` directories exist for structure. Do not invent AWS/ECR/domain/DB
endpoints here until infrastructure is ready.

## Secrets

Secrets are **never** stored in this repository. Reference Kubernetes
Secrets by name only after creating them manually in the cluster.

## GitOps flow

```text
md-infra
    |
    v
Argo CD ApplicationSet
    |
    +------------------+
    |                  |
    v                  v
md-charts        md-helm-values
    |                  |
    +--------+---------+
             |
             v
           Helm
             |
             v
         Kubernetes
```
