# Contributing Guide

This document is the single source of truth for how the team works on this repository. Every team member is expected to follow it. Rules marked **(enforced)** are checked automatically by GitHub rulesets or CI.

## Table of Contents

- [Workflow at a Glance](#workflow-at-a-glance)
- [Branching](#branching)
- [Commits](#commits)
- [Pull Requests](#pull-requests)
- [Code Review](#code-review)
- [Issues and Planning](#issues-and-planning)
- [Phases and Releases](#phases-and-releases)
- [Definition of Done](#definition-of-done)
- [General Rules](#general-rules)

## Workflow at a Glance

```text
issue  →  branch P<n>/<Feature>  →  commits  →  PR  →  review + CI  →  squash merge  →  branch deleted
```

1. Pick or create an issue and assign yourself.
2. Create a branch from the latest `main`.
3. Commit in small, meaningful steps.
4. Open a pull request that links the issue.
5. Get at least one approval and green CI.
6. Squash merge. The branch is deleted automatically.

## Branching

- `main` is always deployable/demoable. **Direct pushes, force pushes, and deletion are blocked (enforced).**
- Every change is made on a short-lived branch named:

  ```text
  P<PhaseNumber>/<FeatureName>
  ```

  | Part            | Rule                                   | Example         |
  | --------------- | -------------------------------------- | --------------- |
  | `P<PhaseNumber>`| `P` + phase number, no leading zeros   | `P1`, `P2`      |
  | `<FeatureName>` | PascalCase, letters and digits only    | `UserAuth`      |

  Valid: `P1/UserAuth`, `P2/PaymentApi`, `P2/FixLoginRedirect`, `P3/Docs`
  Invalid: `p1/userAuth`, `P1/user-auth`, `feature/login`, `P1/Login_Page`

  The branch name is validated by CI on every PR **(enforced)**.

- Use `P0/<Name>` for repository setup work that belongs to no phase (tooling, CI, docs scaffolding).
- One branch = one feature / one issue. Do not mix unrelated work.
- Keep branches short-lived (ideally merged within a few days). Rebase or merge `main` into your branch regularly to avoid large conflicts.

```bash
git switch main
git pull
git switch -c P1/UserAuth
```

## Commits

We use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

```text
<type>(<optional scope>): <short summary in imperative mood>

<optional body: what and why, not how>

<optional footer: Closes #12, BREAKING CHANGE: ...>
```

| Type       | Use for                                             |
| ---------- | --------------------------------------------------- |
| `feat`     | A new feature                                       |
| `fix`      | A bug fix                                           |
| `docs`     | Documentation only                                  |
| `style`    | Formatting, no logic change                         |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `perf`     | Performance improvement                             |
| `test`     | Adding or fixing tests                              |
| `build`    | Build system or dependencies                        |
| `ci`       | CI configuration                                    |
| `chore`    | Maintenance that doesn't fit the above              |
| `revert`   | Reverting a previous commit                         |

Rules:

- **Commit at meaningful milestones.** A commit should represent one coherent, working step (e.g. "login form validates input"), not "wip", "asdf", or "fixed stuff".
- Summary line: imperative mood ("add", not "added"), lowercase, no trailing period, max 72 characters.
- Each commit should build and not break existing behavior.
- Never commit secrets, `.env` files, credentials, or large binaries.
- **No AI attribution (enforced).** Commits and PR descriptions must not contain `Co-Authored-By` trailers for AI tools (Claude, Copilot, Cursor, Codex, ChatGPT, ...), "Generated with ..." notes, or AI tool emails. GitHub would list the AI as a contributor. If your tool adds these automatically, turn that off (see [AGENTS.md](AGENTS.md)) or remove them before pushing.

Good:

```text
feat(auth): add email/password login form
fix(cart): prevent negative item quantities
docs(readme): add local setup instructions
```

Bad:

```text
update
fixed bug
WIP final final v2
```

## Pull Requests

- **All changes to `main` go through a PR (enforced).**
- **PR title must follow Conventional Commits (enforced by CI).** Because we squash merge, the PR title becomes the commit on `main`.
- Fill in the PR template completely. Link the issue with `Closes #<number>`.
- Keep PRs small and focused (aim for < 400 changed lines). Split large work into several PRs.
- Open a **Draft PR** early if you want feedback before it's finished.
- Before requesting review: rebase on / merge latest `main`, make sure CI is green, and self-review your diff.
- Merge strategy: **Squash and merge only.** The branch is deleted automatically after merge.

## Code Review

- **At least 1 approval from another team member is required (enforced).** You cannot approve your own PR.
- New commits pushed after approval dismiss the approval; re-review is required **(enforced)**.
- All review conversations must be resolved before merging **(enforced)**.
- Reviewers should respond within **24 hours** on working days.
- Review the code, not the person. Be specific and suggest alternatives.
- Prefix optional comments with `nit:` so the author knows they are non-blocking.
- The **author** merges their own PR after approval.

## Issues and Planning

- Every piece of work starts with an issue (use the Bug or Feature templates).
- Each issue is assigned to exactly one owner and to a phase **milestone**.
- Use labels for type (`type: feature`, `type: bug`, ...) and phase (`phase: P1`, ...).
- Track progress on the GitHub Project board: `Todo → In Progress → In Review → Done`.

## Phases and Releases

- Work is organized in phases (`P1`, `P2`, ...), each tracked as a GitHub milestone.
- When a phase is complete, a maintainer:
  1. Updates [`CHANGELOG.md`](CHANGELOG.md) (via a PR).
  2. Tags `main` as `v0.<phase>.0` (e.g. `v0.1.0` for P1).
  3. Publishes a GitHub Release with notes.
- Version `v1.0.0` marks the final delivered project.

```bash
git switch main && git pull
git tag -a v0.1.0 -m "P1: <phase goal>"
git push origin v0.1.0
```

## Definition of Done

A task is done only when all of the following are true:

- [ ] Code is merged to `main` via an approved PR with green CI.
- [ ] Acceptance criteria in the issue are met.
- [ ] Tests are added/updated where applicable.
- [ ] Documentation (README, `docs/`) is updated where applicable.
- [ ] No new warnings, debug logs, or commented-out code.
- [ ] The issue is closed and the board is updated.

## General Rules

- Communicate blockers early in the team channel; don't sit on them.
- Don't rewrite shared history: no force-pushing to branches someone else is working on.
- Agree on new dependencies in the PR description (why it's needed, alternatives).
- Keep the repository clean: no personal notes, IDE files, or generated artifacts.
- Tooling-specific rules (formatter, linter, test command) will be added here once the tech stack is chosen.
