---
name: guardian-spawn
description: Use when creating a new Ocean Guardian instance for the ocean-intelligence-system. Produces a schema-valid Guardian YAML manifest with correct cadence, ethics, connectors, and activation checklist. Triggers — "spawn a guardian", "create a guardian for <region/species>", "new guardian instance", "add a guardian watching <topic>".
---

# Spawning an Ocean Guardian instance

You are adding a new persistent intelligence agent to the Ocean Intelligence System. A Guardian is **not creative writing** — it is an operational specification that will drive live signal collection and briefing generation.

## Before starting (read in ocean-intelligence-system)

1. `guardians/schema/guardian-schema.yaml` — the JSON Schema every instance must pass.
2. `guardians/README.md` — archetype list and usage contract.
3. `ETHICS.md` — Guardian ethics requirements (sensitive populations, location precision, grounded-or-silent).
4. `mcp/registry/connectors.yaml` — verify your connectors exist and are `status: implemented`.

## Choosing an archetype

| Archetype | Use when |
|-----------|----------|
| `reef` | Monitoring coral reef health, bleaching, thermal stress |
| `species` | Tracking a population or pod (ethics critical — see below) |
| `fishery` | Fishing effort, MPA overlap, high-seas monitoring |
| `coastal` | Coastal communities, tides, water quality, storm surge |
| `basin` | Ocean-basin-scale thermal or circulation anomalies |

## Required frontmatter structure

```yaml
id: <kebab-case-unique-id>
name: "<Human Readable Name>"
version: "0.1"
archetype: reef|species|fishery|coastal|basin
status: draft            # always draft until you run the activation checklist
license: MIT

scope:
  region: "<Geographic scope>"
  species: []             # only for species archetype
  bbox: [lon_min, lat_min, lon_max, lat_max]   # WGS84 decimal degrees

cadence:
  mode: scheduled
  schedule: "0 6 * * *"  # cron string — required when mode is scheduled

# For event-driven guardians, use this pattern instead:
# cadence:
#   mode: event-driven
#   triggers:
#     - source: obis
#       event: new_occurrence

connectors:
  - id: coral-reef-watch
    signals: [ocean-state]
  - id: obis
    signals: [occurrence]

outputs:
  - type: briefing
    audience: ngo
  - type: alert
    audience: researcher

ethics:
  location_precision: regional  # never exact for vulnerable populations
  sensitive_population: false   # set true for e.g. Southern Resident orca
  grounded_or_silent: true      # MANDATORY — never generates ungrounded claims
  welfare_gate: true
```

## Cadence rules

- `mode: scheduled` → `schedule:` must be a **single cron string** (e.g. `"0 6 * * *"`). Do NOT use a list of objects.
- `mode: event-driven` → `triggers:` is a YAML list. No `schedule:` key.
- A guardian can have multiple cadence blocks if it has both a scheduled heartbeat and event triggers — express them as two separate `cadence` entries or use the `mode: hybrid` pattern (see schema).

## Ethics rules (non-negotiable)

- **`grounded_or_silent: true` is mandatory.** A guardian that generates claims not traceable to a BLC commons artifact or a live connector output must stay silent.
- **Sensitive populations**: any Guardian watching a population with < 1,000 individuals must set `location_precision: coarsened` and `sensitive_population: true`.
- **No precise locations** for aggregation sites, breeding grounds, or nesting beaches in any output.

## Connector wiring rules

Every connector in `connectors:` must exist in `mcp/registry/connectors.yaml` with `status: implemented`. If a connector you need is only `planned`, either implement it first (use `/connector-build`) or mark it in the guardian's `# TODO` section.

## Activation checklist (in the file, as YAML comments)

```yaml
# ACTIVATION CHECKLIST (delete each line when done):
# [ ] All referenced BLC artifacts are published (status: approved or published)
# [ ] All connectors in connectors: are status: implemented in registry
# [ ] Ethics has been reviewed (location_precision correct, sensitive_population flag)
# [ ] Guardian has been test-run via: python guardians/runtime.py --instance <id> --dry-run
# [ ] status changed from draft → active
```

## Finish

1. Save to `guardians/instances/<id>.md`.
2. Run `/validate-artifact` pointing at the guardian file.
3. Run `/ethics-check` on the outputs section.
4. Run `/open-artifact-pr`.

> Built on SIP · Ocean Intelligence System (MIT).
