# Infra-Portal-GitOps

GitOps repository for the [Infra-Portal](https://github.com/pavan-nagula/Infra-Portal) application.
ArgoCD watches this repo and syncs the cluster to match what's here.

## Layout

```
charts/infra-portal/                   # Helm chart (templates + chart defaults values.yaml)
environments/<env>/infra-portal/       # Per-environment value overrides (image.tag lives here)
apps/infra-portal/application.yaml     # ArgoCD Application (one per env)
projects/infra-portal.yaml             # ArgoCD AppProject
```

## How a deploy happens

1. Code change merges to `main` in **Infra-Portal** (app repo).
2. CI builds and pushes images tagged `sha-<short>` to GHCR.
3. Human opens a PR in **this repo** bumping `image.tag` in `environments/<env>/infra-portal/values.yaml`.
4. PR merged → ArgoCD detects drift → syncs → new pods roll out.
