---
name: validate-artifact
description: Use to run the Blue Life Commons schema validator over a draft artifact's frontmatter before opening a PR. Triggers — "validate this artifact", "does this pass the schema", before /open-artifact-pr.
---

# Validate artifact (schema gate)

The commons ships a deterministic schema validator. Run it; do not eyeball.

## Procedure

1. Ensure the draft has complete YAML frontmatter (required: `id, type, title, status, sources, review, outputs, contributors, license`).
2. From the repo root, run the project's validator:

   ```bash
   python scripts/validate_artifacts.py content/path/to/your-artifact.md
   ```

   (No args scans all of `content/` and `missions/`.)
3. If marine-mcp is available, you can also self-check frontmatter via its `validate_artifact` tool before touching the filesystem.

## Interpreting results

- `OK <file>` — passes the schema.
- `FAIL <file>` followed by `- <field>: <message>` — fix each before PR. Common causes: missing `review` block, empty `sources`, `id` not matching `^[a-z0-9]+(-[a-z0-9]+)*$`, `species_group` value outside the enum.

The CI workflow `validate-artifacts.yml` runs this on every PR — a local failure is a guaranteed CI failure. Fix locally first.

> Built on SIP · Blue Life Commons.
