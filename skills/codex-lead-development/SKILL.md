---
name: codex-lead-development
description: Lead an end-to-end coding task in OpenAI Codex from repository setup and planning through adaptive subagent delegation, independent review, verification, and pull-request delivery. Use when the user explicitly invokes this skill or asks Codex to own a non-trivial feature, bug fix, refactor, or migration and deliver a reviewed PR. Do not trigger for explanation-only requests, small one-off edits, or review-only work.
---

# Codex Lead Development

Own the outcome as the technical lead. Keep the workflow proportional to the task: preserve the quality gates, but skip ceremony that does not improve correctness or speed.

## Establish the workspace

1. Read repository instructions and inspect the relevant code and VCS state.
2. State the goal, final deliverable, success criteria, hard constraints, and explicit out-of-scope items. Ask only when a missing decision would materially change the result.
3. Protect unrelated user changes. Use the repository-native VCS and create a dedicated feature branch unless the user or repository workflow says otherwise. Use an isolated worktree when required, when the current tree is dirty, or when agents may write concurrently.
4. Maintain a short live plan covering implementation, validation, review, fixes, and PR delivery. Do not create a separate plan document unless it will remain useful after the task.

## Size the task before delegating

Choose the lightest structure that preserves an independent quality check.

- **Trivial:** implement directly, run focused validation, and inspect the diff.
- **Simple:** implement directly or use one bounded implementer; request one fresh review when the change can regress behavior.
- **Medium:** delegate one substantial implementation or investigation task, then use a separate reviewer.
- **Complex or uncertain:** split into two or three large, non-overlapping workstreams such as architecture/research, implementation, and test strategy. Parallelize only independent work and keep dependent work sequential.

Delegate when fresh context, specialization, context isolation, or parallelism outweighs coordination cost. Do not spawn agents for tiny lookups, routine commands, or work the lead can finish faster.

Keep delegation flat unless explicitly allowed. Give every subagent one owner-sized task with relevant paths, boundaries, expected evidence, verification requirements, and a concise return contract. Tell parallel agents whether to wait for peers and wait for every required result before integration.

Prefer parallelism for read-heavy exploration, tests, triage, and review. For write-heavy work, define file ownership and use isolated branches or worktrees; otherwise keep one writer at a time. Ask agents to return distilled findings and artifact paths instead of flooding the main thread with raw logs.

## Route Codex models intentionally

Honor an explicit user model choice. Otherwise set every delegated agent to `high` reasoning by default.

- **`gpt-5.6-luna`:** use for fast, narrow, repeatable, or high-volume work such as repository mapping, log inspection, mechanical checks, and test-case enumeration.
- **`gpt-5.6-terra`:** use as the default implementer for clear, localized features, bug fixes, tests, and read-heavy scans.
- **`gpt-5.6-sol`:** use for complex or cross-cutting implementation, architecture, ambiguous debugging, risky migrations, and independent code review.

The only effort exception is Luna: freely use `max` for broad hypothesis generation, exhaustive probes, or wide fan-out because it is the economical tier. Use Sol for the final independent review whenever available; a cheaper model must not be the sole judge of its own output.

## Execute and integrate

Understand the affected flow and existing tests before editing. Prefer the smallest coherent change that satisfies the request.

Require implementers to report changed files, design choices, commands run, failures, and remaining uncertainty. Inspect the actual diff and run integration checks yourself; never accept a subagent summary as final evidence. Before declaring completion, confirm that all required subagents have returned and no relevant work is still active.

## Review adaptively

- For a small or localized change, use one fresh Sol reviewer focused on correctness, regressions, tests, and maintainability.
- For a complex, risky, or cross-cutting change, use up to three independent Sol reviewers with distinct scopes: correctness/data flow, architecture/maintainability, and tests/edge cases/security.
- Require actionable findings with severity, exact location, failure scenario, and suggested direction. Treat unsupported preferences as optional.

Triage findings against the code and requirements instead of accepting them blindly. For a disputed or high-impact finding, use a fresh Sol verifier to confirm or refute it with concrete evidence before changing code.

Fix clear local issues directly or return them to the original implementer, then re-review only the changed or disputed area unless the overall design changed. If the same material issue survives two fix cycles, hand it to a fresh Sol implementer with the original brief, review evidence, and attempted fixes. Stop an unproductive loop and ask the user only when a material product or architecture decision is genuinely required.

## Verify and deliver

Run focused tests first, then the strongest broader checks proportionate to risk: tests, lint, typecheck, build, or repository-specific validation. Record commands and outcomes. Inspect the final diff and VCS status for accidental or unrelated changes.

Create focused commits and publish only when authorized. Open a PR following repository conventions; default to draft while material work remains. Include:

- what changed and why;
- validation performed and its results;
- notable decisions, risks, and follow-ups.

When CI or automated review is available, continue until actionable failures and material feedback are resolved: diagnose the root cause, fix it, rerun covering checks, commit, push, and reassess. Stop and report the exact blocker when the same failure survives two attempts, infrastructure is broken, or only human gates remain.

Do not merge unless the user explicitly asks. Finish with the branch, PR URL, validation evidence, and any remaining risk or manual gate.
