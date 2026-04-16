# Task 3 — Information Design (ERD)

> This file follows [`../AGENT.md`](../AGENT.md). Last synced: 2026-04-16.
>
> **Word budget**: ~450 words | **Points**: 9 | **Target completion**: 2026-04-17
>
> **Companion diagram**: [`../diagrams/ERD_spec.md`](../diagrams/ERD_spec.md) (drawing spec) → `../diagrams/ERD.png` (exported from draw.io)

## Word allocation

| Section | Target words |
|---|---|
| 3.1 Narrative (describe the ERD) | 300 |
| 3.2 Assumptions & Limitations | 150 |
| **Total** | **450** |

---

## Final English Draft

### 3.1 Narrative

The CampusShare Storage data model is organized around nine entities, reflecting the platform's three functional layers: actor and identity management, core transaction processing, and trust and risk management. The entity-relationship diagram (ERD) is drawn using Chen notation and is included as Figure 1.

At the actor layer, **Student** is the central entity, capturing each user's verified identity (`.edu` email, name, phone, and verification status). A Student belongs to one **University**, which stores the institution's email domain and is used to enforce campus-gated access. Because the same individual can act as both a storage provider (Host) and a storage seeker (Renter) on the platform, these roles are not modeled as separate entities; the Student's context-dependent role is determined by their participation in a given transaction.

The core transaction layer is anchored by **StorageListing** and the **Booking** associative entity. A Student acting as Host creates one or more StorageListings, each describing a specific space's dimensions, location, pricing, and availability window. A Student acting as Renter initiates a Booking, which resolves the many-to-many relationship between Renters and Listings: a single Renter may book multiple Listings over time, and a single Listing may be booked across multiple non-overlapping periods. The Booking entity carries the contractual state of the transaction (status: pending, confirmed, active, completed, or cancelled) and serves as the central hub to which all downstream entities attach. **StoredItem** records the physical belongings associated with a Booking, including photo documentation and declared value. **Payment** records each financial event tied to a Booking, including deposit, periodic rental fees, and any refunds, acknowledging that a single Booking may involve multiple payment transactions.

The trust and risk layer contains three entities dependent on a completed or active Booking. **Review** is the second associative entity, resolving the many-to-many relationship of mutual peer evaluation: after a Booking concludes, both the Renter and the Host may each submit a review rating the other party, creating two Review records per Booking. Review links a reviewer Student and a reviewee Student through the Booking context, ensuring reviews are tied to verified transactions. **Dispute** records any disagreements raised during or after a Booking, capturing the reason, current status, and resolution. **InsurancePolicy** is an optional entity that may be attached to a Booking at the Renter's discretion, recording coverage amount, premium paid, and policy terms.

### 3.2 Assumptions and Limitations

The data model operates under the following assumptions. First, the platform scope is a single university campus; multi-campus or cross-institutional bookings are not modeled. Second, a Student entity represents a single user who may play either role; no separate Host or Renter entity is introduced, as this would introduce unnecessary redundancy given the shared identity foundation. Third, payment processing is delegated to a third-party gateway (e.g., Stripe); the Payment entity captures confirmation records only, not the internal mechanics of payment authorization or fraud detection. Fourth, the model does not represent partial item returns, multi-stage handovers, or promotional discount codes, as these fall outside the minimum viable transaction scope. Fifth, platform administrators and customer service roles are excluded from the ERD, as operational workflows are addressed in the BPMN process model (Task 4). Finally, the Dispute resolution workflow is captured at a high level within the Dispute entity and expanded in the BPMN sub-process; its internal escalation states are not decomposed further in the data model.

---

## Word count check

Current: ~450 / 450
