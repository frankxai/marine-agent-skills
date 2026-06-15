---
name: open-artifact-pr
description: Use to assemble and open a Blue Life Commons pull request for a finished artifact. Triggers — "open a PR for this artifact", "submit my contribution", after validate/ethics/source checks pass.
---

# Open an artifact PR

The commons accepts **PRs, not direct commits** — every change flows through review. Only open the PR after `/source-verify`, `/ethics-check`, and `/validate-artifact` pass.

## Procedure

1. Branch: `git checkout -b artifact/<type>-<id>`.
2. Place the artifact at its `outputs.github_path` with complete frontmatter.
3. Commit with a descriptive message naming the artifact and the issue it addresses.
4. Push and open the PR. The PR body must state:
   - The **issue** addressed.
   - **Sources used** (and their tiers).
   - Any claims flagged **needs-expert-review**.
   - A filled-in review checklist (`.github/PULL_REQUEST_TEMPLATE.md`).

## PR body template

```markdown
## Artifact
<type>: <title> (`<id>`)

## Issue
Closes #<n>

## Sources
- [Tier 1] <citation + DOI/URL>
- ...

## Review flags
- science: <pending|required|n/a>
- ethics: <pending|required|n/a>
- claims needing expert review: <list or none>

## Checklist
- [ ] Every factual claim sourced (/source-verify passed)
- [ ] Ethics policy honored (/ethics-check passed)
- [ ] Schema valid (/validate-artifact passed)
- [ ] No precise locations for vulnerable taxa
```

Do not weaken, remove, or bypass review requirements, ethics rules, or citation standards in the PR. Reviewers decide scientific truth; you supply sourced, schema-valid drafts.

> Built on SIP · Blue Life Commons.
