# Agent Review Workflow

A portable code review skill for **Claude Code, Cursor, and Codex**. Keep one review
procedure in Git, define what matters in your repository, and ask for evidence-backed
findings with bounded review/fix rounds.

Inspired by [How the Claude Code team does code review](https://claude.dev/blog/solving-code-review-at-anthropic/)
by Addy Osmani, Morgan Westlee Lunt, and Qing Wang. This is an independent community
implementation, with original skill instructions and templates.

## What it does

- Reviews changes against their intended behavior and the repository's `REVIEW.md`.
- Includes staged, unstaged, and relevant untracked files.
- Classifies risk by consequences and reversibility.
- Requires evidence for findings: severity, a narrow file/line reference, trigger, and impact.
- Records checks that ran, their results, and remaining uncertainty.
- Distinguishes an independent review from the builder's self-review.
- Caps a requested review/fix loop at three rounds, including the initial review.

The default review is read-only. Fixes require a request that includes fixing the findings.
Agent findings and check results are evidence; repository protections and human approval
requirements still govern merging.

## Repository layout

```text
.agents/skills/review-change/SKILL.md   Canonical portable skill
.claude/skills/review-change           Symlink to the canonical skill
AGENTS.md                             Instructions for contributing to this package
CLAUDE.md                             Symlink to AGENTS.md
REVIEW.md                             Review policy for this package
templates/REVIEW.md                   Starting policy for your application repository
templates/pull_request_template.md    Testing, risk, review, and rollback handoff
LICENSE                              MIT license for this package
```

There is no runtime package or API key to install. Use a Git repository and a host version
that supports local Agent Skills. The commands below assume a POSIX shell and symlink
support. On systems where symlinks are unavailable, copy the skill into `.claude/skills`
and update that copy whenever the canonical skill changes.

## Install in a project

### 1. Get this package

```bash
git clone https://github.com/samir-sayyed/agent-review-workflow.git "$HOME/agent-review-workflow"
review_source="$HOME/agent-review-workflow"
```

If you already cloned it, set `review_source` to that checkout instead. Run the remaining
commands from **your target project's root**.

### 2. Install the shared skill

```bash
test ! -e .agents/skills/review-change &&
test ! -L .agents/skills/review-change &&
mkdir -p .agents/skills &&
cp -R "$review_source/.agents/skills/review-change" .agents/skills/review-change
```

The command stops if that skill already exists. Review and merge an existing installation
manually. Codex and Cursor discover this project-local location.

Add the Claude Code alias:

```bash
test ! -e .claude/skills/review-change &&
test ! -L .claude/skills/review-change &&
mkdir -p .claude/skills &&
ln -s ../../.agents/skills/review-change .claude/skills/review-change
```

### 3. Add your review policy

```bash
test ! -e REVIEW.md && test ! -L REVIEW.md &&
cp "$review_source/templates/REVIEW.md" REVIEW.md
```

Customize it with your actual build/test commands, domain invariants, sensitive behavior,
and recovery steps. If `REVIEW.md` already exists, merge useful guidance into it.

Add this pointer to the project's existing `AGENTS.md` and `CLAUDE.md`, as applicable:

```markdown
For change reviews, use the review-change skill and apply REVIEW.md.
Report checks run, unresolved findings, risk, and whether the review was independent.
```

Keep the project's existing instructions. This package's own `AGENTS.md` and `REVIEW.md`
describe maintaining the package; `templates/REVIEW.md` is the application starting point.
Optionally merge `templates/pull_request_template.md` into your project's GitHub PR template.

Commit the installed skill, policy, and alias so other contributors receive them.

## Use it

Start or refresh your agent session, then confirm `review-change` appears in its skill selector.

| Host | Invoke |
|---|---|
| Claude Code | `/review-change` |
| Cursor | Type `/` in Agent chat and select `review-change` |
| Codex | `$review-change` |

Supply the intended outcome and review scope. For current edits:

> Use review-change to review my current changes against HEAD. The intended behavior is
> that retrying a failed upload preserves the original file. Apply REVIEW.md, include new
> files, and report findings without editing files.

For a branch, name the actual agreed base, such as `origin/main`, instead of HEAD.
For an independent review, open a fresh session and give it the requirements, diff, policy,
and relevant evidence. A session that implemented the change should identify its own pass
as self-review. Changing the agent host alone does not guarantee independent judgment.

## Review output

The report starts with an actionable finding count. Each finding includes severity,
file/line, trigger, wrong outcome, and evidence. Pre-existing issues are identified separately.
The report also gives:

- Reviewed base, revision, and scope.
- Risk level and its reason.
- Check commands, exit statuses, and meaningful results.
- Unresolved questions and unexecuted checks.
- Review independence and a concrete rollback suggestion.

When supported by the review, the report says **0 actionable findings** and explains its
coverage limits. During a requested fix loop, findings are tracked as fixed, disproved,
intentional, or unresolved. Re-review follows edited hunks and affected dependencies.
After three rounds, unresolved causes are surfaced for reconsideration.

## Enforcement and validation

This package supplies agent instructions. Add required CI checks, security review,
CODEOWNERS, branch protection, and approval rules in your repository host as needed.
An agent executes checks only when permitted; skipped or unavailable checks remain unknown.

Package validation covers skill metadata, canonical paths, symlink targets, documentation
installation commands, and the publishable file list. A populated temporary project was
used to check that the documented installation preserves existing files. Live review
execution in Claude Code, Cursor, and Codex has not been validated by this package.

Discovery documentation checked on October 26, 2026:
[Claude Code skills](https://code.claude.com/docs/en/skills),
[Cursor skills](https://cursor.com/docs/skills), and
[Codex skills](https://learn.chatgpt.com/docs/build-skills).
Confirm discovery after host upgrades and in remote environments.

## Contributing

Keep the procedure host-neutral and the skill description specific to change reviews.
Explain the demonstrated failure behind a proposed rule. Update installation instructions
when changing paths, and check aliases and metadata before submitting a change.

## License

Original package files are licensed under [MIT](LICENSE). Linked articles and vendor
documentation retain their respective owners' rights.
