# Loot Allocation

## Project Overview

Loot Allocation is a release, ordering, and distributor allocation management application for collectible card and hobby stores. It helps a store track upcoming releases, distributor solicitations, requested quantities, allocated quantities, allocation revisions, and actual received product — all in one place.

Loot Allocation is not intended to replace a store's POS or conventional inventory system. It manages the period *before and during* product acquisition, not day-to-day sellable inventory.

## Problem Being Solved

Conventional retail inventory systems assume a simple model: inventory is low, so you reorder more. That model doesn't hold for many collectible products.

A collectible release instead tends to look like: a product is announced, a distributor sends a solicitation, the store requests a quantity, the distributor allocates some amount (often less than requested), that allocation can change before shipment, product arrives (sometimes across multiple partial shipments), and additional waves may follow later.

Requested quantity, allocated quantity, revised allocation, and received quantity are all different, meaningful values — and collapsing them into a single "quantity on hand" number throws away information the store needs. Loot Allocation is designed to preserve that entire story, including allocation history over time, so it becomes useful institutional knowledge rather than something scattered across emails, spreadsheets, and memory.

## Current Status

> The project is currently in product discovery and workflow validation. Application development has not begun.

This repository currently contains project documentation and repository scaffolding only.

## Proposed Technology Stack

- React
- TypeScript
- ASP.NET Core Web API
- Entity Framework Core
- Azure SQL Database
- Azure Static Web Apps
- Azure Container Apps
- Microsoft Azure

These are the accepted technical decisions in the current PRD. They are separate from the product requirements themselves, which are still being validated (see below).

## Documentation

- [`docs/prd/Loot_Allocation_PRD_v0.2.md`](docs/prd/Loot_Allocation_PRD_v0.2.md) — full Product Requirements Document
- [`docs/prd/Loot_Allocation_Product_Owner_Review_Brief.md`](docs/prd/Loot_Allocation_Product_Owner_Review_Brief.md) — workflow assumptions pending Product Owner validation
- `docs/architecture/` — reserved for architecture documentation as it is produced
- `docs/decisions/` — reserved for architecture decision records

## Development Status

### Confirmed Technical Decisions

- React + TypeScript frontend
- ASP.NET Core Web API backend
- Entity Framework Core for data access and migrations
- Azure SQL Database as the authoritative datastore
- Azure hosting (Static Web Apps + Container Apps)
- Cloud-first persistence — production data must not depend on a local developer machine
- Azure cost containment as an explicit architectural requirement

### Proposed Product Requirements

The domain model, MVP feature set, dashboard behavior, and workflow described in the PRD are proposed requirements, not finalized ones. They are grounded in the product's core principles (requested ≠ allocated ≠ received, allocation history must be preserved, one product may involve multiple distributors) but the surrounding workflow details are still subject to Product Owner review.

### Pending Discovery

The [Product Owner Review Brief](docs/prd/Loot_Allocation_Product_Owner_Review_Brief.md) lists open workflow questions that need to be answered before the MVP is finalized, including how distributors communicate solicitations and allocation changes, whether requests are made in cases or units, how later product waves should be represented, and whether internal allocation planning (splitting allocated product across in-store, online, events, etc.) belongs in the first version at all.

## Product Owner

The Loot Card Shop owner is serving as the Product Owner for this project. The current proposed workflow must be validated with them before MVP requirements are treated as final. Unvalidated assumptions are documented as assumptions, not as confirmed requirements.

## Cost Safety

The architecture is intentionally designed around Azure free-tier usage (Azure SQL free offer, Static Web Apps Free plan, Container Apps scaled to zero when idle). Avoiding accidental cloud charges is an explicit project requirement: where a free tier supports pausing or stopping service instead of incurring paid overage, that behavior is required. Budget alerts and resource isolation are part of the planned safeguards.
