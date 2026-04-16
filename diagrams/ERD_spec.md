# ERD Specification — Chen Notation

> This file follows [`../AGENT.md`](../AGENT.md). Last synced: 2026-04-16.
>
> **Drawing tool**: draw.io (https://app.diagrams.net/)
> **Notation**: Chen notation — rectangle = entity, diamond = relationship, oval = attribute, double oval = multi-valued attribute, underline = primary key
> **Export**: `ERD.png` (A4 landscape, 300 DPI, Border 20), embedded in the report for Task 3 as **Figure 1**
> **Caption**: `Figure 1. Entity-Relationship Diagram for CampusShare Storage (Chen notation).`

## Full-marks checklist (tick after finishing in draw.io)

- [x] Entity count = **9** (requirement ≥ 7)
- [x] Associative entities = **2** (Booking, Review; requirement ≥ 1)
- [x] All M:N relationships resolved
- [ ] The 9 entity rectangle labels match the §5 terminology table in AGENT.md exactly
- [ ] Every connector shows cardinality (1 / N / 0..1)
- [ ] All 11 relationship diamonds present
- [ ] Booking and Review are drawn as associative entities (diamond enclosing a rectangle)
- [ ] Every attribute shown as an oval
- [ ] Primary-key attributes underlined
- [ ] `StoredItem.photo_url` drawn as a double oval (multi-valued attribute)
- [ ] `Student.rating_avg` drawn as a dashed oval (derived attribute)
- [ ] Figure 1 caption centered below the diagram

---

## Entity list (9)

### 1. Student
- **student_id** (PK)
- edu_email
- name
- phone
- rating_avg (derived attribute — draw with a dashed oval)
- verified_flag
- created_at

### 2. University
- **univ_id** (PK)
- name
- country
- address
- email_domain (used for `.edu` verification, e.g., "berkeley.edu")

### 3. StorageListing
- **listing_id** (PK)
- host_id (FK → Student)
- title
- address
- size_cuft (cubic feet)
- price_per_day
- available_from
- available_to
- description

### 4. Booking — associative entity (connects Student-as-Renter × StorageListing)
- **booking_id** (PK)
- renter_id (FK → Student)
- listing_id (FK → StorageListing)
- start_date
- end_date
- total_price
- status (pending / confirmed / active / completed / cancelled)
- created_at

### 5. Payment
- **payment_id** (PK)
- booking_id (FK)
- amount
- type (deposit / first_period / final / refund)
- method
- status
- timestamp

### 6. StoredItem
- **item_id** (PK)
- booking_id (FK)
- description
- photo_url (multi-valued attribute → double oval)
- declared_value

### 7. Review — associative entity (connects reviewer Student × reviewee Student via Booking)
- **review_id** (PK)
- booking_id (FK)
- reviewer_id (FK → Student)
- reviewee_id (FK → Student)
- rating (1–5)
- comment
- created_at

### 8. Dispute
- **dispute_id** (PK)
- booking_id (FK)
- raised_by (FK → Student)
- reason
- resolution
- status (open / resolved / escalated)

### 9. InsurancePolicy
- **policy_id** (PK)
- booking_id (FK)
- coverage_amount
- premium
- terms
- status

---

## Relationship list (Chen diamonds)

| Relationship diamond | Connects | Cardinality | Notes |
|---|---|---|---|
| **belongs to** | Student — University | N:1 | Each Student belongs to one University |
| **creates** | Student (Host) — StorageListing | 1:N | A Host can publish multiple listings |
| **initiates** | Student (Renter) — Booking | 1:N | A Renter can initiate multiple Bookings |
| **booked for** | Booking — StorageListing | N:1 | A Booking targets one Listing |
| **contains** | Booking — StoredItem | 1:N | A Booking can hold multiple items |
| **settles** | Booking — Payment | 1:N | A Booking may involve multiple payments |
| **generates** | Booking — Review | 1:2 (optional) | Both parties may each post one review |
| **reviewer** | Student — Review | 1:N | A Student authors multiple Reviews |
| **reviewee** | Student — Review | 1:N | A Student is evaluated in multiple Reviews |
| **raises** | Booking — Dispute | 1:0..N | A Booking may have 0+ disputes |
| **covered by** | Booking — InsurancePolicy | 1:0..1 | Optional insurance |

---

## How to draw it (draw.io steps)

1. Open https://app.diagrams.net/
2. Create a Blank Diagram
3. Left Shape panel → More Shapes → enable "Entity Relation (Chen)"
4. Following the table above, draw the entities (rectangles) first, then the relationships (diamonds)
5. Attach attributes (ovals) to entities
6. For the associative entities Booking and Review, write the relationship name inside the diamond and treat the diamond itself like an entity (optionally label "associative entity")
7. Annotate cardinalities on the connectors ("1" or "N")
8. When finished: File → Export as → PNG (Border 20 px, 300 DPI)
9. Save as `../diagrams/ERD.png`

## Layout suggestion (A4 landscape 3508×2480 px @ 300 DPI; coordinates are percent of canvas)

```
        [0%]                [33%]                [66%]              [100%]
  ┌──────────────┐                                         ┌──────────────┐
  │  University  │◆ belongs to                             │    Review    │ ⭐
  │   (10,20)    │═══════════════╗                         │   (80,20)    │
  └──────────────┘               ║                         └──────▲───────┘
                                 ▼                    ┌───────────┘ reviewer
                          ┌──────────────┐            │     ┌───────┘ reviewee
                          │   Student    │════════════╪═════╛
                          │   (40,25)    │
                          │  (hub)       │
                          └──────▲───────┘
             creates ◆───────────┤        ◆initiates (as Renter)
                     │    (as Host)│
                     ▼            ▼
              ┌─────────────┐  ┌─────────────┐  ◆ booked for
              │StorageListing│◆═══│   Booking   │═══════════════╗
              │   (20,55)   │    │   (50,55)   │ ⭐ (assoc.)    ║
              └─────────────┘    └─────┬──┬──┬─┘                ║
                                       │  │  │                  ║
                         ◆ contains ───┘  │  └─── ◆ settles     ║
                                          │                     ║
                             ┌────────────┴────┬───────────┐    ║
                             ▼                 ▼           ▼    ▼
                      ┌─────────────┐  ┌────────────┐  ┌──────────────┐
                      │ StoredItem  │  │  Payment   │  │   Dispute    │
                      │   (30,85)   │  │  (50,85)   │  │   (70,85)    │
                      └─────────────┘  └────────────┘  └──────────────┘
                                                       ◆ covered by ▼
                                                       ┌──────────────┐
                                                       │InsurancePolicy│
                                                       │   (85,85)    │
                                                       └──────────────┘

Legend: ■ = entity (rectangle)  ◆ = relationship (diamond)  ⭐ = associative entity (diamond enclosing a rectangle)
        (x,y) = position on canvas as a percentage
```

### How to draw associative entities

Booking and Review are **associative entities**. The standard Chen representation is:
**outer diamond enclosing an inner rectangle** (a diamond "wrapping" a rectangle).

```
┌─────────────────────┐
│ ╱╲       Booking    │╲     <- outer diamond (the relationship)
│╱  ╲    booking_id   │ ╲
│╲  ╱    renter_id    │ ╱    <- inner rectangle (the entity carrying attributes)
│ ╲╱     listing_id   │╱
│        status       │
└─────────────────────┘
```

draw.io steps: drag a diamond (relationship symbol), drag a rectangle and shrink it inside the diamond, then group them. Alternatively, use the preset "Weak Entity / Associative Entity" shape from the ER shape library.

### Cardinality-annotation rules

- Place the label at the **midpoint** of the connector, closer to the corresponding entity
- Use consistent digits/symbols: `1` / `N` / `M` / `0..1` / `0..N` (do not mix `*` and `N`)
- Suggested font size 12 pt, gray or black
- Cross-check every relationship against the "Cardinality" column above
