---
name: species-page
description: Use when authoring or updating a Blue Life Commons species page (cetacean, pinniped, turtle, shark/ray, reef). Produces schema-valid Markdown with sourced claims and honest review status. Triggers — "write a species page", "add a species to the commons", "document <species>".
---

# Authoring a Blue Life Commons species page

You are contributing to a public scientific commons. **Unsourced claims do not ship.**

## Before writing (read these in the target repo)

1. `AGENTS.md` — binding rules for agent contributions.
2. `SOURCES.md` — citation tiers (Tier 1 primary / Tier 2 institutional only for science claims).
3. `ETHICS.md` — no anthropomorphism, no interaction promotion, no precise locations for vulnerable taxa.
4. `schema/artifact-schema.yaml` — required frontmatter fields.

## The gold-standard structure (each factual sentence must trace to a source)

1. **Taxonomy & description** — accepted scientific name + authority; defer ranges to cited sources.
2. **IUCN status** — category **with assessment date and Red List version**. A 2009 "Least Concern" is not current truth; surface the assessment age.
3. **Distribution** — at *safe* granularity. Use regional/coarse description, never exact aggregation sites, nesting beaches, or haul-outs for vulnerable taxa. When in doubt, coarsen.
4. **Behavior / ecology** — every claim cited to Tier 1–2; no "what the animal is thinking" framing.
5. **Threats** — cite IUCN threat classification + regional pressures.
6. **What you can do** — welfare-first; minimum approach distance citing the strictest applicable rule; link iNaturalist / Happywhale for ethical reporting.
7. **Sources** — full list, DOIs where available, access dates.

## Frontmatter

Set `status: needs-expert-review` and `review.science: pending` for any new science page — **you do not decide scientific truth; reviewers do.** Fill `sources[]`, `species`, `species_group`, `outputs.website_path`, `outputs.github_path`, `contributors`, `license: CC-BY-4.0`.

## Sensitivity check (mandatory)

Before finalizing distribution: would a coarse location + a season + a visible landmark in any included media reconstruct an exact site? If yes, coarsen further or omit. Apply the GBIF four-category model where the commons has adopted it.

## Finish

1. Run `/source-verify` over the draft (no un-sourced factual sentence).
2. Run `/ethics-check`.
3. Run `/validate-artifact` (schema gate).
4. Run `/open-artifact-pr`.

> Built on SIP · Blue Life Commons (CC-BY-4.0).
