# cognidispatch-reusable-workflows

Shared reusable GitHub Actions workflows for the **CogniDispatch** organization.

## Workflows

| Workflow | Trigger | Purpose |
|---|---|---|
| `_build_push.yml` | `workflow_call` | Build Docker image, scan with Trivy/Snyk/SonarCloud, push to ACR |
| `_update_helm.yml` | `workflow_call` | Update `imageTag` in the Helm chart repo for GitOps |

## Usage

Each microservice repo calls these workflows from its own `ci.yml`:

```yaml
jobs:
  build-and-push:
    uses: cognidispatch/cognidispatch-reusable-workflows/.github/workflows/_build_push.yml@main
    with:
      image_name: cogni-auth-service
      tag: ${{ needs.prepare.outputs.tag }}
      is_production: ${{ needs.prepare.outputs.is_production }}
    secrets:
      ACR_USERNAME: ${{ secrets.ACR_USERNAME }}
      ACR_PASSWORD: ${{ secrets.ACR_PASSWORD }}
```

## Branch Strategy

| Branch | Environment | Image Tag Format |
|---|---|---|
| `production` | Production | `v0.0.1`, `v0.0.2` … (semver auto-increment) |
| `dev` | Development | `dev-<7-char-sha>` |
