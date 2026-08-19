# AI-Assisted Development Workflow

## Purpose

This project uses AI (Claude Code) as part of a controlled engineering workflow, not as an autonomous source of product requirements. AI tooling accelerates specification, planning, implementation, and verification, but every product decision still traces back to a human: the Product Owner or the Developer. This document explains how that control is enforced so any engineer — human or AI — can pick up the process without prior conversation history.

Two systems divide the work:

- **Spec Kit** owns specification and planning.
- **Superpowers** owns disciplined implementation and verification.

Spec Kit plans the work. Superpowers executes and verifies the work. Neither system is used to replace the other's output.

## Roles

### Product Owner

The Loot Card Shop owner is the Product Owner. The Product Owner validates the real-world workflow and business requirements — what the product must do and why. See [`docs/prd/Loot_Allocation_Product_Owner_Review_Brief.md`](../prd/Loot_Allocation_Product_Owner_Review_Brief.md) for the open questions currently pending Product Owner review.

### Developer

The developer owns architecture, implementation decisions, code review, and final approval. Spec Kit and Superpowers assist the developer; they do not replace developer judgment or sign-off.

### Spec Kit

Owns product and feature specification, requirements clarification, the project constitution (engineering principles), technical planning, and breaking planned work into tasks.

### Superpowers

Owns implementation discipline: test-driven development, executing approved plans, systematic debugging, specification compliance review, code quality review, verification before completion, and finishing feature branches.

### Claude Code

Acts as the AI development agent operating within these constraints, using Spec Kit skills during planning and Superpowers skills during implementation.

## Workflow

```text
1. Product Owner feedback or approved PRD requirement
2. Spec Kit specification            (/speckit-specify)
3. Spec Kit clarification            (/speckit-clarify)
4. Spec Kit technical plan           (/speckit-plan)
5. Spec Kit task breakdown           (/speckit-tasks)
6. Human review / approval
7. Create GitHub issue / feature branch
8. Superpowers executes approved tasks
9. Tests written and run (TDD)
10. Systematic debugging if failures occur
11. Verify implementation against specification
12. Code quality review
13. Pull request
14. CI checks
15. Human review
16. Merge
```

The implementation agent must treat approved Spec Kit artifacts (the specification, plan, and task list under `specs/`) as the source of truth for *what* to build. Superpowers governs *how* it gets built and verified.

If, during implementation, Superpowers (or the developer) discovers:

- A contradiction between artifacts,
- A missing requirement,
- An unclear business rule,
- An architectural conflict, or
- A requirement that cannot reasonably be implemented as specified,

implementation must **stop and surface the issue** rather than silently redesigning the feature. The fix is to go back to Spec Kit (re-run `/speckit-clarify` or `/speckit-specify` as appropriate), not to improvise a new plan mid-implementation.

## Source of Truth Hierarchy

When artifacts conflict, the higher item wins. Conflicts that matter should be escalated for clarification rather than silently resolved by whichever agent notices them first.

1. Confirmed Product Owner decisions
2. Approved PRD
3. Approved Spec Kit feature specification
4. Approved Spec Kit technical plan
5. Approved task breakdown
6. Implementation

## Development Practices

- Feature branches for all product work; no direct commits to `main`.
- Small, reviewable commits and pull requests.
- Test-driven development where appropriate (Superpowers' `test-driven-development` skill).
- Automated tests must pass before a branch is considered done.
- Code review before merge (Superpowers' `requesting-code-review` skill, plus human review).
- Verification against the approved specification before a task is marked complete.
- No secrets committed to source control.
- No unapproved paid Azure services or tier upgrades — this project is explicitly cost-constrained to free-tier usage.

## Product Discovery State

This project is still validating parts of the real Loot Card Shop workflow. Proposed requirements in the current PRD are not automatically "confirmed" just because they're written down — the [Product Owner Review Brief](../prd/Loot_Allocation_Product_Owner_Review_Brief.md) exists specifically to validate those assumptions. Any feature that depends on an unanswered Product Owner question should remain pending rather than being specified or implemented as if it were settled.

## Where Things Live

- `.specify/memory/constitution.md` — project constitution (engineering principles), managed via `/speckit-constitution`.
- `specs/` — Spec Kit specifications, plans, and task breakdowns (created as features are specified).
- `.claude/skills/speckit-*` — Spec Kit skills, committed to the repo (`speckit-constitution`, `speckit-specify`, `speckit-clarify`, `speckit-plan`, `speckit-tasks`, `speckit-implement`, `speckit-converge`, plus optional `speckit-analyze`, `speckit-checklist`, `speckit-taskstoissues`).
- `.claude/settings.json` — enables the `superpowers` Claude Code plugin (`superpowers@claude-plugins-official`) at project scope, so anyone who opens this repo in Claude Code has it available automatically. Superpowers itself installs at the user level, not into this repo; only the enablement pointer is committed.
- Superpowers skills relevant to implementation: `writing-plans`, `executing-plans`, `test-driven-development`, `systematic-debugging`, `verification-before-completion`, `requesting-code-review` / `receiving-code-review`, `finishing-a-development-branch`.
- `docs/prd/` — the PRD and Product Owner Review Brief; authoritative for product intent once Product Owner-approved.
