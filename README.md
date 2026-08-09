# md-helm-values

Environment-specific Helm value overrides for **Marketing Digest**.

## Purpose

This repository holds **per-environment** Helm values only. It does not
contain Helm chart templates, application source, Argo CD manifests, or
secrets.

## Why values are separate from charts

| Repository | Owns |
|---|---|
| [md-charts](https://github.com/Yuvraj02/md-charts.git) | Chart templates and safe defaults |
| [md-helm-values](https://github.com/Yuvraj02/md-helm-values.git) | Environment overrides (this repo) |
| [md-infra](https://github.com/Yuvraj02/md-infra.git) | Argo CD bootstrap / ApplicationSets / infra |

Keeping overrides here lets the same charts deploy to local, staging, and
production without forking templates.

## Local environment

```text
local/
  gateway.yaml    # overrides for md-charts/gateway (Kind: kind-kind)
  services.yaml   # overrides for md-charts/services (Kind: kind-kind)
```

Staging and production are not defined yet.

Local files are intentionally minimal: chart defaults already match Kind
for `ClusterIP`, small resources, gateway on **8080**, and auth/blogs
each on **50051** (distinct Services). Image tags, in-cluster env,
probes, and Secret references must be supplied when those details are
known — they are not invented here.

## Secrets

Secrets are **never** stored in this repository.

Do not put passwords, tokens, `OWNER_STUDIO_SECRET`, or private keys in
these YAML files. Reference existing Kubernetes Secrets by name only
(via chart-supported `env` / `envFrom` shapes) after creating Secrets
manually in the cluster.

## Eventual GitOps flow

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
