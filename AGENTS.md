# Agent Review Workflow

This public package contains a portable review skill and policy templates. The canonical
skill is `.agents/skills/review-change/SKILL.md`; `.claude/skills/review-change` aliases it.
`CLAUDE.md` aliases this file. Edit canonical files so the aliases stay consistent.

For change reviews, apply `REVIEW.md`. Inspect the README when changing installation or
invocation. Use `templates/` for the policy and PR handoff users copy into other repos.

Validate manifest metadata, relative symlink targets, and the documented installation in
a temporary project. Run `git diff --check`. Keep validation claims scoped to what ran;
host discovery, live agent execution, and hosted merge enforcement require separate evidence.
