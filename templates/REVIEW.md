# Project change review policy

Review the intended behavior against the requested change and the connected code needed
to assess it. Record what failure would look like for the people using this project.

## Evidence

- Run the affected build, lint, and test commands documented by this project. Record each
  command, exit status, and meaningful result. Unavailable or skipped checks remain unknown.
- For a bug fix, demonstrate the failure and the intended recovery with an appropriate
  regression check. Choose tests that assert behavior independently of the implementation.
- For a UI change, inspect the resulting interface and record relevant visual evidence.
- Check compatibility and recovery when a change affects persistent data, permissions,
  external contracts, or release behavior.

Add the project's exact commands and domain invariants here as they are established.

## Risk

| Level | Typical consequences | Review emphasis |
|---|---|---|
| Low | Small, reversible copy or internal changes | Targeted checks and stated intent |
| Medium | Product behavior, shared abstractions, integrations | Affected callers, regression proof, fresh-context review when available |
| High | Authentication, payments, destructive data changes, permissions, CI or releases | Failure scenarios, recovery, and the repository's required human review |

Classify the actual consequences. Apply the existing authorization and merge rules;
an agent's assessment never fills a required human approval slot.

## Findings and completion

Lead with the actionable finding count. Each finding names severity, file/line, trigger,
wrong outcome, and evidence. Check callers before reporting a hypothesis as a defect.
Mark pre-existing issues separately. Limit optional nits to two.

Use fresh context for an independent review, and label a builder's own pass self-review.
Count the initial review as round one and cap a requested review/fix loop at three rounds.
Re-review edited hunks, affected dependencies, and earlier findings; report unresolved
causes at the cap. Evidence applies to the reviewed revision.

Finish with check results, risk, remaining uncertainty, and a concrete rollback suggestion.
