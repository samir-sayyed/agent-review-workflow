# Package review policy

This package fails when the skill misroutes a request, installation replaces a user's
existing configuration, a host cannot find the canonical skill, or the report claims
proof or authority it does not have.

## Check the affected contract

- Keep the skill portable across Claude Code, Cursor, and Codex. Vendor-specific syntax
  belongs in the README. Check discovery claims against the linked official documentation.
- Confirm required manifest metadata, canonical paths, relative aliases, and installation
  commands whenever changing the package structure.
- Try documented installation in a temporary project, including existing destination files.
  Existing instructions and skills must be preserved.
- Preserve full requested diff coverage, including relevant untracked files, evidence-backed
  findings, read-only review by default, and explicit uncertainty for checks that did not run.
- Preserve the distinction between self-review, independent review, and human approval.
- Keep the three-round boundary meaningful: unresolved findings remain unresolved at the cap.
- Run `git diff --check`, and report the actual checks and limits.

## Risk and findings

Copy changes are usually low risk. Changes to discovery, scope, or findings behavior need
closer review. Installation, permission, publishing, or approval changes need attention to
their consequences and the existing authorization rules. Risk labels do not authorize a
merge or publication.

Lead with the actionable finding count. Give severity, file/line, trigger, impact, and
evidence. Distinguish pre-existing issues, omit unsupported hypotheses, and limit optional
nits to two. When no actionable defect is found, state the review's coverage limits.
