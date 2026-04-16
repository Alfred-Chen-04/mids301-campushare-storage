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

**Symbol key** (used in both views below):

| Symbol | Meaning |
|---|---|
| `[n]` | Activity number n (full name in the 14-activity table) |
| `●` | Start Event (thin circle) or End Event (thick circle) |
| `◇` | XOR Exclusive Gateway (diamond with × inside) |
| `⟦ ⟧` | Sub-process (collapsed rounded rectangle with `+`) |
| `→` | Sequence flow — solid arrow, used **within** Pool 1 (same pool) |
| `⇢` | Message flow — dashed line + open arrowhead, used for **async notifications** (e.g. Renter notifies Host) |
| `⇄` | Bidirectional message flow (two message flows drawn in opposite directions) |

> **Cross-lane sequence flows are normal**: when a sequence flow crosses a lane boundary inside Pool 1 (e.g. [2] Renter → [3] System), it is still a **sequence flow** (solid arrow). Only flows between different pools use a message flow (dashed).

---

### View 1 — Swimlane table (spatial reference for draw.io)

Use this to know which activities to place in which lane and roughly where on the canvas.

```
Lane    | Activities (left → right = timeline)
--------|-----------------------------------------------------------
Renter  | [1] → [2] → [4] → [5] → [8] → [9] → [11]
        |                    ↓ notify              ↕ msg flow
Host    |                   [6] → [7◇] ─Yes─→ [11]
        |                          └─ No → (End)
System  |       [3]                [10] → [12] → [13] → [14] → ●
```

Pool / canvas size guide:
- **Pool 1 (Platform, 3 lanes)** — ~55% of canvas height
- **Pool 2 (Database, no lanes)** — ~30% of canvas height, directly below Pool 1
- **Pool 3 (Payment Gateway, optional)** — ~15% of canvas height, at the bottom

Pool 2 data-store icons (barrel shape), one row:
`[Student]   [StorageListing]   [Booking]   [StoredItem]   [Payment]   [Review]`

Notes:
- `[3]` is triggered by `[2]` — draw a sequence flow from Renter lane into System lane
- `[10]` follows the Yes branch of `[7]` — sequence flow from Host lane into System lane
- `[11]` sits at the Renter/Host lane boundary — draw a bidirectional message flow ⇄ between the two lane icons
- `[12]` has an exception branch into Sub-process: Dispute Handling

---

### View 2 — Sequential flow (logic reference)

Use this to understand the execution order, gateway branches, and where message flows occur.

```
● Start
[1]  Renter  : Log in & verify .edu              → READ  Student
[2]  Renter  : Enter search criteria
[3]  System  : Query available listings          → READ  StorageListing
[4]  Renter  : Browse and select a listing
[5]  Renter  : Submit booking request            → WRITE Booking (pending)
                 ⇢ message flow → Host notified
[6]  Host    : Receive notification
[7]  Host    : Approve or Decline?  ◇ XOR        → UPDATE Booking
     ├─ Yes → [8]
     └─ No  → End event  (UPDATE Booking: cancelled)
[8]  Renter  : Upload item photos & declare value → WRITE StoredItem
[9]  Renter  : ⟦Sub-process: Payment Processing⟧  → WRITE Payment
[10] System  : Generate booking agreement         → UPDATE Booking (confirmed)
[11] Renter + Host : Arrange handover time        ⇄ message flow (bidirectional)
[12] System  : Record handover completion         → UPDATE Booking (active)
               └─ if contested → ⟦Sub-process: Dispute Handling⟧
[13] System  : End-of-term return confirmation    → UPDATE Booking (completed)
[14] System  : Prompt mutual review               → WRITE Review
● End

Legend:
  ●   = Start / End event (thin circle = start, thick circle = end)
  ◇   = XOR exclusive gateway
  ⟦⟧  = Sub-process symbol (rounded rectangle with + marker)
  →   = sequence flow (solid arrow, same pool)
  ⇢   = message flow (dashed line + open arrowhead, cross-lane or cross-pool)
  ⇄   = bidirectional message flow
```

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

## Shape Quick Reference

Before drawing, find these shapes in the BPMN 2.0 panel in draw.io. Use the panel's search box if needed.

| BPMN Element | What it looks like | draw.io search term |
|---|---|---|
| Start Event | Thin-border circle | `Start Event` |
| End Event | Thick/bold-border circle | `End Event` |
| User Task | Rounded rectangle + person icon (top-left) | `User Task` |
| Service Task | Rounded rectangle + gear icon (top-left) | `Service Task` |
| Receive Task | Rounded rectangle + envelope icon (top-left) | `Receive Task` |
| Sub-Process (collapsed) | Rounded rectangle + **`+`** at bottom center | `Sub Process` |
| XOR Exclusive Gateway | Diamond with **`×`** inside | `Exclusive Gateway` |
| Data Store | Barrel / cylinder shape | `Data Store` |
| Pool | Wide rectangle with label strip on left | `Pool` |
| Sequence Flow | Solid arrow with filled arrowhead | default connector inside a pool |
| Message Flow | Dashed arrow with **open** (hollow) arrowhead | `Message Flow` |
| Data Association | Dashed line with no filled arrowhead | right-click any connector → Edit Style → `dashed=1;endArrow=open;` |

---

## Step-by-step drawing guide (draw.io)

### A. Setup

1. Open https://app.diagrams.net/ → **Create New Diagram** → choose **Blank**
2. Enable BPMN shapes: click **`+ More Shapes`** at the bottom of the left panel → tick **"BPMN 2.0"** → click **OK**. BPMN shapes now appear in the left panel.
3. Optional: File → Page Setup → set Orientation to **Landscape (A4)**

### B. Create the three pools (top-to-bottom order)

4. Drag a **Pool** shape onto the canvas. Label it: `CampusShare Platform`
   - This is Pool 1. Resize it to fill roughly **55% of the canvas height**.
5. Add 3 lanes inside Pool 1: **right-click the pool** → **Add Lane** (do this twice to get 3 lanes). Rename the lanes top-to-bottom: **Renter**, **Host**, **System**
6. Drag a second **Pool** shape directly below Pool 1. Label it: `Database`
   - This is Pool 2. Resize to ~**30% canvas height**. Do **not** add lanes.
7. Drag a third **Pool** shape below Pool 2. Label it: `Payment Gateway`
   - This is Pool 3. Resize to ~**15% canvas height**. Do **not** add lanes.

> **Fixed layout (top → bottom):** Platform → Database → Payment Gateway. Do not put Database in the middle.

### C. Place start and end events

8. In the **Renter lane**, drag a **Start Event** (thin circle) to the **far left**.
9. In the **System lane**, drag an **End Event** (thick circle) to the **far right**.
10. In the **Host lane**, drag a second **End Event** to the right side of that lane — this is where the "No" branch of the gateway terminates (Booking cancelled).

### D. Place the 14 activity shapes

Refer to the **Swimlane Table (View 1)** for which lane each activity belongs to. Work left-to-right in each lane. Use the **Shape Quick Reference** above to pick the correct shape for each activity's BPMN Type (from the 14-activity table).

Special cases requiring extra attention:

- **[7] Approve or Decline**: Place a **User Task** box in the Host lane, then place an **Exclusive Gateway** (XOR `◇`) immediately to its right, also in the Host lane. Draw a sequence flow from [7 task] → [◇ gateway].
- **[9] Payment Processing**: Use a **Sub-Process** shape. The `+` marker is automatic when you use the Sub-Process shape from the BPMN panel.
- **[11] Arrange handover time**: Place this **User Task** in the **Renter lane** only. The Host's participation will be shown via a message flow in Step F — do not put a second box in the Host lane.
- **[12] Record handover + Dispute Handling exception**: Place [12] as a **Service Task** in the System lane. Then drag a **Sub-Process** shape to the right of [12] in the System lane and label it `Dispute Handling`. You will connect these in Step E.

### E. Draw sequence flows (solid arrows — within Pool 1)

Hover over the source shape until blue connection dots appear, then drag from a dot to the target shape. Draw.io creates a solid sequence flow by default within a pool.

Draw flows in this order:

```
[Start] → [1] → [2] → [3]
[3] → [4] → [5]
[6] → [7 task] → [◇ gateway]
  ◇ "Yes" → [8] → [9] → [10] → [11] → [12] → [13] → [14] → [End]
  ◇ "No"  → [End event in Host lane]
[12] → [Dispute Handling sub-process]   (label this arrow: "if contested")
[Dispute Handling sub-process] → [13]
```

> **Note**: [5] → [6] is drawn as a **message flow** in Step F, not a sequence flow.

Cross-lane sequence flows (the arrow visually crosses the lane boundary — this is correct BPMN):
- [2] (Renter) → [3] (System): arrow goes downward across the Renter/System boundary
- [◇ Yes] (Host lane) → [8] (Renter lane): arrow goes upward
- [10] (System) → [11] (Renter): arrow goes upward
- [11] (Renter) → [12] (System): arrow goes downward

After drawing, click each branch arrow from the gateway and **type a label**: `Yes` on the Yes branch, `No` on the No branch.

### F. Draw message flows (dashed lines with open arrowhead — cross-lane async notifications)

In draw.io, drag a **"Message Flow"** shape from the BPMN panel, or draw any connector and set its style to `dashed=1;endArrow=open;startArrow=none;`.

Draw these two message flows:

1. **[5] → [6]**: From the [5] Submit booking task (Renter lane) to the [6] Receive notification task (Host lane). This crosses the lane boundary — that is intentional.
2. **[11] ⇄ Host lane**: From the [11] Arrange handover task (Renter lane), draw a message flow to a small **Intermediate Event** shape placed in the Host lane at the same horizontal position. Label the intermediate event `Confirm handover time`. Then draw a second message flow back from that intermediate event to [11]. This creates the bidirectional coordination.

### G. Add data stores in Pool 2

11. Drag 6 **Data Store** (barrel) shapes into Pool 2 (Database pool). Space them evenly in a single row.
12. Label each: `Student`, `StorageListing`, `Booking`, `StoredItem`, `Payment`, `Review`

### H. Draw data associations (dashed lines — activities to DB)

Use the **DB mapping matrix** (in the "Data-flow arrow rules" section) to know which activities connect to which data store.

To draw a data association in draw.io: draw any connector between an activity and a data store, then right-click → **Edit Style** → change to `dashed=1;endArrow=open;startArrow=none;`.

Arrow direction:
- **READ** (R in matrix): arrow points **from the data store to the activity**
- **WRITE / UPDATE** (W / U in matrix): arrow points **from the activity to the data store**

Activities with **no DB interaction** (all empty in matrix — do not draw any arrow): [2], [4], [6], [11]

### I. Export

13. File → **Export as** → **PNG**
    - **Border width**: 20
    - **Scale**: 100% (sufficient for 300 DPI equivalent at A4)
    - Enable: **"Whole Diagram"** (exports the full canvas, not just the visible area)
    - Background: white
14. Save as `BPMN.png` in the `diagrams/` folder of this project

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

## Decisions already made (no action needed)

| Decision | Choice | Reason |
|---|---|---|
| Database pool position | **Below** the Platform pool | Standard Database Interaction Layout convention: business process on top, data layer below |
| Payment Gateway pool | **Include it** (Pool 3, at the bottom) | Directly supports the Task 4 §4.3 claim that Payment Processing encapsulates a third-party gateway call |
| [11] cross-lane placement | Activity box in **Renter lane** only; Host participation via message flow | Avoids duplicate boxes; message flow correctly represents asynchronous coordination |
| [12] exception branch | Sequence flow labeled **"if contested"** → Dispute Handling sub-process → [13] | Isolates the exception path without a separate gateway, keeping the main flow clean |
