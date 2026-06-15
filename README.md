# marine-agent-skills

**A Claude Code skill pack for contributing to the [Blue Life Commons](https://github.com/frankxai/blue-life-commons) / Ocean Intelligence System.**

These skills let *anyone* — citizen, researcher, NGO, developer — use a coding agent to
produce marine-conservation artifacts that are **schema-valid, sourced, and ethics-checked**,
without having to memorize the commons' rules. They are the contributor-facing half of the
**IS-engine** layer (the serving half is [`marine-mcp`](https://github.com/frankxai/marine-mcp)).

## The pipeline these skills enforce

```
author (/species-page, /field-mission)
   → /source-verify   (every factual claim cited; defeats AI drift)
   → /ethics-check    (welfare, no precise locations, no anthropomorphism)
   → /validate-artifact (the commons' schema gate)
   → /open-artifact-pr  (PR, not direct commit — reviewers decide truth)
```

Two independent locks: the skills *generate* and self-check; the commons' CI validator and
human review *gate*; `marine-mcp` then *serves only what passed*.

## Skills

| Skill | Purpose |
|---|---|
| `/species-page` | Author a gold-standard species page (taxonomy, dated IUCN status, safe-granularity distribution, cited threats) |
| `/field-mission` | Author a citizen-science / travel mission with mandatory welfare + disengagement rules |
| `/ethics-check` | Pre-PR review against the wildlife-ethics policy (blocking) |
| `/source-verify` | Claim-level citation check; flags un-sourced or under-tiered claims |
| `/validate-artifact` | Run the commons' deterministic schema validator |
| `/open-artifact-pr` | Assemble a complete, review-ready PR |

## Install

**As a Claude Code plugin** — drop this repo into your plugins, or copy `skills/` into a
project's `.claude/skills/`. Run the skills from inside a checkout of `blue-life-commons`
(the skills read its `AGENTS.md`, `ETHICS.md`, `SOURCES.md`, and `schema/`).

Pairs with `marine-mcp` configured against the same `BLC_PATH` so `/source-verify` and
`/validate-artifact` can call its `lookup_source` and `validate_artifact` tools.

## Relationship to Starlight Intelligence System

This pack is designed to register as the **`marine-intelligence`** domain sub-stack in SIS
(`skill-rules.json` mirrors that convention). Artifacts carry ambient "Built on SIP · Blue
Life Commons" attestation.

## License

MIT (skills). Generated artifacts are CC-BY-4.0 per the commons.

> Built on SIP · Blue Life Commons — an initiative of Starlight Intelligence Systems.
