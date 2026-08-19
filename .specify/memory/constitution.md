<!--
Sync Impact Report
Version change: [TEMPLATE] → 1.0.0 (initial ratification)
Rationale: MAJOR — first concrete constitution for this project; all placeholders filled for the
first time, establishing the baseline governance contract.
Modified principles: none (template placeholders → concrete principles, not a renaming)
Added sections:
  - Core Principles I–V (Product Requirements Traceability, Spec Kit Plans / Superpowers
    Executes, Test-First Implementation, Cloud-First Cost-Constrained Architecture,
    Transparent Reviewable Change)
  - Source of Truth Hierarchy (SECTION_2)
  - Development Workflow (SECTION_3)
  - Governance
Removed sections: none
Deferred TODOs: none — RATIFICATION_DATE set to repository initialization date (2026-08-19),
the date this constitution content was first authored; no earlier informal agreement predates it.
Templates requiring follow-up: none — dependent Spec Kit templates/commands read this constitution
at runtime and are not modified by this command per the scope guard.
-->

# Loot Allocation Constitution

## Core Principles

### I. Product Requirements Traceability (NON-NEGOTIABLE)
All product functionality MUST trace back to an approved PRD requirement, a confirmed Product
Owner decision, or an approved Spec Kit feature specification. Requirements still pending
Product Owner validation (tracked in the
[Product Owner Review Brief](../../docs/prd/Loot_Allocation_Product_Owner_Review_Brief.md))
remain proposed, not confirmed, and MUST NOT be implemented as if settled. Application source
files, API endpoints, domain entities, database migrations, or infrastructure MUST NOT be
scaffolded unless explicitly requested and the underlying requirement is Product
Owner-approved.
Rationale: this project is still in product discovery for a domain (distributor allocation
workflows) that the team does not yet have fully validated with the actual store owner.
Building ahead of validation risks a data model and workflow the business can't actually use.

### II. Spec Kit Plans, Superpowers Executes
Spec Kit (specification, clarification, technical planning, task breakdown) and Superpowers
(TDD, implementation, debugging, verification, code review) have non-overlapping
responsibilities and MUST NOT be used as substitutes for one another. Approved Spec Kit
artifacts under `specs/` are the source of truth for *what* to build; Superpowers governs *how*
it is built and verified. If implementation surfaces a contradiction between artifacts, a
missing requirement, an unclear business rule, or an architectural conflict, work MUST stop and
the issue MUST be escalated back to Spec Kit (re-run `/speckit-clarify` or `/speckit-specify`)
rather than silently redesigned mid-implementation.
Rationale: prevents two competing, drifting plans and keeps AI-assisted changes auditable
against a single approved specification.

### III. Test-First Implementation (NON-NEGOTIABLE)
Features and bugfixes MUST follow test-driven development where applicable: tests written and
reviewed before implementation code, red before green. A task is not complete until required
automated tests pass, all pre-existing tests still pass, and relevant error cases are handled —
code that merely runs is not done.
Rationale: this is the concrete mechanism that keeps AI-assisted implementation verifiable
rather than just plausible-looking, and is the primary defense against regressions in a
solo/small-team review setting.

### IV. Cloud-First, Cost-Constrained Architecture
Production data MUST NOT depend on a local developer machine; Azure SQL Database is the
authoritative datastore. The confirmed stack — React + TypeScript frontend, ASP.NET Core Web
API backend, Entity Framework Core for data access and migrations, Azure Static Web Apps, and
Azure Container Apps — is the implementation baseline; changing it is an architecture decision
that MUST be recorded (e.g. under `docs/decisions/`), not an incidental choice made mid-feature.
No paid Azure resources or tier upgrades MUST be provisioned without explicit approval: the
project is deliberately scoped to free-tier usage (Azure SQL free offer, Static Web Apps Free
plan, Container Apps scaled to zero when idle).
Rationale: the project has an explicit, non-negotiable cost ceiling. Free-tier discipline has to
be enforced at the same level as functional correctness, or it erodes one convenient feature at
a time.

### V. Transparent, Reviewable Change
All product work happens on feature branches with small, reviewable commits and pull requests —
no direct commits to `main`. No secrets are committed to source control. Every implemented task
MUST be verified against its approved specification, and code MUST be reviewed (Superpowers'
`requesting-code-review` skill plus human review) before merge.
Rationale: keeps a small team (Developer + Product Owner) able to trust and audit AI-assisted
changes without having to re-derive context from scratch for every change.

## Source of Truth Hierarchy

When project artifacts conflict, the higher item wins. Conflicts that matter MUST be escalated
for clarification rather than silently resolved by whichever agent notices them first:

1. Confirmed Product Owner decisions
2. Approved PRD
3. Approved Spec Kit feature specification
4. Approved Spec Kit technical plan
5. Approved task breakdown
6. Implementation

## Development Workflow

Product work follows this pipeline, described fully in
[`docs/development/ai-assisted-development-workflow.md`](../../docs/development/ai-assisted-development-workflow.md):

1. Product Owner feedback or approved PRD requirement
2. Spec Kit specification (`/speckit-specify`)
3. Spec Kit clarification (`/speckit-clarify`)
4. Spec Kit technical plan (`/speckit-plan`)
5. Spec Kit task breakdown (`/speckit-tasks`)
6. Human review / approval
7. Feature branch created
8. Superpowers executes approved tasks (TDD, debugging, verification, review)
9. Pull request → CI checks → human review → merge

Roles: the Loot Card Shop owner is the **Product Owner**, validating real-world workflow and
business requirements. The **Developer** owns architecture, implementation decisions, and final
sign-off. **Spec Kit** and **Superpowers** assist the Developer; they do not replace Developer
judgment.

## Governance

This constitution supersedes conflicting informal practices for this repository. Amendments are
made via `/speckit-constitution` and MUST update the Sync Impact Report, version, and dates in
the same change. Amendments affecting product-facing principles (Principle I) or the cost/
architecture baseline (Principle IV) additionally require Product Owner or Developer sign-off,
consistent with the Source of Truth Hierarchy above.

**Versioning policy** (semantic versioning for this document):
- **MAJOR** — backward-incompatible governance changes: principle removals or redefinitions.
- **MINOR** — new principle or section added, or existing guidance materially expanded.
- **PATCH** — clarifications, wording fixes, non-semantic refinements.

All pull requests and Spec Kit artifacts MUST be checked for compliance with this constitution.
Any complexity that appears to conflict with a principle (e.g. scaffolding ahead of an approved
requirement, skipping tests, introducing a paid Azure tier) MUST be explicitly justified in the
PR/spec or removed. Runtime, day-to-day development guidance for AI agents lives in
[`CLAUDE.md`](../../CLAUDE.md); this constitution defines the non-negotiable principles that
guidance must not contradict.

**Version**: 1.0.0 | **Ratified**: 2026-08-19 | **Last Amended**: 2026-08-19
