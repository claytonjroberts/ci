# ci

Shared GitHub Actions CI for claytonjroberts repos. Each repo keeps its checks in its own `just` (or `make`)
recipes, so a local run and CI can't drift; this repo owns how CI gets the toolchain and runs them.

| Path | What it is |
|------|------------|
| [`setup`](setup/action.yml) | Composite action: uv, a pinned just, optional Node + pnpm, then `uv sync --locked` |
| [`.github/workflows/checks.yml`](.github/workflows/checks.yml) | Reusable workflow: one job per command, plus actionlint |

## Use

```yaml
jobs:
  checks:
    uses: claytonjroberts/ci/.github/workflows/checks.yml@v1
    with:
      commands: '["just lint", "just type-check", "just test"]'
      # node-version: "22"           # also installs pnpm, pinned by package.json's packageManager
      # pnpm-dir: web                # where that package.json and pnpm-lock.yaml live
      # runs-on: '["self-hosted", "linux"]'
      # sync: uv sync --locked --all-groups
```

A job that needs services, artifacts or secrets stays in the repo and uses the action directly:

```yaml
    steps:
      - uses: actions/checkout@v6
      - uses: claytonjroberts/ci/setup@v1
      - run: just test
```

## Releasing

Callers pin the `v1` tag. After a change merges, move it: `git tag -f v1 origin/main && git push -f origin v1`.
A breaking input change gets `v2`.

This repo is public and holds no secrets ([ADR 0002](docs/adr/0002-public-repo-without-secrets.md)).
