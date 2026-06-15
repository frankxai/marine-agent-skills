---
name: ethics-check
description: Use to review a Blue Life Commons draft against the wildlife-ethics policy before opening a PR. Triggers — "ethics check this", "is this safe to publish", before any artifact involving animal interaction.
---

# Ethics check (pre-PR gate)

Run this over any draft touching living animals. You are emulating the ethics-review brief; flag, don't rationalize. Any FAIL blocks the PR.

## Checklist

- [ ] **No anthropomorphic claims as fact** — no "the whale is saying / feels / wants" presented as truth. Behavioral interpretation must be cited to research.
- [ ] **No interaction promotion** — no encouragement to touch, feed, bait, chase, surround, or swim-with wild animals (describing a permitted research program is OK; promoting it is not).
- [ ] **Minimum approach distance present** where any observation is described, citing the strictest applicable rule.
- [ ] **No precise locations for vulnerable taxa** — no exact haul-outs, nesting beaches, dens, or aggregation coordinates. Check the *combination attack*: coarse location + season + a landmark in attached media must not reconstruct a site.
- [ ] **No invented conservation claims** — population/threat/legal statements are cited to recognized authorities (IUCN etc.).
- [ ] **Disturbance & disengagement guidance** present for field missions.
- [ ] **Legal compliance cited** as the floor, exceeded where welfare science suggests.

## Output

Return one of: `ETHICS: PASS`, or `ETHICS: FAIL — <numbered reasons>`. For science-sensitive uncertainty, recommend setting `review.ethics: required` and labeling the artifact `needs-expert-review`. You flag risks; a human ethics reviewer makes the final call.

> Built on SIP · Blue Life Commons.
