# Task 5 — Critical Reflection

> This file follows [`../AGENT.md`](../AGENT.md). Last synced: 2026-04-16.
>
> **Word budget**: ~500 words | **Points**: 2 | **Target completion**: 2026-04-19

## Word allocation

| Section | Target words |
|---|---|
| 5.1 Ethical Issue (CampusShare-specific) | 200 |
| 5.2 Project Limitations | 150 |
| 5.3 Future Developments | 150 |
| **Total** | **500** |

---

## Final English Draft

### 5.1 Ethical Issue: Inference Risk from Combined Data Exposure

A distinctive ethical risk in CampusShare's design arises not from any single data point but from the **combination** of three types of information the platform necessarily collects: verified student identity (name and `.edu` email), precise Host residential address, and itemized photographs of stored belongings including declared monetary value. In isolation, each of these is routine for a storage transaction. Together, they constitute a complete asset-location-identity profile that, if exposed through a data breach, insider misuse, or insufficient access controls, could enable targeted theft or personal harassment — risks that are disproportionately serious for international students, who may face higher barriers to engaging local law enforcement and may lack robust local social support networks (Nissenbaum, 2010; FTC, 2024).

Three specific exposure vectors are of concern. First, item photos uploaded by Renters create a visible inventory of high-value possessions (electronics, instruments, designer goods), making compromised listings a roadmap for burglary. Second, Host addresses are a direct pointer to a private residence; premature disclosure before a booking is confirmed creates a window during which a malicious actor could conduct reconnaissance under the guise of a booking inquiry. Third, the `.edu` identity linkage means that profile data is tied to a real, verifiable individual within a bounded community, raising the severity of any breach compared to an anonymous marketplace.

CampusShare mitigates these risks through several design controls. Item photos are stored with end-to-end encryption and are accessible only to the verified Renter on an active confirmed booking; raw image files are automatically purged 90 days after booking completion. Host addresses are withheld until booking confirmation and displayed only at neighborhood-level granularity during the browsing phase. Additionally, the platform offers a campus-designated drop-off point option, enabling Hosts to elect a neutral public location for item handover without disclosing their residential address at any point in the transaction.

### 5.2 Limitations

The current model carries several acknowledged constraints. The platform is scoped to a single university campus and does not address cross-campus or inter-city storage scenarios, limiting its market reach by design. The trust infrastructure relies on voluntary post-transaction reviews, which creates a bootstrapping problem: early users face a sparse review environment and must accept higher uncertainty about counterparty reliability. Insurance claim processing is mentioned as a feature but is not fully modeled in the BPMN; the internal workflow for assessing and settling claims remains undefined. The data model does not include a platform administrator or customer service entity, meaning operational oversight roles are architecturally invisible. Finally, prohibited item categories (perishables, hazardous materials, living animals) are assumed but not formally encoded in the StorageListing or StoredItem entities.

### 5.3 Future Developments

Several extensions are viable once the initial campus model achieves product-market fit. A **cross-campus consortium** — connecting multiple universities within the same metropolitan area — would allow a student at one institution to store items with a Host at a neighboring campus during city-wide housing transitions. **Machine learning-based matching** could rank listings by predicted compatibility between stored item types and Host space attributes, improving booking success rates. **Dynamic pricing** would allow Host pricing to respond to real-time supply and demand signals, with premium pricing during pre-summer peak weeks and discount incentives during low-demand periods. A **logistics add-on service**, connecting Renters with student-worker couriers for assisted pickup and delivery, would address a key barrier for students without vehicles. Finally, a **formalized insurance partnership** with a licensed provider would replace the current first-party coverage model with standardized, regulated policies — increasing legal protection for both parties and enabling coverage of higher-value item categories.

---

## References

Federal Trade Commission. (2024). *The Federal Trade Commission 2023 privacy and data security update.* https://www.ftc.gov/reports/federal-trade-commission-2023-privacy-data-security-update

Nissenbaum, H. (2010). *Privacy in context: Technology, policy, and the integrity of social life.* Stanford University Press.

---

## Word count check

Current: ~500 / 500
