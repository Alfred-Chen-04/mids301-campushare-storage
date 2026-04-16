---
name: Porter's Five Forces Diagram Spec
description: Visual for Task 2 §2.2 showing 5 forces with intensity and evidence
type: diagram-spec
---

# Porter's Five Forces — Diagram Specification

> This file follows [`../AGENT.md`](../AGENT.md). Last synced: 2026-04-16.
>
> **Purpose**: companion diagram for Task 2 §2.2 → **Figure 3**
> **Source text**: [`../drafts/02_strategy.md:29-47`](../drafts/02_strategy.md#L29-L47) (intensities and evidence must match the prose)
> **Tool**: prefer Mermaid (VS Code Preview renders it directly); fall back to draw.io if style needs to match other diagrams
> **Export**: `porter_five_forces.png` (A4 landscape or 4:3, 300 DPI)
> **Caption**: `Figure 3. Porter's Five Forces Analysis for CampusShare Storage.`

---

## Intensity summary (from draft §2.2)

| Force | Intensity | One-line evidence |
|---|---|---|
| Threat of New Entrants | **Moderate** | Low technical barrier, but the `.edu` trust wall + two-sided network effects + review data create time-based defensibility |
| Bargaining Power of Suppliers (Hosts) | **Low–Moderate** | Fragmented supply; peak-summer leverage; review-reputation soft lock-in |
| Bargaining Power of Buyers (Renters) | **Moderate** | Price-sensitive students with visible substitutes, offset by walkable campus proximity |
| Threat of Substitutes | **High** ⚠ | U-Haul / Public Storage / friend storage / shipping home / university programs |
| Industry Rivalry | **Low–Moderate** | No current `.edu`-gated campus P2P storage competitor; Neighbor.com / SpareFoot misaligned |

---

## Mermaid source (drop into a Markdown preview)

```mermaid
flowchart TB
    E["<b>Threat of New Entrants</b><br/>⚫ Moderate<br/><i>Low tech barrier, but .edu trust wall<br/>+ network effects + review moat</i>"]
    S["<b>Bargaining Power of Suppliers</b><br/>(Hosts)<br/>⚫ Low–Moderate<br/><i>Fragmented supply; peak-summer leverage;<br/>review reputation creates soft lock-in</i>"]
    B["<b>Bargaining Power of Buyers</b><br/>(Renters)<br/>⚫ Moderate<br/><i>Price-sensitive students with substitutes,<br/>offset by walkable proximity</i>"]
    X["<b>Threat of Substitutes</b><br/>🔴 High<br/><i>U-Haul, Public Storage, friend storage,<br/>shipping home, university programs</i>"]
    R(["<b>INDUSTRY RIVALRY</b><br/>⚫ Low–Moderate<br/><i>No current .edu-gated campus P2P<br/>competitor; Neighbor.com misaligned</i>"])

    E -->|pressures| R
    S -->|pressures| R
    B -->|pressures| R
    X -->|pressures| R

    classDef force fill:#F5F5F5,stroke:#333,stroke-width:1.5px,color:#000
    classDef center fill:#FFE4B5,stroke:#8B4513,stroke-width:3px,color:#000
    classDef high fill:#FFCCCC,stroke:#CC0000,stroke-width:2px,color:#000
    class E,S,B force
    class X high
    class R center
```

---

## Layout notes (if drawing in draw.io instead)

```
          ┌─────────────────────────────┐
          │  Threat of New Entrants     │
          │  Moderate                    │
          └──────────────┬──────────────┘
                         │
                         ▼
┌───────────────┐ ┌─────────────┐ ┌────────────────┐
│  Suppliers    │ │   RIVALRY    │ │    Buyers     │
│(Hosts)        │→│ Low–Moderate │←│  (Renters)    │
│Low–Moderate   │ │   (center)   │ │  Moderate     │
└───────────────┘ └──────┬───────┘ └────────────────┘
                         ▲
                         │
          ┌──────────────┴──────────────┐
          │  Threat of Substitutes      │
          │  HIGH  ⚠                    │
          └─────────────────────────────┘
```

- **Center box** = Industry Rivalry, scaled 1.3× with a warm fill (light orange / yellow) and a thicker border for emphasis
- **Four surrounding forces** = north/east/south/west positions; each box holds 3 lines: force name / intensity label / 1-line evidence
- **Substitutes** uses a red warning fill (because intensity is highest)
- **Arrows** point from each force into the central Rivalry (showing pressure direction)
- **Typography**: titles bold 14 pt, evidence regular 10 pt, everything fits on one page

---

## draw.io steps (fallback route)

1. Open https://app.diagrams.net/ → New → Blank Diagram
2. From Shapes > General drag 5 rectangles
3. Rename the center rectangle "Industry Rivalry" and fill it with light orange `#FFE4B5`
4. Arrange the other 4 rectangles around it per the layout above
5. Fill the Substitutes rectangle with light red `#FFCCCC`
6. Draw arrows from the four surrounding forces toward the center
7. Text: 14 pt bold titles, 10 pt italic evidence
8. File → Export as → PNG (Border 20, 300 DPI; uncheck transparent background)
9. Save as `../diagrams/porter_five_forces.png`

---

## Full-marks checklist

- [ ] All 5 forces present (names match Porter 1985 terminology exactly)
- [ ] Every force shows an intensity label matching draft §2.2 (Moderate / Low-Moderate / Moderate / High / Low-Moderate)
- [ ] Each force shows a 1-line key evidence statement (≤ 15 words)
- [ ] Substitutes stands out visually (High-intensity warning styling)
- [ ] Rivalry is centered and visually enlarged
- [ ] Arrows point from each of the 4 forces into central Rivalry
- [ ] Figure 3 caption centered below the diagram
