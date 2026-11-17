# launchbay-gitops

The GitOps source of truth for services launched with [Launchbay](https://github.com/paighanutkarsh2410-lab/launchbay).

What's on `main` here is what runs. Launchbay launches a service by committing one file to
`services/`; an Argo CD ApplicationSet turns each file into an Application deployed with the
shared chart in `charts/service`. Removing the file removes the service.

```yaml
# services/payments-api.yaml
name: payments-api
owner: team-payments
template: python-fastapi
image: ghcr.io/launchbay-labs/payments-api@sha256:…
port: 8000
host: payments-api.localtest.me
```

Every service gets the same hardened defaults from the chart: two replicas, a PodDisruptionBudget,
readiness and liveness probes on `/healthz`, a non-root `RuntimeDefault` seccomp profile, no
privilege escalation, all capabilities dropped, resource requests and limits, and an Ingress.
