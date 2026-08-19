# CLAUDE.md

Guidance for Claude Code (and other AI agents) working in this repository.

## Project

Loot Allocation is a release, ordering, and distributor allocation management application for collectible card and hobby stores. See [`README.md`](README.md) for the project overview and [`docs/prd/Loot_Allocation_PRD_v0.2.md`](docs/prd/Loot_Allocation_PRD_v0.2.md) for the full PRD.

The project is currently in product discovery and workflow validation. Application development has not begun; do not scaffold product features, API endpoints, domain entities, database migrations, or infrastructure unless explicitly asked to and unless the underlying requirement is Product Owner-approved (see below).

## Development Workflow

This repository uses **Spec Kit** for planning and **Superpowers** for implementation, with clearly separated responsibilities. Full details: [`docs/development/ai-assisted-development-workflow.md`](docs/development/ai-assisted-development-workflow.md).

```text
Product Owner Feedback
        |
       PRD
        |
     Spec Kit  --  Specify / Clarify / Plan / Tasks
        |
Approved Spec Kit Tasks
        |
    Superpowers  --  TDD / Implementation / Debugging / Verification / Review
        |
   Pull Request -> CI -> Human Review -> Merge
```

The key rule: **Spec Kit plans the work. Superpowers executes and verifies the work.** Do not create parallel, competing plans between the two.

### Product Requirements

The PRD and Product Owner-approved requirements are authoritative for product behavior. Do not invent new business requirements. Proposed requirements that are still pending Product Owner validation (see [`docs/prd/Loot_Allocation_Product_Owner_Review_Brief.md`](docs/prd/Loot_Allocation_Product_Owner_Review_Brief.md)) remain proposed, not confirmed, until validated.

### Spec Kit

Use Spec Kit for:

- Feature specification (`/speckit-specify`)
- Requirement clarification (`/speckit-clarify`)
- Technical planning (`/speckit-plan`)
- Task breakdown (`/speckit-tasks`)

Spec Kit artifacts are authoritative during implementation once they have been approved by the developer (and, for product behavior, the Product Owner).

### Superpowers

Use Superpowers, after planning is complete, for:

- Test-driven development
- Implementation
- Systematic debugging
- Verification
- Specification compliance review
- Code quality review
- Branch completion

### Important Boundary

Do not use Superpowers' brainstorming or planning skills to replace an existing approved Spec Kit plan. If a Spec Kit plan is incomplete or contradictory, raise the issue and resolve it through Spec Kit (re-clarify, re-plan) before implementation continues — don't silently redesign the feature mid-implementation.

### No Unapproved Scope Expansion

Do not add features merely because they seem useful. Product functionality must trace back to an approved PRD requirement, direct Product Owner feedback, or an approved feature specification.

### Definition of Done

A feature is not complete merely because the code runs. Before calling work complete:

- Required automated tests pass
- Existing tests pass
- Implementation matches the approved specification
- Relevant error cases are handled
- No secrets are committed
- No unexpected Azure cost impact has been introduced (this project is free-tier constrained — see [Cost Safety](README.md#cost-safety))
- Code has been reviewed
- Documentation is updated where necessary

## Architecture

Confirmed technical decisions (see [README.md](README.md#confirmed-technical-decisions)):

- React + TypeScript frontend
- ASP.NET Core Web API backend
- Entity Framework Core for data access and migrations
- Azure SQL Database as the authoritative datastore
- Azure hosting (Static Web Apps + Container Apps)
- Cloud-first persistence — production data must not depend on a local developer machine
- Azure cost containment as an explicit architectural requirement; do not provision paid Azure resources or upgrade tiers without explicit approval
