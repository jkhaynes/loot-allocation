# Loot Allocation
## Product Requirements Document

**Version:** 0.2  
**Status:** Discovery / Initial Definition  
**Working Product Name:** Loot Allocation  
**Primary Business:** Loot Card Shop

---

# 1. Product Summary

Loot Allocation is a release, ordering, and distributor allocation management application designed for collectible card and hobby stores.

Unlike conventional retail inventory systems, collectible stores cannot assume that low inventory can simply be replenished. Highly desirable products may be allocation constrained, requests may be partially fulfilled, quantities may change before release, and additional product may arrive in later waves.

Loot Allocation is intended to manage the period **before and during product acquisition**, giving stores a single place to understand:

- What products are coming
- Which distributors are offering them
- When orders are due
- What quantity was requested
- What quantity was ultimately allocated
- What quantity was actually received
- How distributor allocations have behaved historically
- What still requires action

The application is **not intended to replace the store's POS or conventional inventory system**.

---

# 2. Problem Statement

Managing sealed collectible inventory begins well before inventory arrives at the store.

A typical retail inventory system might operate on a model like:

> Inventory is low → determine reorder quantity → order more inventory.

That model does not accurately represent many collectible products.

A collectible release may instead behave like:

> Product announced → distributor solicitation received → store requests quantity → distributor determines allocation → allocation may change → product ships → store receives some or all allocated product → additional waves may or may not become available.

For example:

| Stage | Booster Boxes |
|---|---:|
| Requested | 24 |
| Initial Allocation | 10 |
| Final Allocation | 8 |
| Received | 8 |

The fact that the store originally wanted 24 units is valuable information even though only 8 entered inventory.

Traditional inventory systems primarily care about the final **8 units**.

Loot Allocation should preserve the entire story.

Over time, that history becomes useful operational data.

---

# 3. Product Vision

Create a lightweight operational tool that helps collectible stores answer:

> **What product is coming, what have we asked for, what are we actually getting, and what do we need to do next?**

Long term, the application should help stores build institutional knowledge about distributor relationships and product acquisition that would otherwise be scattered across emails, distributor portals, invoices, spreadsheets, and memory.

---

# 4. Product Principles

## 4.1 Requested does not mean ordered inventory

A requested quantity represents what the store would like to receive.

It must never automatically be treated as incoming inventory.

## 4.2 Allocated does not mean received

An allocation indicates what a distributor currently expects to provide.

Only product physically received should be considered received.

The application must keep these values separate.

## 4.3 Allocations can change

The system must support changes to an allocation without losing the previous value.

Example:

**August 3**  
Requested: 24

**August 15**  
Allocated: 12

**August 22**  
Revised allocation: 8

The application should retain this history rather than simply changing `12` to `8`.

## 4.4 One release may involve multiple distributors

The same product may be requested from several distributors.

Example:

### Booster Box

| Distributor | Requested | Allocated |
|---|---:|---:|
| Distributor A | 24 | 8 |
| Distributor B | 12 | 6 |
| Distributor C | 12 | 0 |

Distributor activity must therefore be modeled separately from the product itself.

## 4.5 Products belong to releases

Stores often make purchasing decisions at the release or product line level rather than looking at every SKU independently.

Example:

### Pokémon: Example Expansion

- Booster Display
- Elite Trainer Box
- Booster Bundle
- Three Pack Blister
- Collection Box

The application should group related products under a release.

## 4.6 Historical data should inform decisions, not make them

The application may eventually surface information such as:

> Average booster box fulfillment from Distributor A across the last six comparable releases: 38%.

However, it should not present this as a guarantee that the next request will receive a 38% allocation.

Historical data provides context.

It does not determine distributor behavior.

## 4.7 Cloud first persistence

Persistent application data must be stored in managed cloud infrastructure.

The production application must not depend on a database running on the developer's local machine.

Local development tools may connect to the managed cloud database through secure configuration.

## 4.8 Cost safety is an architectural requirement

The application should be designed to remain within Azure free service allowances during expected development and early production use.

Where Azure provides a hard free limit or an option to stop service when a free allowance is exhausted, the project must prefer that behavior over paid overage.

Services without a native hard spending limit must use conservative scaling, authentication, monitoring, budget alerts, and automated shutdown protection where practical.

---

# 5. Primary Users

## Store Owner / Buyer

The primary user responsible for:

- Reviewing upcoming releases
- Deciding quantities to request
- Placing distributor orders
- Recording allocations
- Monitoring order deadlines
- Tracking incoming product
- Reviewing distributor history

## Store Employee

A future or secondary user who may:

- View upcoming releases
- Record shipments received
- Review product allocation information

Sensitive actions may eventually be restricted by role.

---

# 6. Goals

The initial product should:

1. Provide one location for tracking upcoming collectible releases.
2. Track distributor solicitation and ordering deadlines.
3. Track requested quantities separately from allocated quantities.
4. Track allocation changes over time.
5. Track actual received quantities.
6. Allow the same product to be tracked across multiple distributors.
7. Clearly show which upcoming items require action.
8. Preserve historical acquisition data.
9. Make distributor history easy to review.
10. Reduce reliance on spreadsheets, email searches, and memory.
11. Persist production data in Azure rather than on a local developer machine.
12. Operate within Azure free tier limits during expected early usage.
13. Avoid accidental paid cloud usage through explicit technical safeguards.

---

# 7. Non Goals

The initial version will **not** attempt to:

- Replace the store's POS
- Manage live sellable inventory
- Synchronize Shopify inventory
- Synchronize TCGplayer inventory
- Automatically price collectibles
- Track individual singles
- Manage buylists
- Process customer payments
- Automatically submit distributor orders
- Scrape distributor portals
- Predict allocations using AI
- Predict collectible prices
- Determine which products the store should purchase
- Provide accounting functionality
- Optimize for large scale multi tenant SaaS usage
- Introduce paid cloud services unless explicitly approved

These capabilities may integrate with Loot Allocation later, but they are outside the initial product boundary.

---

# 8. Core Domain Model

## 8.1 Release

Represents a collectible release or product family.

Example:

**Pokémon TCG: Example Expansion**

Suggested fields:

- Id
- Name
- Game / Product Line
- Manufacturer
- Release Date
- Prerelease Date, optional
- Notes
- Status
- Created Date
- Updated Date

Possible statuses:

- Announced
- Ordering
- Allocation
- Incoming
- Released
- Closed

## 8.2 Product

Represents a sellable product associated with a release.

Examples:

- Booster Display
- Elite Trainer Box
- Booster Bundle
- Three Pack Blister

Suggested fields:

- Id
- Release Id
- Product Name
- SKU, optional
- Manufacturer SKU, optional
- Product Type
- Units Per Case, optional
- MSRP, optional
- Notes

The system should support products whose purchasing unit differs from their eventual selling unit.

For example:

> Requested from distributor: **2 cases**  
> Case size: **6 booster boxes**  
> Equivalent quantity: **12 booster boxes**

Unit conversion should be explicit rather than assumed.

---

# 9. Distributor

Represents a distributor from which products may be requested.

Suggested fields:

- Id
- Name
- Account Notes
- Website, optional
- Active
- General Notes

The application should not assume every distributor follows the same ordering process.

---

# 10. Solicitation

A solicitation represents a distributor offering products for an upcoming release.

A single release may have solicitations from several distributors.

Example:

**Pokémon Example Expansion**

### Distributor A
Order due: September 3

### Distributor B
Order due: September 6

Suggested fields:

- Id
- Release Id
- Distributor Id
- Solicitation Received Date
- Order Deadline
- Expected Allocation Date, optional
- Expected Ship Date, optional
- Notes
- Status

Possible statuses:

- Open
- Submitted
- Partially Allocated
- Allocated
- Shipped
- Received
- Closed
- Cancelled

---

# 11. Product Request

Each solicitation can contain requested products.

Example:

### Distributor A

| Product | Requested |
|---|---:|
| Booster Display | 24 |
| Elite Trainer Box | 20 |
| Booster Bundle | 24 |

Suggested fields:

- Id
- Solicitation Id
- Product Id
- Requested Quantity
- Request Unit
- Submitted Date
- Cost Per Unit, optional
- Notes

---

# 12. Allocation Events

Allocations should be stored as historical events rather than only as a current value.

Example:

### Booster Display — Distributor A

| Date | Allocation |
|---|---:|
| Aug 12 | 12 |
| Aug 18 | 10 |
| Aug 21 | 8 |

Current Allocation:

**8**

Suggested fields:

- Id
- Product Request Id
- Allocated Quantity
- Allocation Unit
- Recorded Date
- Notes

This provides an audit trail and allows future reporting on allocation changes.

---

# 13. Receipts

The system must track what physically arrives separately from the allocation.

A product may arrive across multiple shipments.

Example:

Allocated: **12**

### Shipments

Aug 24 — 8 received  
Aug 27 — 4 received

Total Received: **12**

Suggested fields:

- Id
- Product Request Id
- Quantity Received
- Received Date
- Shipment / Invoice Reference, optional
- Notes

---

# 14. Primary Workflow

## Step 1 — Create Release

User records an upcoming release.

Example:

**Pokémon TCG: Example Expansion**

Release Date: October 17

## Step 2 — Add Products

User adds products associated with the release.

Example:

- Booster Display
- Elite Trainer Box
- Booster Bundle

## Step 3 — Record Distributor Solicitation

User records that a distributor is accepting orders.

Example:

**Distributor A**

Order Deadline: September 4

## Step 4 — Enter Requested Quantities

User records what Loot requested.

Example:

Booster Displays: **24**

ETBs: **20**

Booster Bundles: **24**

## Step 5 — Record Allocation

When the distributor provides quantities:

Booster Displays:

**Requested: 24**

**Allocated: 8**

Allocation fulfillment:

**33%**

## Step 6 — Update Allocation If Necessary

If allocation changes:

Previous: **8**

New: **6**

The history remains available.

## Step 7 — Record Receipt

When shipment arrives:

Allocated: **6**

Received: **6**

The solicitation may now be considered complete.

---

# 15. Dashboard

The dashboard should prioritize **actionable information**, not generic statistics.

## Needs Attention

Example:

### Orders Due

**Tomorrow**  
Pokémon Example Expansion  
Distributor A  
3 products not yet submitted

### Awaiting Allocation

Pokémon Example Expansion  
Distributor B  
Requested August 10

### Allocation Changed

Booster Display  
12 → 8

### Incoming

Distributor A  
Expected August 28  
16 total units

---

# 16. Release Detail Screen

The release page should provide a consolidated view.

Example:

# Pokémon Example Expansion

**Release:** October 17

### Distributor Summary

| Distributor | Requested | Allocated | Received |
|---|---:|---:|---:|
| A | 68 | 30 | 30 |
| B | 40 | 14 | 0 |
| C | 24 | Pending | 0 |

Selecting a distributor exposes individual products.

---

# 17. Distributor Detail Screen

A distributor page should eventually provide historical context.

Example:

# Distributor A

### Recent Requests

| Release | Requested | Allocated | Fulfillment |
|---|---:|---:|---:|
| Set A | 48 | 16 | 33% |
| Set B | 36 | 18 | 50% |
| Set C | 60 | 20 | 33% |

The application may later allow filtering this information by:

- Game
- Product type
- Manufacturer
- Date range

Historical fulfillment percentages should always be labeled as historical information rather than predicted future allocation.

---

# 18. Search and Filtering

Users should be able to locate releases using:

- Release name
- Product name
- Game / product line
- Distributor
- Release date
- Status

Useful filters may include:

- Orders due
- Awaiting allocation
- Allocated
- Incoming
- Released

---

# 19. Notifications and Deadlines

For MVP, notifications should exist **inside the application**.

Examples:

> Order due in 2 days

> Allocation has not been recorded

> Shipment expected this week

External email, text, or push notifications are future enhancements.

---

# 20. Business Rules

### BR 1

Requested quantity must never automatically become allocated quantity.

### BR 2

Allocated quantity must never automatically become received quantity.

### BR 3

Allocation changes must retain historical values.

### BR 4

Multiple receipts may exist for one allocated product.

### BR 5

Multiple distributors may offer the same product.

### BR 6

A product must belong to a release.

### BR 7

A solicitation must belong to a distributor and release.

### BR 8

Quantities must include or inherit a unit of measure.

### BR 9

The system must not silently assume that distributor case sizes are identical.

### BR 10

Historical fulfillment percentages must not be presented as guaranteed future allocations.

### BR 11

Production persistence must use Azure SQL Database rather than a local production database.

### BR 12

No paid Azure service or paid service tier may be introduced without explicit project approval.

### BR 13

Where a free tier supports disabling or pausing service instead of charging overage, that behavior must be enabled.

---

# 21. MVP

The first usable release should contain only the functionality necessary to prove the workflow.

## Release Management

- Create release
- Edit release
- Archive release
- Add products

## Distributor Management

- Create distributor
- Edit distributor

## Solicitation Management

- Add distributor solicitation
- Record order deadline
- Add requested products
- Record requested quantities

## Allocation Management

- Record allocation
- Revise allocation
- View allocation history

## Receiving

- Record received quantity
- Support partial shipments
- Compare allocated versus received

## Dashboard

- Upcoming order deadlines
- Awaiting allocations
- Incoming releases
- Recently changed allocations

## History

- View historical releases
- View distributor request and allocation history

---

# 22. Possible MVP Extension: Internal Product Allocation

After distributor quantities are confirmed, Loot may need to decide how scarce product should be used.

Example:

**8 Booster Displays Allocated**

| Destination | Quantity |
|---|---:|
| In Store | 3 |
| Online | 2 |
| Packs | 2 |
| Event Support | 1 |

The system could verify:

> 8 available  
> 8 assigned  
> 0 unassigned

This feature is intentionally marked as an **MVP candidate rather than a confirmed requirement** until Loot's current workflow is better understood.

---

# 23. Future Features

## Customer Interest

Allow customers or employees to record demand for upcoming products without creating guaranteed preorders.

## Allocation Planning

Assign scarce allocated product between:

- In store
- Online
- Events
- Packs
- Holds
- Other uses

## Distributor Analytics

Analyze:

- Historical fulfillment
- Allocation changes
- Product availability
- Order frequency

## Release Performance

Combine acquisition history with sales information to understand how different releases performed.

## Restock / Wave Tracking

Track product offered after initial release separately from the original solicitation.

## Invoice Attachments

Associate distributor invoices with shipments and solicitations.

## CSV Import

Import product and distributor information from existing spreadsheets.

## External Integrations

Potential future integration with:

- Shopify
- POS system
- Distributor data exports

Integrations should only be built after the core workflow proves valuable.

---

# 24. Technical Architecture

## 24.1 Frontend

**Technology:** React + TypeScript

**Hosting:** Azure Static Web Apps Free plan

The frontend should be deployed as a static web application with HTTPS enabled.

The project should remain on the Free plan unless a future requirement cannot be met without upgrading and that upgrade is explicitly approved.

## 24.2 Backend

**Technology:** ASP.NET Core Web API

**Hosting:** Azure Container Apps using the Consumption model

Initial production scaling configuration:

- Minimum replicas: `0`
- Maximum replicas: `1`

The API should be allowed to scale to zero while idle.

A cold start after inactivity is considered an acceptable tradeoff during the hobby / early production stage.

Scaling limits may only be increased after reviewing the impact on the Azure free allowance.

## 24.3 Data Access

**Technology:** Entity Framework Core

Entity Framework Core will be responsible for:

- Mapping domain entities to Azure SQL
- Querying and persistence
- Schema migrations

Database migrations should be version controlled.

## 24.4 Database

**Technology:** Azure SQL Database

Azure SQL Database is the authoritative persistent datastore for the application.

The database must use the Azure SQL free database offer where available.

At provisioning time, the database must be configured so that reaching the applicable monthly free limit causes the database to pause or become unavailable until the allowance resets rather than automatically continuing into paid usage.

The application must tolerate temporary database unavailability gracefully.

## 24.5 Authentication

The production application must require authentication before access to store operational data.

Anonymous public access to API endpoints containing Loot business data is not permitted.

Authentication technology will be finalized before the first public deployment.

## 24.6 Secrets and Configuration

Secrets must not be committed to source control.

Examples include:

- Database connection strings
- Authentication secrets
- Azure credentials

Local development secrets should use environment variables, .NET user secrets, or another approved secret management mechanism.

Production configuration should use Azure supported secret / environment configuration.

---

# 25. Cloud Cost Safety Requirements

Cost safety is a first class technical requirement.

## 25.1 General Requirement

The project should target **$0 monthly Azure spend** during expected hobby and early production usage.

This is a target, not an assumption.

Azure pricing, quotas, and free service terms must be reviewed when infrastructure is provisioned and before significant architecture changes.

## 25.2 Azure SQL Safety

The Azure SQL free offer must be selected.

The database must be configured to stop or pause when its free monthly allowance is exhausted rather than continue with paid usage.

Storage and compute usage should be reviewed periodically.

## 25.3 Static Web App Safety

The frontend must remain on the Azure Static Web Apps Free plan.

No upgrade to Standard or another paid plan may occur without explicit approval.

## 25.4 Container App Safety

The API must initially use:

```text
minReplicas = 0
maxReplicas = 1
```

This limits automatic scale out and allows the API to consume no running replica resources when idle.

The maximum replica count may not be increased without reviewing expected cost and free tier impact.

## 25.5 Azure Budget

An Azure Cost Management monthly budget must be configured for the Loot Allocation resource scope.

Recommended alert thresholds:

- $1 actual or forecasted spend
- $3 actual or forecasted spend
- $5 actual or forecasted spend

Budget alerts are monitoring controls only.

They must not be treated as a hard spending cap because Azure budgets do not automatically stop resources when a threshold is reached.

## 25.6 Automated Cost Response

Where practical, cost alerts should trigger automated actions capable of disabling or stopping billable resources.

The exact shutdown automation will be determined during the deployment milestone.

The automation must prioritize preventing unexpected charges over keeping a hobby environment continuously available.

## 25.7 Resource Isolation

Loot Allocation Azure resources should be placed in a dedicated resource group.

This provides:

- Easier cost reporting
- Easier cleanup
- Easier policy assignment
- Reduced risk of unrelated Azure resources being confused with project resources

## 25.8 Resource Provisioning Restrictions

The project should use Azure Policy, Infrastructure as Code restrictions, deployment review, or another suitable control to reduce the risk of accidentally creating unapproved expensive resources or service tiers.

## 25.9 No Silent Upgrades

Infrastructure must never automatically change from a free tier to a paid tier as part of application deployment.

Any paid tier change requires explicit approval.

## 25.10 Cost Documentation

The repository should contain a short cloud cost safety document describing:

- Which Azure services are used
- Which free plan / offer is expected
- Important limits
- Scale settings
- Budget configuration
- Shutdown behavior
- How to verify current Azure spending

This document should be updated when infrastructure changes.

---

# 26. Nonfunctional Requirements

## NFR 1 — Data Durability

Production data must persist independently of the developer's local machine.

## NFR 2 — Security

Operational business data must require authenticated access.

## NFR 3 — Auditability

Historical allocation changes must remain reviewable.

## NFR 4 — Reliability

Temporary Azure service suspension caused by free allowance exhaustion must not corrupt data.

## NFR 5 — Cost Containment

Expected hobby / early production usage should remain within Azure free allowances.

## NFR 6 — Fail Closed on Cost

Where a supported Azure configuration allows the project to become unavailable instead of generating paid overage, the project should prefer unavailability.

## NFR 7 — Observability

The application should provide enough logging to diagnose application failures without enabling expensive logging or monitoring services by default.

## NFR 8 — Maintainability

Business rules should be enforced primarily in the application / domain layer rather than relying solely on frontend validation.

---

# 27. Testing Strategy

Business rules around quantities and allocations should receive particularly strong automated test coverage.

## Allocation revision

Given:

Requested: 24  
Previous allocation: 12

When:

Allocation changes to 8

Then:

Current allocation is 8

And:

Historical allocation of 12 remains available.

## Partial receipt

Given:

Allocation: 12

When:

8 units are received

Then:

Received = 8

Remaining expected = 4.

## Multiple receipt

Given:

Allocation: 12  
Previously received: 8

When:

4 additional units arrive

Then:

Total received = 12.

## Multiple distributors

Requests for the same product from different distributors must remain independent.

## Database unavailable

Given:

Azure SQL is temporarily unavailable because a free allowance has been exhausted or the database is paused

When:

The API attempts a database operation

Then:

The application should return an appropriate error without corrupting state or presenting the action as successful.

---

# 28. Initial Success Criteria

The first version should be considered successful if Loot can use it for a real upcoming release and:

1. Track every distributor solicitation for the release.
2. See every ordering deadline in one place.
3. Record what was requested from each distributor.
4. Record what each distributor ultimately allocated.
5. Record what actually arrived.
6. Review the complete acquisition history afterward without referring to another spreadsheet.
7. Access the application and its data without relying on the developer's local computer.
8. Operate during normal usage without generating Azure charges.

The strongest early success metric is not number of features.

It is:

> **Was the application useful enough that Loot chose to use it for the next release too?**

---

# 29. Product Validation Questions

Before implementation begins, the following should be validated against Loot's real workflow.

## Distributor Workflow

1. Which distributors does Loot currently use?
2. How are solicitations received?
3. Where are order deadlines currently tracked?
4. How are orders submitted?
5. When does Loot normally learn its allocation?
6. Can allocations change after initially being communicated?
7. How does Loot learn that an allocation changed?
8. Are later product waves treated as new orders or continuations of the original release?

## Quantities

9. Does Loot typically request cases, individual units, or both?
10. How often do distributor case sizes differ?
11. Does the distributor communicate allocations in cases or individual units?

## Receiving

12. Are shipments commonly split?
13. Can shipped quantity differ from the final communicated allocation?
14. Does Loot currently reconcile distributor invoices against what physically arrives?

## Planning

15. Before allocation is known, does Loot track how much product it hopes to receive?
16. After allocation is known, does Loot decide in advance how much product will go online, in store, to events, or be opened?
17. Would tracking customer interest before release provide useful information when deciding requested quantities?

## Current Pain Points

18. What part of this process currently requires the most manual work?
19. What information is most often difficult to find later?
20. What mistake or forgotten task would this system be most valuable in preventing?

---

# 30. Recommended Development Philosophy

Loot Allocation should not begin as an attempt to create a commercial SaaS platform for every game store.

The first objective is much simpler:

> **Build the smallest product that accurately models Loot Card Shop's real acquisition workflow and makes that workflow meaningfully easier.**

Once that succeeds, the product can be evaluated for broader applicability to other collectible retailers.

Real workflow should drive features.

Not hypothetical functionality.

---

# 31. Current Architecture Decision Summary

The following decisions are now considered accepted for the initial project direction:

| Area | Decision |
|---|---|
| Frontend | React + TypeScript |
| Backend | ASP.NET Core Web API |
| ORM | Entity Framework Core |
| Database | Azure SQL Database |
| Frontend Hosting | Azure Static Web Apps Free |
| API Hosting | Azure Container Apps Consumption |
| API Scaling | Minimum 0 replicas, maximum 1 replica initially |
| Persistence | Managed Azure cloud database |
| Cloud Provider | Microsoft Azure |
| Cost Target | $0 during expected hobby / early production usage |
| Paid Services | Require explicit approval |
| Database Free Limit Behavior | Pause / become unavailable rather than paid overage |
| Budgeting | Azure Cost Management alerts plus automated response where practical |
| Production Data Access | Authenticated only |
