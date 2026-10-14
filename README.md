# .github

Public org profile plus reusable GitHub Actions workflows.

- **Org Overview (public):** [`profile/README.md`](profile/README.md)
- **Reusable workflows:** [`.github/workflows/`](.github/workflows/)

This repository is public so GitHub can render the Overview README. Workflow files here contain no secrets.

## Reusable workflows

### `pick-runner`

Selects a self-hosted runner when one is online, else falls back to a GitHub-hosted runner.

```yaml
jobs:
  pick-runner:
    uses: yggy-tech/.github/.github/workflows/pick-runner.yml@v1
    with:
      scope: org
    secrets:
      app_id: ${{ secrets.APP_ID }}
      app_private_key: ${{ secrets.APP_PRIVATE_KEY }}

  build:
    needs: pick-runner
    runs-on: ${{ fromJSON(needs.pick-runner.outputs.labels) }}
```

Pin callers to a tag (`@v1`), not `@main`.

The App needs org **Self-hosted runners: Read**. A PAT mapped to `runner_check_token` works as a fallback. Callers in this org pass the App secrets they already use for gitops.
