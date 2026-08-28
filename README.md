GitHub Starter Workflows are completely free and included by default with all GitHub accounts. However, running these workflows consumes continuous integration (CI) compute time and artifact storage.
Understanding how GitHub Actions pricing, free tiers, and starter workflow usages operate ensures cost-effective workflow management:
1. Free Quotas for GitHub Actions
GitHub provides free monthly compute minutes and storage for running Action workflows (including starter workflows) based on your account tier:
| Account Plan | Free Actions Minutes / Month | Free Artifact Storage | Actions Cache Limit (per repo) |
|---|---|---|---|
| Public Repositories | Unlimited Free | 500 MB | 10 GB |
| GitHub Free (User/Org) | 2,000 minutes | 500 MB | 10 GB |
| GitHub Pro | 3,000 minutes | 1 GB | 10 GB |
| GitHub Team | 3,000 minutes | 2 GB | 10 GB |
| GitHub Enterprise | 50,000 minutes | 50 GB | 10 GB |
Note: All GitHub Actions run on public repositories are 100% free.
2. Overage Rates (When Exceeding Included Quotas)
If you exceed your monthly included minutes on private repositories, standard per-minute compute billing applies:
 * Linux (2-core standard runner): $0.006 / minute
 * Linux (1-core slim runner): $0.002 / minute
 * Windows (2-core runner): $0.010 / minute
 * macOS (3/4-core runner): $0.062 / minute
 * Additional Storage: $0.25 per GB / month for build artifacts; $0.07 per GB / month for cache storage above limit.
3. Best Practices to Optimize Starter Workflows & Reduce Minute Usage
 * Use Path Filtering (paths-ignore):
   Configure workflow triggers so builds don't execute on non-code changes (like .md documentation updates):
   on:
  push:
    branches: [main]
    paths-ignore:
      - '**.md'
      - 'docs/**'

 * Cache Dependencies (actions/setup-node):
   Avoid re-downloading packages during every run by leveraging package manager caching:
   - uses: actions/setup-node@v4
  with:
    node-version: 20
    cache: 'npm'

 * Set Execution Timeouts (timeout-minutes):
   Prevent hung processes from consuming your entire minute allowance by capping step durations:
   jobs:
  build:
    runs-on: ubuntu-latest
    timeout-minutes: 15

 * Concurrently Cancel Outdated PR Runs:
   Use concurrency groups to automatically terminate previous workflow runs when new commits are pushed to the same pull request:
   concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

