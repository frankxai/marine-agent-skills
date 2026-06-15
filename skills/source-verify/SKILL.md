---
name: source-verify
description: Use to verify every factual claim in a Blue Life Commons draft is backed by an acceptable source before PR. Defends against AI-generated text drifting from cited fact. Triggers — "verify sources", "check citations", "is every claim sourced".
---

# Source verification (claim-level)

The #1 integrity failure for an agent-authored commons is plausible-but-uncited sentences slipped between the real citations. This skill catches them.

## Procedure

1. Extract every **factual** sentence (a claim about the world: distribution, diet, behavior, population, status, law, statistics). Skip clearly-labeled first-person field observations and pure logistics.
2. For each, identify the supporting source in the artifact's `sources[]` / Sources section.
3. Check the source tier against `SOURCES.md`:
   - **Tier 1** (peer-reviewed, IUCN assessment, government monitoring) — required for core science claims.
   - **Tier 2** (NGO/museum/university outreach) — acceptable with attribution.
   - **Tier 3** (science journalism, documentaries) — context only, never a core claim.
   - **Never**: forums, AI output, uncited blogs, social media.
4. Where the commons uses marine-mcp, call `lookup_source` to confirm a cited URL is real and already known to the corpus.

## Output

`SOURCES: PASS`, or a list of un-sourced / under-tiered claims:

```
- "<sentence>" — no source / Tier-3 used for a core claim → add Tier 1–2 or remove the claim.
```

If any claim cannot be sourced, the rule is: **remove the claim or mark the artifact `status: needs-sources`.** Never leave an unsourced factual sentence in a merged artifact.

> Built on SIP · Blue Life Commons.
