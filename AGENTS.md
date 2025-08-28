# Agent Guidelines for Homelab

- Build/lint/test
  - kubectl: kubectl apply --dry-run=client -f <file>
  - Kustomize (single overlay/dir): kustomize build <dir> | kubectl apply --dry-run=client -f -
  - Helm template (with values): helm template <release> <chart> --values <values.yaml> | kubectl apply --dry-run=client -f -
  - YAML lint: yamllint <file> or yamllint <dir>
  - Single test focus: prefer testing the smallest unit (a single manifest or one kustomization directory) before composing larger stacks.

- Repo layout hints: apps/* for app bases/overlays, clusters/* for Flux, drivers/* and monitoring/* for infra controllers; use kustomize build at those directory roots to validate.

- YAML/style
  - 2-space indent; keep keys lowercase; always include apiVersion and kind first; add comments for non-obvious fields.
  - Names: namespaces and resources lowercase, descriptive (e.g., memos-namespace, memos-deployment, memos-service). Labels: app: <appname> for selectors.
  - Prefer explicit values over implicit defaults; sort top-level keys logically: apiVersion, kind, metadata, spec, status.

- Kubernetes best practices
  - Always set resources (requests/limits); replicas 1–2 for homelab; use Traefik ingressClass with TLS; SOPS-encrypt Secrets (matches ^(data|stringData)$) under clusters/* .sops.yaml rules.
  - Helm Releases: pin chart versions, set install.crds: Create and upgrade.crds: CreateReplace, specify interval (e.g., 30m).

- Error handling and validation
  - Validate before commit; use kubectl explain <Kind>.<field> for schema help; test changes first in smallest scope, then layer upward.

- Security and secrets
  - No hardcoded creds; use age + sops for secret material; follow RBAC least-privilege.

- Editors/assistants
  - No Cursor/Copilot rules found in repo; follow this file. Keep PRs small and focused; include commands you ran to validate in the PR/body.