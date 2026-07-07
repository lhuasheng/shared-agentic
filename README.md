# shared-agentic

Markdown-based [GitHub Agentic Workflows](https://docs.github.com/en/copilot/using-github-copilot/using-copilot-coding-agent)
that implement the reasoning layer of the **AI-Enhanced Software Development
Lifecycle (AI-SDLC) Framework**. These workflows are informational and
advisory — they do not block merge. Deterministic, merge-blocking gates live
in [`lhuasheng/shared-sdlc`](https://github.com/lhuasheng/shared-sdlc).

`shared-sdlc`'s `dispatch-agentic` composite action routes commands and
triggers (a `/ai-review` comment, a tag push, an opened issue) to the
workflow files here via the GitHub Actions dispatch API, pointing
`workflow-repo` at `lhuasheng/shared-agentic`.

> **This repo must be public** (or accessible via a GitHub App) so that
> `dispatch-agentic` can reach it from any project repo in the org.

---

## What's in here

```
.github/workflows/          ← GitHub Agentic Workflow Markdown files
  weekly-digest.md            ← Monday 09:00 UTC engineering digest issue
  issue-triage.md             ← Auto-classify new issues
  docs-sync.md                ← Documentation sync PR on push to main
  pr-review.md                ← AI pre-screen review (triggered by /ai-review)
  ci-investigator.md          ← CI failure root cause analysis
  release-notes.md            ← Draft release notes PR on semver tag
  architecture-review.md      ← Architecture review after CI passes
  vuln-triage.md              ← Vulnerability ranking from security scan
  tech-debt.md                ← Monthly tech debt scan
  compliance-report.md        ← Monthly compliance audit
  coverage-suggester.md       ← Test coverage improvement suggestions
```

## Prerequisites

Before any of these workflows will actually run when dispatched, see
[`shared-sdlc/docs/environment-check.md`](https://github.com/lhuasheng/shared-sdlc/blob/main/docs/environment-check.md):

- GitHub Copilot's coding agent must be enabled for this org (**Settings →
  Copilot → Coding agent**).
- The `gh aw` extension compiles these `.md` sources — verify it's available
  wherever these workflows are validated.

## Documentation

Full architecture, integration patterns, workflow catalogue, and cost
budgets are documented in
[`shared-sdlc/docs/playbook.md`](https://github.com/lhuasheng/shared-sdlc/blob/main/docs/playbook.md).
