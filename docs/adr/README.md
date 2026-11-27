# Architecture Decision Records

An ADR records one decision: the problem, what was chosen, what it costs and what was rejected.

| ADR | Decision |
|-----|----------|
| [0001](0001-shared-setup-and-checks.md) | Share the toolchain setup and a checks workflow; keep deploys in each repo |
| [0002](0002-public-repo-without-secrets.md) | Keep this repo public and free of secrets |

## When to write one

Write one when the decision is **hard to reverse** and **not derivable from the code**: a convention others
must follow, a choice between viable approaches, or a tradeoff someone will later mistake for a bug.
Clayton's decisions on how this repo works always get one, however small. Otherwise:

| Instead of an ADR | Use |
|-------------------|-----|
| One input's gotcha | its `description` |
| How to use or release something | [`README.md`](../../README.md) |
| A plan or work breakdown | a GitHub issue |

## Writing one

1. Copy [`template.md`](template.md) to `NNNN-kebab-case-title.md`, taking the next free number. Numbers are
   never reused. If another PR took yours first, renumber before merging.
2. Ground it in the repo: real files, names and measurements. One decision per record.
3. Set the status, and flip `Proposed` to `Accepted` when Clayton agrees:

   | Status | Meaning |
   |--------|---------|
   | `Proposed — YYYY-MM-DD` | Written, not yet agreed |
   | `Accepted — YYYY-MM-DD` | Agreed and in force |
   | `Superseded by NNNN — YYYY-MM-DD` | Replaced; the record stays |

4. Add it to the table above.

Don't edit an accepted record's Decision: write a new ADR and mark the old one superseded. Typos, dead links
and a stale status are free to fix.
