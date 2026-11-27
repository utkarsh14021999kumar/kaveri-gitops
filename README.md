# kaveri-gitops

Desired state for KAVERI environments (ADR-0016 in kaveri-platform). Argo CD in each cluster pulls from `main`.

## Layout
- `clusters/dev/` - Argo CD Application manifests (children of the root app `kaveri-dev-root`)
- `platform/dev/` - namespace and Workload Identity service account for DEV
- `apps/boutique/base/` - Online Boutique manifests vendored from kaveri-boutique (see `SOURCE.md`)
- `apps/boutique/components/` - kustomize components used by overlays
- `apps/boutique/overlays/dev/` - DEV: namespace `kaveri-dev`, images by digest, no public load balancer

## Rules
- `main` is PR-only (squash); required check: `validate`.
- Overlays reference images by digest only; digests come from kaveri-boutique CI (`release-digests.json`).
- No secrets in this repository (Gitleaks); secret values are supplied out-of-band (ADR-0011).
- Rollback = `git revert` of the promotion PR; Argo CD converges automatically.
