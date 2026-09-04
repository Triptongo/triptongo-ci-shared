# triptongo-ci-shared

Reusable GitHub Actions workflows for Triptongo Track Apps — deploy, D1 migration apply +
verification, and health-check smoke testing.

**Public on purpose.** GitHub reusable workflows (`uses: owner/repo/.github/workflows/x.yml@ref`)
can only be called from repos owned by the *same* org/user account when the workflow repo is
private, with no way to allowlist specific external owners. Making this one repo public lets
Track Apps outside the Triptongo org (personal projects, client repos under a different account)
still call the same workflow instead of vendoring a copy.

Nothing sensitive lives here. Every credential and environment-specific value (D1 database name,
health check URL, Cloudflare account) comes in via `secrets:`/`inputs:` from the calling repo —
this repo only holds the generic mechanics.

The methodology this workflow is part of (CLAUDE.md templates, ADRs, agent profiles, Track
scaffolds) stays private in `triptongo-dev-playbook`. This repo is deliberately narrow: only the
CI/CD glue that needs cross-owner reach.

## Usage

```yaml
jobs:
  deploy-staging:
    uses: Triptongo/triptongo-ci-shared/.github/workflows/post-merge-deploy.yml@main
    with:
      environment: staging
      ref: ${{ github.event.workflow_run.head_sha }}
      deploy_api: true
      deploy_dashboard: true
      d1_database_name: "your-db-staging"
      api_health_url: "https://your-api-staging.example.workers.dev/status"
    secrets: inherit
```

See `.github/workflows/post-merge-deploy.yml` for the full input list.
