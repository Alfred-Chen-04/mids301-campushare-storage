# BPMN Specification — Booking & Item Handover Process

> This file follows [`../AGENT.md`](../AGENT.md). Last synced: 2026-04-16.
>
> **Standard**: BPMN 2.0
> **Layout**: Database Interaction Layout (Class 5-1 slide 20) — business pool on top, Database pool spanning the middle of the canvas, optional external-system pool at the bottom; activities drop data associations down to the DB
> **Drawing tool**: draw.io (enable the BPMN shape library)
> **Export**: `BPMN.png` (A4 landscape, 300 DPI, Border 20), embedded in the report for Task 4 as **Figure 2**
> **Caption**: `Figure 2. Booking and Item Handover Process (BPMN 2.0, Database Interaction Layout).`

## Full-marks checklist (tick after finishing in draw.io)

- [x] Activity count = **14** (requirement ≥ 10)
- [x] At least one standalone Database pool/lane → **Pool: Database**
- [x] Sub-process count = **2** (Payment Processing, Dispute Handling)
- [ ] Pool 1 (Platform) contains 3 lanes: Renter / Host / System
- [ ] Pool 2 (Database) contains 6 data-store icons (Student, StorageListing, Booking, StoredItem, Payment, Review)
- [ ] The 14 activities are numbered in order and linked by solid sequence flows
- [ ] XOR gateway (diamond) carries Yes/No branch labels
- [ ] The 2 sub-processes are rounded rectangles marked with `+`
- [ ] Every DB read/write activity has a dashed data-association connector (direction: READ = DB → Activity; WRITE = Activity → DB)
- [ ] Start event (thin circle) and End event (thick circle) are both present
- [ ] The Renter ↔ Host handover coordination uses a message flow (dashed line with open arrowhead)
- [ ] Figure 2 caption is centered beneath the diagram

---

## Pool / lane structure (Database Interaction Layout, stacked vertically)

```
╔════════════════════════════════════════════════════════════════════════╗
║ Pool 1: CampusShare Platform                     (~55% of canvas)     ║
║ ┌──────────────────────────────────────────────────────────────────┐ ║
║ │ Lane: Renter     ●→ [1]→ [2] → [4] → [5] → [8] → [9] → [11] ─┐ │ ║
║ └──────────────────────────────────────────────────────────────┼──┘ ║
║ ┌──────────────────────────────────────────────────────────────┼──┐ ║
║ │ Lane: Host                  [6] →◇[7]───── [11] ←────────────┘  │ ║
║ └──────────────────────────────────────────────────────────────┼──┘ ║
║ ┌──────────────────────────────────────────────────────────────┼──┐ ║
║ │ Lane: System       [3] ←────────────── [10] → [12] → [13] → [14] ●│║
║ └──────────────────────────────────────────────────────────────┘  │ ║
╚════════════════════════════════════════════════════════════════════════╝
                           ↕ (dashed data association)
╔════════════════════════════════════════════════════════════════════════╗
║ Pool 2: Database                                 (~30% of canvas)     ║
║  [Student]   [StorageListing]   [Booking]   [StoredItem]              ║
║                [Payment]                   [Review]                    ║
╚════════════════════════════════════════════════════════════════════════╝
                           ↕ (only the Payment sub-process hits this pool)
╔════════════════════════════════════════════════════════════════════════╗
║ Pool 3: Payment Gateway (external, optional)     (~15% of canvas)     ║
║   ○→ Validate method → Call API → Return response                     ║
╚════════════════════════════════════════════════════════════════════════╝

Legend: ●=Start event   ○=End event   ◇=XOR gateway   [n]=Activity n
       →  = sequence flow (solid)   ↕ = data association (dashed)
       ⇢  = message flow (dashed + open arrowhead, for cross-lane coordination)
```

**Key layout constraints**:
1. The three lanes in Pool 1 run in parallel left→right, advancing the timeline
2. Pool 2 is a single pool (no lanes); all data-store icons sit on one horizontal row
3. Pool 3 is only used when the Payment Processing sub-process is expanded to show the external call; it can be omitted if the sub-process is kept collapsed
4. The connection between `[6]` Host receive notification and `[5]` Renter submit request uses a **message flow** (cross-lane), not a sequence flow
5. `[11]` Arrange handover time spans the Renter and Host lanes, with a bidirectional message flow between them

---

## 14 activities in detail

| # | Lane | Activity Name | BPMN Type | DB Interaction | Notes |
|---|---|---|---|---|---|
| 1 | Renter | Log in & verify .edu email | Task | READ `Student` | First step after the Start event |
| 2 | Renter | Enter search criteria (location, dates, size) | User Task | — | |
| 3 | System | Query available listings | Service Task | READ `StorageListing` | |
| 4 | Renter | Browse and select a listing | User Task | — | |
| 5 | Renter | Submit booking request | User Task | WRITE `Booking` (status=pending) | |
| 6 | Host | Receive notification | Receive Task | — | |
| 7 | Host | Approve or decline | User Task (**Gateway**) | UPDATE `Booking` | XOR gateway: approve → continue; decline → End |
| 8 | Renter | Upload item photos & declare value | User Task | WRITE `StoredItem` | |
| 9 | Renter | Pay deposit + first period | **Sub-process: Payment Processing** | WRITE `Payment` | Encapsulates the Stripe interaction |
| 10 | System | Generate agreement | Service Task | UPDATE `Booking` (status=confirmed) | |
| 11 | Renter + Host | Arrange handover time | User Task (collaboration) | — | Message flow between the two lanes |
| 12 | System | Record handover completion | Service Task | UPDATE `Booking` (status=active) | If contested → **Sub-process: Dispute Handling** |
| 13 | System | End-of-term return confirmation | User Task | UPDATE `Booking` (status=completed) | |
| 14 | System | Prompt mutual review | Service Task | WRITE `Review` | End event follows |

---

## Sub-process details

### Sub-process A: Payment Processing (Activity 9)
**Internal steps** (expand by double-clicking in draw.io):
- Validate payment method
- Call Stripe API
- Handle response (success / failure)
- Write to the `Payment` table

**Why a sub-process**: encapsulates the third-party payment-gateway technical detail, keeping the business semantics of the main flow clear.

### Sub-process B: Dispute Handling (exception branch of Activity 12)
**Internal steps**:
- Capture dispute reason
- Write to the `Dispute` table
- Notify both parties
- Platform mediation
- Resolve or escalate

**Why a sub-process**: the exception path is isolated so it does not pollute the main success path, and the internal workflow has meaningful complexity of its own.

---

## Data-flow arrow rules

- Every DB read/write activity has a **dashed arrow** (BPMN association / data association) connecting to the corresponding entity in Pool 2
- Direction:
  - READ: DB → Activity
  - WRITE / UPDATE: Activity → DB

### 14 activities × 6 DB entities mapping matrix

| Activity # | → Student | → Listing | → Booking | → StoredItem | → Payment | → Review |
|---|---|---|---|---|---|---|
| 1 Log in & verify | R | | | | | |
| 2 Enter criteria | | | | | | |
| 3 Query listings | | R | | | | |
| 4 Browse & select | | | | | | |
| 5 Submit booking | | | W(pending) | | | |
| 6 Notify Host | | | | | | |
| 7 Approve/decline (◇) | | | U | | | |
| 8 Upload items | | | | W | | |
| 9 Pay (sub-process) | | | | | W | |
| 10 Generate agreement | | | U(confirmed) | | | |
| 11 Arrange handover | | | | | | |
| 12 Record handover | | | U(active) | | | |
| 13 Return confirmation | | | U(completed) | | | |
| 14 Prompt review | | | | | | W |

> R = READ | W = WRITE | U = UPDATE (state shown in parentheses)
> Empty cell = no interaction; do not draw an arrow for that pair.

---

## Drawing steps (draw.io)

1. Open https://app.diagrams.net/ → New Diagram
2. Shapes panel → More Shapes → enable "BPMN 2.0"
3. Draw 3 horizontal pools first
4. In Pool 1, add 3 lanes (Renter / Host / System)
5. Place the Task blocks in the order of the 14-activity table; connect them with solid sequence flows
6. Use a diamond (XOR exclusive gateway) for the gateway
7. Use a rounded rectangle with a `+` mark for each sub-process
8. Place data-store ("barrel") icons inside Pool 2
9. Use dashed arrows for data associations
10. Export: File → Export as → PNG (Border 20, 300 DPI, whole diagram)
11. Save as `../diagrams/BPMN.png`

---

## XOR gateway detail (Activity 7)

```
      [6 Notify Host]
           │
           ▼
   [7 Approve or Decline?]
           │
          ◇  XOR Gateway
         ╱ ╲
    Yes ╱   ╲ No
       ▼     ▼
   [8 Upload   [End event:
    items]     Booking cancelled]
                    │
                    ▼
              UPDATE Booking
              status=cancelled
```

- The XOR gateway diamond holds a question mark or "Approved?"
- Both branches must carry a text label ("Yes" / "No" or "Approved" / "Declined")
- The Decline branch terminates at an End event and simultaneously updates `Booking.status=cancelled`; draw a data association for this update to the Booking entity as well.

---

## Open questions

- [ ] Does Class 5-1 Slide 20's "Database Interaction Layout" require the Database pool on the top, the bottom, or the right? The current spec places it **mid-canvas** (a common convention for Database Interaction Layout); if the lecture slide differs, shift Pool 2 vertically in draw.io.
- [ ] Do we need to explicitly show an external Payment Gateway pool? Recommended **yes**, because it directly supports the Task 4 §4.3 statement that Payment Processing "encapsulates the multi-step interaction with a third-party payment gateway."
