# Loot Allocation
## Product Owner Review Brief

**Purpose:** Confirm Loot Card Shop's real distributor and allocation workflow before development begins.

**Product Owner:** Loot Card Shop Owner  
**Status:** Workflow Discovery  
**Working Product Name:** Loot Allocation

---

# 1. What We Are Trying to Build

Loot Allocation is a lightweight tool for managing the part of sealed product purchasing that happens **before inventory becomes normal sellable stock**.

The core problem is that collectible stores cannot treat inventory like conventional retail.

For many products, Loot may:

1. Learn about an upcoming release.
2. Receive solicitations from one or more distributors.
3. Decide how much product it would like to receive.
4. Submit requested quantities.
5. Receive an allocation that may be lower than requested.
6. Have that allocation change before shipment.
7. Receive product in one or more shipments.
8. Potentially receive additional opportunities or later waves.

The app should help Loot keep that entire process in one place.

The goal is not to replace the POS or existing inventory system.

---

# 2. The Core Information We Plan to Track

For each upcoming release, we currently expect to track:

## Release

Example:

**Pokémon TCG: Example Expansion**

- Release date
- Prerelease date, if relevant
- Products included in the release
- General notes

## Products

Examples:

- Booster Displays
- Elite Trainer Boxes
- Booster Bundles
- Collection Boxes

## Distributors

A release may be available from more than one distributor.

For each distributor, we may track:

- When the solicitation was received
- Order deadline
- Products offered
- Requested quantities
- Allocated quantities
- Allocation changes
- Expected shipment information
- Actual received quantities
- Notes

---

# 3. Proposed Workflow

This is the workflow we currently believe best represents the process.

Please review it and identify anything that is missing, incorrect, or happens in a different order.

## Step 1: Upcoming Release Is Identified

A new release is added to the system.

Example:

> Pokémon TCG: Example Expansion  
> Release Date: October 17

Products associated with the release are added.

---

## Step 2: Distributor Solicitation Is Received

When a distributor begins accepting orders, Loot records:

- Distributor
- Order deadline
- Products being offered
- Any relevant notes or restrictions

The same release may have separate solicitations from multiple distributors.

---

## Step 3: Loot Decides What to Request

Loot records how much of each product it asks the distributor for.

Example:

| Product | Requested |
|---|---:|
| Booster Displays | 24 |
| Elite Trainer Boxes | 20 |
| Booster Bundles | 24 |

Requested quantity represents what Loot would like to receive.

It does **not** mean that quantity is guaranteed.

---

## Step 4: Distributor Allocation Is Recorded

When the distributor communicates what Loot will actually receive, the allocation is recorded separately.

Example:

| Product | Requested | Allocated |
|---|---:|---:|
| Booster Displays | 24 | 8 |
| Elite Trainer Boxes | 20 | 12 |
| Booster Bundles | 24 | 0 |

---

## Step 5: Allocation Changes Are Preserved

If an allocation changes, we want to retain the history.

Example:

> Requested: 24  
> Initial Allocation: 12  
> Revised Allocation: 8

We do not want the original allocation to disappear when the new number is entered.

---

## Step 6: Product Is Received

When product physically arrives, Loot records what was actually received.

The system should support partial shipments.

Example:

> Allocated: 12  
> August 24: 8 received  
> August 27: 4 received  
> Total Received: 12

Allocated quantity and received quantity remain separate.

---

## Step 7: Release History Remains Available

After the release is complete, Loot should be able to look back and answer questions such as:

- How much did we request?
- How much were we allocated?
- Did the allocation change?
- How much actually arrived?
- Which distributors supplied the release?
- How did this compare with previous releases?

---

# 4. Proposed Main Dashboard

The home screen should focus on things that require attention rather than generic statistics.

Examples:

## Orders Due

> Pokémon Example Expansion  
> Distributor A  
> Order due tomorrow

## Awaiting Allocation

> Pokémon Example Expansion  
> Distributor B  
> Request submitted August 10

## Allocation Changed

> Booster Display  
> 12 → 8

## Incoming Product

> Distributor A  
> Expected this week

The goal is for the dashboard to answer:

> **What do I need to pay attention to right now?**

---

# 5. What We Are NOT Planning to Build Initially

The first version is not intended to:

- Replace Loot's POS
- Manage normal sellable inventory
- Manage singles
- Automatically price products
- Automatically place distributor orders
- Process customer payments
- Manage accounting
- Predict allocations with AI
- Predict future collectible prices
- Automatically decide what Loot should order
- Replace Shopify or TCGplayer

The first version should stay focused on distributor ordering, allocations, and receiving.

---

# 6. Possible Feature We Need Feedback On

## Internal Product Allocation

After Loot learns how much product it is actually receiving, does the store decide in advance how that product will be used?

Example:

**8 Booster Displays Allocated**

| Use | Quantity |
|---|---:|
| In Store | 3 |
| Online | 2 |
| Open for Packs | 2 |
| Event Support | 1 |

If this is already part of the real workflow, it may make sense to include it.

If it is not useful, we should leave it out of the first version.

---

# 7. Workflow Questions for the Product Owner

These are the main questions we need answered before finalizing the MVP.

## A. How Releases Start

1. How do you normally learn about upcoming products?
2. Do you actively track releases before distributors begin taking orders?
3. Is there one place you currently keep upcoming release information?

## B. Distributor Solicitations

4. Which distributors does Loot currently order from?
5. How does each distributor communicate new ordering opportunities?
   - Email?
   - Distributor website?
   - Sales representative?
   - Something else?
6. Do different distributors handle solicitations differently?
7. Where do you currently keep track of order deadlines?
8. How often is missing an order deadline a real concern?

## C. Requested Quantities

9. When you submit an order, are you typically requesting:
   - Individual units?
   - Cases?
   - Both?
10. Do you ever request more than you realistically expect to receive because you know allocations may be reduced?
11. Do distributors use different case sizes for the same type of product?
12. What information do you use when deciding how much to request?

## D. Allocations

13. How and when do you normally learn your allocation?
14. Do you receive allocations for individual products or for the release as a whole?
15. How often do allocations change after the first number is communicated?
16. How are allocation changes communicated?
17. Is it important to remember previous allocation amounts after they change?
18. Are there situations where a distributor offers additional product later?

## E. Receiving Product

19. Do orders frequently arrive in multiple shipments?
20. Does the amount shipped ever differ from the final allocation you were told?
21. Do you currently compare received product against:
   - The distributor order?
   - The allocation?
   - The invoice?
22. What receiving mistakes are most frustrating or important to prevent?

## F. Later Waves and Restocks

23. When additional product becomes available after release, do you think of that as:
   - A new order?
   - Another wave of the original release?
   - It depends?
24. Would it be useful to preserve the history of these later opportunities?

## G. Internal Planning

25. Once you know your allocation, do you decide how much goes to:
   - In store sales?
   - Online sales?
   - Packs?
   - Events?
   - Holds or preorders?
26. Would tracking those decisions in this tool be useful?

## H. Historical Information

27. What information from old releases do you wish were easier to look up?
28. Would comparing requested versus allocated quantities across releases help future ordering decisions?
29. Would comparing distributors be useful?
30. What historical information would you actually look at?

---

# 8. Most Important Discovery Questions

If we do not answer every question immediately, these are the most important ones to resolve first:

1. **What does the real workflow look like from solicitation to product arriving at Loot?**
2. **Where does Loot currently track this information?**
3. **What parts of that process are annoying, repetitive, or easy to forget?**
4. **What information is difficult to find later?**
5. **How do allocations and allocation changes actually work across Loot's distributors?**
6. **How should later waves or restocks be represented?**
7. **What would make this useful enough that you would actually use it for every release?**

---

# 9. MVP Proposal

If the workflow above is accurate, the first usable version would likely include:

### Releases
- Create and edit upcoming releases
- Add products to a release

### Distributors
- Create and manage distributors

### Solicitations
- Record distributor ordering opportunities
- Record order deadlines

### Requests
- Record requested quantities for each product

### Allocations
- Record allocated quantities
- Record allocation changes
- Preserve allocation history

### Receiving
- Record actual received quantities
- Support partial shipments

### Dashboard
- Show orders that are due
- Show requests awaiting allocation
- Show recent allocation changes
- Show incoming releases

### History
- Review completed releases
- Review requested, allocated, and received quantities

---

# 10. What We Need From the Product Owner

Before development begins, we want the Product Owner to help us confirm three things.

## 1. Is the workflow accurate?

Identify anything that:

- Happens differently
- Is missing
- Is unnecessary
- Needs to happen in a different order

## 2. What problems matter most?

We want to prioritize problems that actually affect Loot today.

Examples:

- Missing solicitation deadlines
- Forgetting what was requested
- Tracking changing allocations
- Reconciling shipments
- Finding historical information
- Comparing distributor behavior

## 3. What would make the MVP genuinely useful?

The first release does not need to do everything.

It needs to solve enough of the real workflow that Loot would choose to use it for the next product release.

---

# 11. Product Owner Signoff Goal

After reviewing this document, we should be able to update the full PRD so that:

- The workflow reflects how Loot actually operates.
- MVP features solve real current problems.
- Unnecessary assumptions are removed.
- Important edge cases are documented.
- The Product Owner agrees that the proposed MVP would be useful in day to day operations.

The goal is not to approve every future feature.

The goal is to confirm:

> **We understand the problem correctly and are building the right first version.**
