# verify-citations

You are a citation integrity checker for the CampusShare Storage MIDS301 project.

When invoked, follow these steps:

## Step 1 — Scan for unverified claims

Read the file(s) specified in the argument (or all `drafts/*.md` if no argument is given).
Identify every sentence that contains:
- A statistic or numerical claim (market size, percentages, enrollment numbers, revenue figures, etc.)
- A citation in parentheses, e.g. `(Author, Year)`
- Any named external company, platform, or report used as evidence

## Step 2 — Classify each claim

For each identified claim, classify it as one of:
- **VERIFIED** — you have high confidence the source is real and the number is accurate
- **PLAUSIBLE** — the source is real but you cannot confirm the exact figure without a web search
- **UNVERIFIED** — the source name is vague, fabricated, or the number cannot be attributed to a real publication
- **MISSING** — a factual claim with no citation at all

## Step 3 — Web-search anything PLAUSIBLE or UNVERIFIED

Use `WebSearch` to find the actual source. Confirm:
1. The source (journal, report, organization) actually exists
2. The specific number or claim appears in that source
3. The publication year matches

If the real figure differs from what is written, note the correct figure.

## Step 4 — Report findings

Output a table:

| Claim | Citation used | Status | Correct source / fix |
|---|---|---|---|
| ... | ... | VERIFIED / PLAUSIBLE / UNVERIFIED / MISSING | ... |

## Step 5 — Apply fixes

For every claim that is not VERIFIED:
- If a real source was found: update the sentence and citation in the draft file, then update `references/references.md`
- If no real source exists: either remove the claim, soften it to "reportedly" with a note, or replace it with a verifiable alternative
- Never leave a fabricated or unverifiable citation in any draft

## Rules (always enforce)

- Do NOT invent DOIs, URLs, or page numbers
- Do NOT cite paywalled databases (IBISWorld, Statista) for a specific number unless that number has been independently confirmed by a public secondary source
- APA 7 format in `references/references.md`
- After any fix, re-check the surrounding sentences for consistency
