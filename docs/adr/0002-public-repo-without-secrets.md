# ADR 0002: Keep this repo public and free of secrets

**Status:** Accepted — 2026-11-27
**Deciders:** Clayton
**Related:** [0001](0001-shared-setup-and-checks.md)

## Context

BenThereMedia/shnipper is one of the repos that must use this one, and it lives under a different owner.
GitHub lets a private repo share actions and reusable workflows only with repos of the same owner, so a
private claytonjroberts/ci would be out of shnipper's reach.

## Decision

The shared CI repo is public so repos under other owners can use it, and it holds no secrets. Clayton chose
"Public claytonjroberts/ci" because BenThereMedia/shnipper has to be able to read it.

Every value a caller's run needs comes in as an input or as a secret the caller passes. Nothing here names
a project, cloud account, bucket or identity provider.

## Consequences

**Positive:** any repo of any owner can call it, with no per-repo Actions access settings.

**Negative / risks:** anything committed here is world-readable, including history. A reviewer must refuse
a token, a project id or an internal hostname in this repo, even as a default.

## Alternatives considered

- **Private, with shnipper keeping its own copy** — rejected: one of the repos Clayton named would stay
  unstandardized.
- **Private, in the BenThereMedia org** — rejected: the claytonjroberts repos couldn't read it.
