---
name: review-change
description: Review a working diff or branch against its intended behavior and repository review policy. Use for a requested change review or PR readiness assessment, rather than a whole-repository audit.
---

# Review a change

Produce evidence-backed findings and a clear account of what was actually verified.
Review is read-only unless the user also requested fixes.

1. Establish the intended behavior and review base from the request. For current edits,
   default to HEAD; for a branch, use the agreed base. If the intended outcome is missing,
   infer what the diff demonstrates and label uncertainty. Read the applicable repository
   instructions and `REVIEW.md` when present; a nested policy supplements the root policy.

2. Inspect `git status --short`, the diff against the base, and
   `git ls-files --others --exclude-standard`. Include staged, unstaged, and relevant
   untracked additions. For a deleted or renamed path, follow its callers and references.
   Open connected code or source passages needed to validate the changed behavior.
   Completion means the full requested scope is accounted for, including new files.

3. Classify risk from the failure's consequences and reversibility. Separate behavior,
   permissions/security, data compatibility, and release risks where relevant. Apply the
   repository's policy and runnable checks; record the command, exit status, and useful
   result. Execute only checks permitted in the current review. Missing tooling, skipped
   checks, and unavailable environments remain unknown. A document claiming success is
   not execution evidence.

4. Validate each candidate finding against the implementation, its callers, and the
   requirements. Report a reproducible or evidenced wrong outcome with severity and a
   narrow file/line reference. Distinguish pre-existing issues and uncertainty. Prefer
   one finding describing every confirmed instance of a bug class over repeated comments.
   Leave style-only cleanup to existing formatting and lint tools.

5. Return the actionable finding count first, then findings ordered by severity. Include
   the reviewed base and scope, risk with reason, check results, unresolved questions,
   review independence, and a concrete rollback suggestion. Say `0 actionable findings`
   when supported; describe the review's limits. An agent's assessment does not count as
   human approval or permission to merge.

An independent review uses a fresh session or an available, authorized reviewer with the
requirements, diff, applicable policy, and evidence. If this session built the change,
label its pass self-review and state whether independent review remains outstanding.

For a requested review/fix loop, count the initial review as round one and cap the loop
at three rounds. Re-review changed hunks, their affected dependencies, and previous
findings. After the cap, surface the unresolved cause and reconsider the approach before
continuing. Preserve an explicit disposition for each finding: fixed, disproved with
evidence, intentional with rationale, or unresolved. Recheck relevant proof after edits;
evidence and approvals apply only to the revision reviewed.
