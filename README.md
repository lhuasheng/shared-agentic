# shared-agentic

Markdown-based [GitHub Agentic Workflows](https://github.github.com/gh-aw/)
that implement the reasoning layer of the **AI-Enhanced Software Development
Lifecycle (AI-SDLC) Framework**. These workflows are informational and
advisory — they do not block merge. Deterministic, merge-blocking gates live
in [`lhuasheng/shared-sdlc`](https://github.com/lhuasheng/shared-sdlc).

This repo is the **canonical source** for the agentic workflows. Project
repos do not dispatch into it at runtime — instead,
[`shared-sdlc/new-project.sh`](https://github.com/lhuasheng/shared-sdlc/blob/main/new-project.sh)
**vendors** each workflow's `.md` source *and* its compiled `.lock.yml` into
the project's own `.github/workflows/`, so agentic runs and their
safe-outputs happen inside the analyzed repo using the repo-scoped
`GITHUB_TOKEN`. Event-driven workflows (issue triage, weekly digest) trigger
directly off the project's own events; on-demand ones (PR review, release
notes) are dispatched *locally* by `shared-sdlc`'s composite actions,
targeting the compiled `*.lock.yml` filename — GitHub Actions only registers
`.yml` files, so a `.md` can never be dispatched.

> **This repo must be public** (or accessible via a GitHub App) so that
> `new-project.sh` can fetch the workflow sources and lock files when
> bootstrapping a project.

---

## What's in here

```
.github/workflows/          ← .md sources + compiled .lock.yml files
  weekly-digest.md            ← Monday 09:00 UTC engineering digest issue (cron in the lock)
  issue-triage.md             ← Auto-classify new issues (triggers on issues: opened)
  pr-review.md                ← AI pre-screen review (dispatched by /ai-review via shared-sdlc)
  release-notes.md            ← Draft release notes PR (dispatched on semver tag via shared-sdlc)
  docs-sync.md                ← Documentation sync PR on push to main
  ci-investigator.md          ← CI failure root cause analysis
  architecture-review.md      ← Architecture review after CI passes
  vuln-triage.md              ← Vulnerability ranking from security scan
  tech-debt.md                ← Monthly tech debt scan
  compliance-report.md        ← Monthly compliance audit
  coverage-suggester.md       ← Test coverage improvement suggestions
```

The first four are vendored into every new project by `new-project.sh`; the
rest are catalogue workflows you can vendor the same way when a project needs
them.

## Editing a workflow

Every `.md` has a compiled `.lock.yml` next to it — the lock file is the
actual GitHub Actions workflow. If you change a `.md`, recompile and commit
both files:

```bash
gh extension install github/gh-aw   # once
gh aw compile
```

Consumers of precompiled locks never need to compile anything; they only
recompile if they edit the vendored `.md` in their own repo.

## Prerequisites

Before any of these workflows will actually run, see
[`shared-sdlc/docs/environment-check.md`](https://github.com/lhuasheng/shared-sdlc/blob/main/docs/environment-check.md):

- The workflows use `engine: copilot` with the `copilot-requests: write`
  permission, so GitHub Copilot (with agentic workflow / premium requests
  policy) must be enabled for the org.

## Documentation

- [`TRIGGERS.md`](TRIGGERS.md) — **start here**: how every workflow in this
  repo triggers, its inputs and safe-outputs, editing rules, and the overall
  compile → vendor → run logic.
- [`shared-sdlc/docs/triggers.md`](https://github.com/lhuasheng/shared-sdlc/blob/main/docs/triggers.md) —
  the same triggers from the project-repo side (callers, composite actions,
  tokens, verification).
- [`shared-sdlc/docs/playbook.md`](https://github.com/lhuasheng/shared-sdlc/blob/main/docs/playbook.md) —
  full architecture, integration patterns, workflow catalogue, and cost
  budgets.
