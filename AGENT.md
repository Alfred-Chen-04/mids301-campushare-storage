# AGENT.md — CampusShare Storage Project Constitution

> **Any AI agent (Claude Code / ChatGPT / others) opening this project MUST read this file in full before writing, modifying, or modeling anything.**
> **This file is the project's single source of truth. If any section draft, diagram, or slide conflicts with this file, this file wins. If an update is needed, change this file first and then propagate downstream.**

---

## 1. Project one-liner

**CampusShare Storage** is a **campus-internal P2P short-term storage marketplace** gated by university `.edu` identity verification. It connects students who need temporary storage while off campus in the summer (Renters) with students who have unused space (Hosts), and monetizes through a platform commission.

- **Course**: MIDS301 Introduction to Information — A Systems and Design Approach (Spring 2026)
- **Assignment weight**: 40 points
- **Business model**: peer-to-peer marketplace / sharing economy
- **Core strategy**: **Focused Differentiation**
- **Core competitive moat**: `.edu` identity trust + walkable ultra-short distance + dual-direction review system

---

## 2. Key timeline

| Date | Milestone |
|---|---|
| 2026-04-15 (today) | Workspace set up |
| 2026-04-16 | Task 2 first draft + ERD spec |
| 2026-04-17 | ERD finalized + Task 3 |
| 2026-04-18 | BPMN finalized + Task 4 + value chain table |
| 2026-04-19 | Task 1 + Task 5 + presentation outline |
| 2026-04-20 | Merge into final report + rehearsal |
| **2026-04-21 or 04-23** | **In-class presentation (10 min)** |
| 2026-04-24 – 04-26 | Report polish |
| **2026-04-27 23:59** | **Final report submitted** |

---

## 3. Target customers and use cases

### Primary customer: Renter (demand side)
- **Primary**: international students leaving campus for 2–3 months over summer (return home or summer internship); dorm belongings have nowhere to go
- **Secondary**: students moving between dorms mid-semester, short-term internships abroad, or travel

### Primary customer: Host (supply side)
- **Primary**: enrolled students with unused space at home/apartment/dorm (closets, storage rooms, garages)
- **Secondary**: graduate apartments, shared rentals with empty rooms

### Shared characteristics
- Both sides are verified students at the **same university** (`.edu` email)
- **Trust is anchored in university identity**, not stranger-to-stranger

---

## 4. Core assumptions (not to be overturned casually)

1. **Single-university scope**: business modeling covers one university only; cross-campus is out of scope
2. **Dual-role Student**: a single Student entity can act as both Host and Renter — **do not split into two entities**
3. **Third-party payment is a black box**: payments go through Stripe/PayPal; internal fraud/risk is not modeled
4. **`.edu` email = identity**: other credentials (student ID, campus card) are not modeled
5. **Primarily short-term rentals**: 1 day – 3 months; annual leases are not modeled
6. **No item-category restrictions**: no special modeling for specific categories (perishables, prohibited items) — this can be surfaced as a limitation in Task 5
7. **Platform does not physically hold items**: only facilitates the transaction; Hosts keep custody

---

## 5. Terminology table (all documents must stay consistent)

| Term | Meaning | Forbidden alternatives |
|---|---|---|
| **Student** | Platform user; can simultaneously be Host and Renter | ~~User / Customer~~ |
| **Host** | A Student acting as a storage provider | ~~Provider / Lender / Owner~~ |
| **Renter** | A Student acting as a storage seeker | ~~Customer / Buyer / Guest~~ |
| **StorageListing** | A space published by a Host | ~~Space / Unit / Room / Spot~~ |
| **Booking** | A reservation initiated by a Renter (associative entity) | ~~Order / Reservation~~ |
| **StoredItem** | A record of a stored item | ~~Item / Thing / Package~~ |
| **Review** | Peer review between the two parties (associative entity) | ~~Rating / Feedback (you may use "feedback" in narrative only)~~ |
| **Dispute** | A record of a dispute | ~~Complaint / Issue~~ |
| **Payment** | A payment record | ~~Transaction / Charge~~ |
| **University** | The university entity, used for email-domain validation | ~~School / Campus~~ |
| **InsurancePolicy** | Optional insurance add-on | ~~Coverage / Protection~~ |

---

## 6. Word budget (total 3000 words ±10%, hard cap 3300)

| Task | Recommended words | Running total |
|---|---|---|
| Task 1 Executive Summary | 300 | 300 |
| Task 2 Strategy & Value Proposition | 1300 | 1600 |
| Task 3 Information Design | 450 | 2050 |
| Task 4 Business Execution | 450 | 2500 |
| Task 5 Critical Reflection | 500 | 3000 |
| **Total** | **3000** | |

**Words do NOT include**: references, appendices, Title Page, or table contents (but narrative paragraphs do count).

---

## 7. Full-marks hard requirements (by task)

### Task 2 (9 pts) — Porter + Strategy
- [ ] Cover **all five** forces (Entrants / Suppliers / Buyers / Substitutes / Rivalry)
- [ ] Each force **names a specific competitor or data point** (U-Haul, Public Storage, Neighbor.com, etc.)
- [ ] Explicitly state **Focused Differentiation** and explain why Cost Leadership / Broad Differentiation were rejected
- [ ] Revenue model: **10% platform commission + optional insurance add-on**, with reasoning for rejecting subscription / advertising

### Task 3 (9 pts) — ERD
- [ ] **Chen notation** (rectangle = entity, diamond = relationship, oval = attribute, double oval = multi-valued attribute, underline = primary key)
- [ ] **≥ 7 entities** (this project = 9)
- [ ] **≥ 1 associative entity** (this project = Booking + Review = 2)
- [ ] All M:N relationships resolved into associative entities
- [ ] All cardinalities (1:1, 1:N, M:N) annotated
- [ ] Narrative + assumptions section present

### Task 4 (9 pts) — BPMN
- [ ] Value Chain Primary Activities **table** (Inbound / Operations / Outbound / Marketing & Sales / Service)
- [ ] BPMN diagram uses the **Database Interaction Layout** (Class 5-1 slide 20)
- [ ] **Standalone Database pool/lane**
- [ ] **≥ 10 activities** (this project = 14)
- [ ] Every data-touching activity has a `data flow` arrow to the DB
- [ ] At least 1 sub-process symbol + explanation (this project = Dispute Handling + Payment Processing = 2)
- [ ] Narrative + assumptions section present

### Task 5 (2 pts) — Reflection
- [ ] Ethical issue is **specific to CampusShare** (inference risk from item photos + Host address), not a generic "privacy" issue
- [ ] Concrete **mitigation** described
- [ ] **Limitations** and **future work** sections present

### Formatting (across the whole report)
- [ ] Title Page includes business name + all group members
- [ ] Direct quotations use `"straight double quotes"` (not Chinese 「」 or single quotes)
- [ ] APA citation format
- [ ] Unencrypted `.doc` or `.pdf`

---

## 8. Language rule (enforced)

**All project files must be written in English** — including drafts, diagram specs, checklists, comments, table headers, and any other content written into the workspace files. This applies regardless of the language used in the chat conversation with an AI assistant (which may be mixed Chinese/English). If a file contains non-English text, it must be translated before the next working session.

---

## 9. Prohibited topics (scope-creep guard)

- ❌ Do **not** introduce ML/recommendation algorithm details in the main body (a one-liner in Task 5 future work is fine)
- ❌ Do **not** expand the internal logic of the payment gateway in the BPMN (encapsulate as a sub-process)
- ❌ Do **not** model cross-campus or inter-city business (excluded by assumption)
- ❌ Do **not** split Host and Renter into two separate Student entities
- ❌ Do **not** add an "administrator / platform operations" entity in the ERD (drifts from core business)
- ❌ Do **not** write the strategy as "we're both cheap and premium" (violates Porter's stuck-in-the-middle warning)
- ❌ Do **not** exceed 3300 words (hard grading threshold)
- ❌ Do **not** cite unverifiable numbers or fabricated citations — every statistic must trace to a real source with the original number intact (see citation rules below)

---

## 9b. Citation integrity rules (enforced)

> **Every time you add a new statistic, market number, percentage, or externally sourced conclusion to any draft file, immediately verify it with `/verify-citations`, or confirm the source exists and the figure is correct before writing.**

Specific rules:
1. **Every number needs a source** — no "approximately X billion" from memory; it must be traceable to a specific report or article
2. **Paywalled database numbers (IBISWorld, Statista) may be cited only after a public secondary source has confirmed them**
3. **Do not fabricate DOIs, URLs, or page numbers**
4. **Citation format**: APA 7 (see the template in `references/references.md`)
5. **Update `references/references.md` immediately after citing**
6. **Verification skill**: running `/verify-citations [filename]` scans and corrects citation issues in a draft file

---

## 10. Information still pending from the user (will be asked during execution)

- [ ] **Group member names** — for the Title Page and speaking assignments
- [ ] **Target university** (if any) — whether to anchor on one university for launch (otherwise use "a mid-sized US university")
- [ ] **Screenshot / description of Class 5-1 Slide 20** — the exact form of the Database Interaction Layout
- [ ] **Assigned peer-feedback target group** — assigned on presentation day
- [ ] **Citation-format details from the instructor** (e.g., APA 6 vs APA 7)

---

## 11. Workspace file map

```
/Users/alfred/Desktop/MIDS 301 Final Project/
├── MIDS301 S26 Group Assignment (1).docx   original assignment (do not edit)
├── AGENT.md                                  ⭐ this file
├── README.md                                 workspace overview
├── drafts/
│   ├── 01_executive_summary.md
│   ├── 02_strategy.md
│   ├── 03_information_design.md
│   ├── 04_business_execution.md
│   └── 05_critical_reflection.md
├── diagrams/
│   ├── ERD_spec.md
│   ├── BPMN_spec.md
│   └── value_chain_table.md
├── presentation/
│   └── slides_outline.md
├── peer_feedback/
│   └── investor_feedback_template.md
├── references/
│   └── references.md
└── final/
    ├── CampusShare_Storage_Report.docx   (merged final report, not yet generated)
    └── CampusShare_Storage_Slides.pptx    (final slide deck, not yet generated)
```

---

## 12. Iteration rules

- This file is the **first** thing updated after any major decision (strategy change, customer change, entity change, etc.)
- Add one line to the Changelog at the end on every update
- Every Task draft file must open with `> This file follows AGENT.md. Last synced: YYYY-MM-DD`

---

## Changelog

- **2026-04-15**: initial draft. Confirmed business direction (CampusShare Storage), strategy (Focused Differentiation), 9-entity ERD, 14-activity BPMN, and 3000-word allocation.
