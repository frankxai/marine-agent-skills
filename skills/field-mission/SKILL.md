---
name: field-mission
description: Use when authoring a Blue Life Commons field mission (citizen-science or travel observation). Enforces the ETHICS.md wildlife-interaction rules. Triggers — "create a field mission", "whale-watching mission", "citizen science protocol".
---

# Authoring a Blue Life Commons field mission

A field mission sends a real human toward real wildlife. **Animal welfare overrides content value.** Read `ETHICS.md` first and treat it as binding.

## Required sections (a mission missing any of these is incomplete)

1. **Objective** — what the participant observes/records, and why it's useful.
2. **Minimum distances & disengagement** — explicit approach distances citing the strictest applicable regulation; a "when to leave" rule.
3. **Signs of disturbance** — for the target species, with sources. The participant must be able to recognize stress and back off.
4. **Seasonal sensitivities** — breeding, pupping, nesting, molting windows to avoid.
5. **Legal & permit requirements** — cited.
6. **Logistics** — meeting point, gear, conditions (regulations cited within still need sources).
7. **"Do no harm" section** — overrides all other instructions in the mission.

## Hard rules (from ETHICS.md)

- **No interaction promotion**: do not encourage touching, feeding, baiting, chasing, surrounding, or swim-with programs for wild animals.
- **No precise sensitive locations**: regional granularity only for vulnerable populations.
- **No anthropomorphism** presented as fact.

## Frontmatter

`type: field-mission`, `status: needs-expert-review`, `review.ethics: required`. Field missions are ethics-sensitive by default — an ethics reviewer can block publication regardless of other approvals.

## Finish

`/ethics-check` → `/validate-artifact` → `/open-artifact-pr`.

> Built on SIP · Blue Life Commons (CC-BY-4.0).
