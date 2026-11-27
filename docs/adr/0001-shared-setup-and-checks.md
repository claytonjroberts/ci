# ADR 0001: Share the toolchain setup and a checks workflow; keep deploys in each repo

**Status:** Proposed — 2026-11-27
**Deciders:** Clayton
**Related:** `setup/action.yml`, `.github/workflows/checks.yml`

## Context

Clayton asked to standardize his CI into GitHub Actions in a single repo that all his repos use, and noted
that claytonjroberts.com "needs CI".

Before this repo, three repos each carried their own `setup-just` action, and two had their own `setup`
action. They pinned different versions of setup-uv, setup-node,
pnpm/action-setup and checkout. Every repo already runs its checks as `just <recipe>` (claylib uses
`make`), so what they share is the toolchain plus "run these recipes". Their deploy jobs (Docker, Pulumi,
migrations, desktop and iOS releases) have nothing in common beyond that.

## Decision

1. **One `setup` action** installs uv (cached), a pinned just, and optionally Node with pnpm, then syncs the
   project. Every repo uses it in place of its own setup actions. just is fetched by URL, not through
   extractions/setup-just, which resolves versions through a releases list GitHub has served empty.
2. **One reusable `checks.yml`** runs a list of commands, one job each, plus actionlint. A repo whose checks
   need nothing but the toolchain calls it. A job that needs services, artifacts, secrets or a runner chosen
   at run time stays in its repo and uses `setup` directly.
3. **Deploys stay in each repo.** They differ per project, and sharing them would add inputs without
   removing code.
4. **Callers pin `@v1`**, a tag moved forward on each release; a breaking change gets `v2`.

## Consequences

**Positive:** a toolchain or action-version bump happens once. A new repo gets CI from one short job.

**Negative / risks:** moving `v1` changes every caller's next run at once, so a bad release breaks them
all. Check a change against a caller's PR, pinned to the branch, before moving the tag. Job names change
to `checks / <command>`, so a branch rule must require the caller's own gate job, not a matrix leg.

## Alternatives considered

- **One reusable workflow that covers everything, deploys included** — rejected: shnipper's and mechometer's
  graphs (runner pickers, artifacts between jobs, service containers, per-job concurrency) would turn into
  inputs that only one repo sets.
- **Only the setup action, no checks workflow** — rejected: the four smaller repos would each still repeat
  the checkout, setup and matrix boilerplate.
