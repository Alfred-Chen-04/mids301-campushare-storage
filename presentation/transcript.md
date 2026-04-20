# Presentation Transcript — CampusShare Storage
# Group 8 | MIDS301 Spring 2026 | 10 minutes

---

## Final Speaker Assignment (based on actual 18-slide PPT)

| Slides | Speaker | Content | Est. Time |
|---|---|---|---|
| 1, 2, 3↷, 4, 5 | **Wendy** | Title + Agenda + Problem + Solution | ~1:30 |
| 6↷, 7 | **Mariama** | Market Opportunity | ~1:00 |
| 8, 9 | **Alfred** | Porter's Five Forces + Strategy & Revenue | ~2:30 |
| 10↷, 11 | **Alex** | ERD walkthrough | ~2:00 |
| 12↷, 13 | **Alfred** | BPMN walkthrough | ~1:30 |
| 14↷, 15, 16, 17, 18 | **Chris (Han Jiang)** | Value Chain + Ethics + Limitations + Closing | ~1:30 |

↷ = section divider slide, flip silently, no speaking needed

**Alfred total: ~4 min** (slides 8-9 and 12-13 — the analytical and technical core)
**Alex total: ~2 min** (slides 10-11 — the ERD he wrote)
**Chris total: ~1:30** (slides 14-18 — 4 content slides, keep each tight)

---

## WENDY — Slides 1–5 (~1:30)
### Title + Agenda + Section 01 Divider + Problem + Solution

[**Slide 1 — Title**]

"Good morning, everyone. We're Group 8, and today we're presenting CampusShare Storage — the campus peer-to-peer storage marketplace. I'm Wendy, and I'll be walking you through our executive summary and the problem we're solving."

[**Slide 2 — Agenda**]

"We have five sections today: the problem and solution, our competitive strategy, information design, business execution, and critical reflection. Let's get into it."

[**Slide 3 — Section 01 divider — flip, no speaking**]

[**Slide 4 — The Problem**]

"Here's the problem. Every year, over 1.1 million international students at U.S. universities face the same crisis — when summer arrives, they have to vacate campus housing with nowhere practical to store their belongings. Commercial self-storage costs over $150 a month, sits five or more miles from campus, and requires a car to access. Most international students have none of that. Meanwhile, their fellow students who stay on campus have closets, rooms, and basements sitting completely empty. The gap isn't storage availability — it's access."

[**Slide 5 — Our Solution**]

"CampusShare closes that gap. It's a peer-to-peer marketplace where Renters — students who need storage — are matched with Hosts — students with unused space. Both sides verify with a .edu email, so you're only dealing with people from your own campus. Terms are flexible — one week to three months. The handover happens on campus, on foot. No truck, no storage unit account required. I'll hand it to Mariama to show you the market."

---

## MARIAMA — Slides 6–7 (~1:00)
### Section 02 Divider + Market Opportunity

[**Slide 6 — Section 02 divider — flip, no speaking**]

[**Slide 7 — Market Opportunity**]

"Thanks Wendy. Let me put a number on the opportunity. Take a mid-sized U.S. university with 5,000 international students. If each stores an average of $300 worth of goods, the gross merchandise value on our platform reaches $1.5 million per campus per year. At our 10% platform commission, that's $150,000 in net revenue from a single campus. We also generate roughly $25,000 from the optional insurance add-on, and another $15,000 from flat transaction fees. Critically, this scales with enrollment — not with infrastructure. Every new campus is a new $150K revenue line. I'll pass it to Alfred for competitive strategy."

---

## ALFRED — Slides 8–9 (~2:30)
### Porter's Five Forces + Strategy & Revenue Model

[**Slide 8 — Porter's Five Forces**]

"Thanks Mariama. To understand where CampusShare competes, I'll walk you through Porter's Five Forces.

**New Entrants — Moderate to High.** Building a service marketplace platform is low-capital. Anyone can copy the interface. But customer acquisition in a two-sided trust market is steep — you have to attract Hosts and Renters simultaneously, and neither side shows up without the other.

**Supplier Power — Moderate.** We rely on software vendors and, as we scale, student-worker labor. No single supplier dominates, but tech costs are rising across the board.

**Buyer Power — High.** Our Renters are price-sensitive students with low switching costs. They could ask a friend, sell their stuff and rebuy, or just use U-Haul. Seasonal demand peaks right when they have the most leverage — right before summer.

**Rivalry — High.** Established REITs and independent storage operators compete aggressively on price and introductory offers in every market we'd enter.

**Substitutes — High.** U-Haul, a friend's apartment, selling furniture and rebuying — all real options. Every one of these requires either significant cost, a vehicle, or informal trust in a stranger.

The answer to all of these forces is the same: we win on what they can't offer.

[**Slide 9 — Strategy & Revenue Model**]

CampusShare pursues **Narrow Differentiation**. We target one specific segment — university students at a single campus — and compete on three things: community trust through the .edu identity wall, convenience through on-campus proximity and one-week minimum terms, and verified peer accountability. We are not trying to out-price U-Haul. We can't — they have decades of scale. We're building something U-Haul has no reason to build: a trusted peer network inside a single campus.

Revenue is per-transaction, three streams. **Primary: 10% platform commission** on each completed booking — $150,000 per campus at full penetration. **Insurance add-on: $5 to $15 per booking** — optional at checkout, roughly $25,000 per campus. **Flat transaction fee** adds another $15,000. No subscriptions — that conflicts with our 'save money' pitch. No advertising — our early user base is too small to monetize attention. I'll hand it to Alex for the information design."

---

## ALEX — Slides 10–11 (~2:00)
### Section 03 Divider + Entity-Relationship Diagram

[**Slide 10 — Section 03 divider — flip, no speaking**]

[**Slide 11 — ERD**]

"Thanks Alfred. Our entity-relationship diagram has nine entities organized in three functional layers, drawn in Chen notation.

**Layer one — Actor & Identity.** Student and University. Every user on the platform is a single Student entity, belonging to one University. That University membership is what enforces the .edu access gate. Importantly, we did not split Host and Renter into separate entities — the same person can act as either role depending on the transaction. Roles are context-determined, not identity-determined.

**Layer two — Core Transactions.** StorageListing, Booking, StoredItem, and Payment. A Host creates StorageListings — each describing a specific space's dimensions, price, and availability. A Renter initiates a Booking — and Booking is our **first associative entity**: it resolves the many-to-many relationship between Renters and Listings. One Renter can book many Listings over time, and one Listing can have multiple non-overlapping bookings. Booking is the central hub that all downstream entities attach to. StoredItem records item photos and declared value. Payment records each financial transaction.

**Layer three — Trust & Risk.** Review, Dispute, and InsurancePolicy. Review is the **second associative entity**: after a booking closes, both the Renter and the Host each submit a review of the other party — two Review records per Booking, both tied to that verified transaction. Dispute captures any disagreements. InsurancePolicy is optional, attached at checkout.

Nine entities. Two associative entities. Booking is the hub. I'll hand it back to Alfred for business execution."

---

## ALFRED — Slides 12–13 (~1:30)
### Section 04 Divider + BPMN

[**Slide 12 — Section 04 divider — flip, no speaking**]

[**Slide 13 — Booking & Handover Process (BPMN)**]

"Thanks Alex. The BPMN models our primary revenue-generating process — the full booking and item handover lifecycle — in BPMN 2.0 using a Database Interaction Layout. Three pools: the CampusShare Platform with Renter, Host, and System lanes; a standalone Database pool; and a Payment Gateway pool. Fourteen activities, two sub-processes, four stages.

**Stage 1 — Search and Selection.** Renter logs in, .edu credentials are verified against the Student table, searches listings, submits a Booking request — written to the database as 'pending.'

**Stage 2 — Confirmation.** The Host receives a notification and approves or declines at an exclusive gateway. On approval, the Renter uploads item photos with a declared value, which writes to the StoredItem table, then pays through the **Payment sub-process** — this encapsulates the entire Stripe API interaction behind a single collapsed box. System generates a digital storage agreement, Booking status updates to 'confirmed.'

**Stage 3 — Handover.** Both parties coordinate a campus meetup through the messaging interface. Physical transfer happens. System records handover completion and updates status to 'active.'

**Stage 4 — Closeout.** End-of-term return is confirmed, Booking status goes to 'completed,' and the system prompts both parties for mutual reviews — written to the Review table.

Two sub-processes: Payment Processing encapsulates the gateway, and **Dispute Handling** isolates the exception path so the main success flow stays clean. I'll hand it to Chris for critical reflection."

---

## CHRIS (HAN JIANG) — Slides 14–18 (~1:30)
### Section 05 Divider + Value Chain + Ethics + Limitations + Closing

[**Slide 14 — Section 05 divider — flip, no speaking**]

[**Slide 15 — Value Chain — Primary Activities**]

"Thanks Alfred. Quickly on the value chain — every primary activity is scoped to the campus boundary. **Inbound Logistics**: host onboarding and .edu verification. **Operations**: search ranking, booking state management, escrow, agreement generation. **Outbound Logistics**: on-campus item handover with timestamped confirmation. **Marketing**: student organizations, dorm boards, international student office channels. **Service**: dispute mediation, dual reviews, insurance support. Everything operates within the campus, which is what keeps cost structure proportional to enrollment — not geography.

[**Slide 16 — Ethics: Combined Data Exposure Risk**]

On ethics. The risk isn't any single data point — it's the combination. Verified student identity, plus a Host's residential address, plus itemized photos of stored belongings. Together, that's a 'who has what, where' profile. If exposed, it enables targeted theft — and that risk is disproportionate for international students who may face language barriers with law enforcement. Our three mitigations: item photos are encrypted and auto-purged 90 days after booking. Addresses are shown only at neighborhood level during browsing, and released only after confirmation. Hosts can elect a campus-designated drop-off point so their address is never disclosed at all.

[**Slide 17 — Limitations & Future Developments**]

Acknowledged limitations: single-campus scope by design, cold-start review problem for early users, insurance claim workflow not fully modeled in the BPMN, no admin entity in the ERD, and prohibited item categories assumed but not formally encoded. Looking ahead: cross-campus networks, ML-based matching, dynamic pricing, student courier add-on, and a licensed insurance partnership.

[**Slide 18 — Closing**]

CampusShare's moat is three things: a .edu identity wall that only verified students can cross, a dual-direction review system where trust compounds with every transaction, and deposit-escrow that aligns incentives for both parties. One point five million dollars in GMV addressable per campus, at zero marginal infrastructure cost. First-mover advantage builds with every review written. One campus. One community. One trusted marketplace. Thank you — we're happy to take questions."

---

## Q&A Prep — Top 5 Likely Questions

| Question | Quick answer |
|---|---|
| "How do you handle disputes about missing or damaged items?" | Dispute entity in ERD; Dispute sub-process in BPMN; photo documentation at handover; InsurancePolicy covers declared item value. |
| "Why 10% commission and not higher?" | Conservative take rate to drive early adoption. Airbnb charges hosts 3% + guests 14-16% — our combined rate is well below that ceiling. |
| "How do you solve the cold-start problem — no reviews at launch?" | Partner with international student offices; offer first-booking incentives; launch timing matches the pre-summer peak when demand is highest. |
| "What stops U-Haul or Airbnb from copying this?" | .edu gating requires campus community relationships they don't have; review history is campus-specific and non-transferable; their models aren't built for this transaction type. |
| "What if a Host cancels last minute on a Renter?" | Cancellation policy is encoded in the Booking status lifecycle; escrow holds deposit until handover confirmed; Renter refunded if Host cancels. |

---

## Notes on Slide Order Quirk

The **Value Chain slide (slide 15) appears after the Section 05 divider** in the PPT — it is technically placed inside Chris's section, not Alfred's. The transcript above reflects this: Chris opens with a quick value chain summary, then moves into ethics. This works because the value chain gives context for the operational scope before discussing ethical risks. Alfred wraps cleanly after the BPMN.
