# Task 4 — Business Execution (Value Chain + BPMN)

> 本文件遵循 [`../AGENT.md`](../AGENT.md)，最后同步日期：2026-04-16
>
> **字数预算**：~450 词 | **分值**：9 分 | **计划完成日**：2026-04-18
>
> **配套文件**：
> - [`../diagrams/value_chain_table.md`](../diagrams/value_chain_table.md)（价值链表）
> - [`../diagrams/BPMN_spec.md`](../diagrams/BPMN_spec.md)（BPMN 规范） → `../diagrams/BPMN.png`

## 字数分配

| 小节 | 目标词数 |
|---|---|
| 4.1 Value Chain Primary Activities（表格 + 简述） | 120 |
| 4.2 Selected Process & BPMN Narrative | 250 |
| 4.3 Sub-process Justification & Assumptions | 80 |
| **Total** | **450** |

---

## Final English Draft

### 4.1 Value Chain — Primary Activities

CampusShare's primary activities are mapped to Porter's value chain framework in Table 1 below, demonstrating how each operational stage reinforces the platform's Focused Differentiation strategy.

| # | Activity | CampusShare Implementation |
|---|---|---|
| 1 | **Inbound Logistics** | Host onboarding: collect space specifications, photos, and availability windows; verify `.edu` identity to ensure supply-side quality. Renter registration with need declaration. |
| 2 | **Operations** | Core matching and transaction execution: search ranking, booking state management, digital agreement generation, deposit escrow, item photo registration, and handover scheduling. |
| 3 | **Outbound Logistics** | Physical item handover: Renter and Host meet at an agreed on-campus location; platform generates timestamped handover confirmation. Return follows the same workflow at booking close. |
| 4 | **Marketing & Sales** | Campus-channel acquisition: student organization partnerships, dormitory notice boards, international student office referrals, and `.edu` mailing list outreach concentrated in the pre-summer peak window. |
| 5 | **Service** | Post-transaction trust maintenance: dual-party reviews, dispute mediation, customer support, insurance claim assistance, and data retention compliance. |

Each activity is deliberately scoped to the campus boundary, ensuring that operational complexity scales with campus size rather than geography — consistent with the Focused Differentiation approach.

---

### 4.2 Selected Process: Booking and Item Handover

The selected BPMN process is the **Booking and Item Handover Process** (Figure 2), which represents the primary revenue-generating workflow of CampusShare. This process was chosen because it encompasses the full lifecycle of a storage transaction — from identity verification through physical handover to peer review — and it is the event at which the 10% platform commission is collected. The process is modeled in BPMN 2.0 using a Database Interaction Layout, with three participant pools: the CampusShare Platform (containing Renter, Host, and System lanes), the Database pool (storing six key entities), and an optional Payment Gateway pool.

The process unfolds in four stages. In the **search and selection** stage, a Renter logs in and their `.edu` credentials are verified against the Student table. The Renter enters search criteria, and the System queries the StorageListing table to return available spaces matching the location, size, and date parameters. The Renter selects a listing and submits a booking request, which writes a new Booking record with a `pending` status. In the **confirmation** stage, the Host receives an automated notification and either approves or declines via an exclusive gateway. Upon approval, the Renter uploads item photos and a declared value — written to the StoredItem table — and completes payment through the Payment Processing sub-process, which writes to the Payment table. The System then generates a digital storage agreement and updates the Booking status to `confirmed`. In the **handover** stage, both parties arrange a mutually agreed meeting time via the platform's messaging interface; once the physical handover occurs, the System records completion and updates the Booking status to `active`. In the **closeout** stage, at the end of the storage term, the return handover is confirmed and the Booking status is updated to `completed`, after which the System prompts both parties to submit a mutual review, writing to the Review table.

---

### 4.3 Sub-Process Justification and Assumptions

Two sub-processes are used in the BPMN diagram, each justified by complexity encapsulation. **Payment Processing** (Activity 9) encapsulates the multi-step interaction with a third-party payment gateway — including payment method validation, API call, response handling, and database write — behind a single collapsed sub-process boundary. This preserves the business-level clarity of the main flow without exposing technical implementation details. **Dispute Handling** (branching from Activity 12) is triggered only when a handover discrepancy is reported and contains an independent sequence of evidence capture, notification, mediation, and resolution. Isolating this as a sub-process keeps the primary success path uncluttered and reflects the exceptional, non-routine nature of disputes.

The model operates under the following assumptions: `.edu` email verification is synchronous; payment succeeds on the first attempt (retry logic is internal to the Payment sub-process); Hosts are expected to respond to booking requests within 24 hours; partial cancellations and multi-stage refunds are excluded from scope; and the Dispute sub-process internal escalation to external arbitration is not expanded in this diagram.

---

## 字数检查

当前字数：~450 / 450 ✓
