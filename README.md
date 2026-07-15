# .github

Org-wide reusable GitHub Actions workflows for drasil-ai.

## Reusable workflows

### `pick-runner`
Selects a self-hosted runner when one is online, else falls back to a GitHub-hosted runner.

```yaml
jobs:
  pick-runner:
    uses: drasil-ai/.github/.github/workflows/pick-runner.yml@v1
    secrets: inherit            # needs org secret RUNNER_CHECK_TOKEN

  build:
    needs: pick-runner
    runs-on: ${{ fromJSON(needs.pick-runner.outputs.labels) }}
    steps:
      - uses: actions/checkout@v4
```

Requires an org secret `RUNNER_CHECK_TOKEN` — a fine-grained PAT with **Administration: Read** on the repos being checked. Pin callers to a tag (`@v1`), not `@main`.
