---
name: connector-build
description: Use when adding a new data source connector to the ocean-intelligence-system MCP layer. Produces the full connector module (connector.py, tool.py, fixtures, tests, README) following the established pattern. Triggers — "add a connector", "build a connector for <API>", "implement <API name> connector", "wire up <data source>".
---

# Building an Ocean Intelligence connector

You are adding a new data source to the Ocean Intelligence System's MCP layer. Every connector follows the same structure and must pass `python -m pytest mcp/` before the PR opens.

## Before starting

1. Read an existing implemented connector: `mcp/servers/obis/` or `mcp/servers/coral-reef-watch/` — match the pattern exactly.
2. Read `mcp/lib/signals.py` — only emit signal types that exist there (`Occurrence`, `OceanState`, `ConservationStatus`, `FishingEffort`, `ProtectedArea`, `Taxon`).
3. Verify the target API is in `mcp/registry/connectors.yaml` as `status: planned`. If not, add it first.
4. Check API auth requirements against the registry — some APIs need free registration; document this.

## Directory structure to create

```
mcp/servers/<connector-id>/
├── README.md              — data source, auth setup, signal schema, license, test command
├── connector.py           — Fetcher type, build_url(), normalize(), fetch_<signal>()
├── tool.py                — TOOL descriptor dict + handle() function
├── fixtures/
│   └── sample_<signal>.json  — representative API response for offline tests
└── test_connector.py      — TestNormalize, TestBuildUrl, TestFetchInjected (min 5 tests)
```

## connector.py pattern

```python
"""<Source Name> connector for Ocean Intelligence System."""
from __future__ import annotations
import datetime
import json, urllib.request
from typing import Callable
from mcp.lib.signals import OceanState, Occurrence, Provenance  # whichever types you emit

# ── Constants ──────────────────────────────────────────────────────
BASE_URL = "https://api.example.org/v1"
SOURCE_META = {
    "source": "<Source Name>",
    "org": "<Organization>",
    "license": "<SPDX identifier>",
}
USER_AGENT = "ocean-intelligence-system/0.1 (https://github.com/frankxai/ocean-intelligence-system)"

# ── Types ──────────────────────────────────────────────────────────
Fetcher = Callable[[str], dict]

def _http_get(url: str) -> dict:
    req = urllib.request.Request(url, headers={"User-Agent": USER_AGENT})
    with urllib.request.urlopen(req, timeout=30) as r:
        return json.loads(r.read())

# ── URL construction ───────────────────────────────────────────────
def build_url(param1: str, param2: str | None = None) -> str:
    ...  # encode params, return full URL string

# ── Normalization ──────────────────────────────────────────────────
def normalize(raw: dict) -> list[OceanState]:  # or Occurrence[], etc.
    records = []
    for item in raw.get("items", []):
        val = item.get("value")
        if val is None:
            continue
        prov = Provenance(
            dataset_id=item.get("id", "unknown"),
            accessed=datetime.date.today().isoformat(),
            **SOURCE_META,
        )
        records.append(OceanState(
            variable="<variable>",
            value=float(val),
            # ... other fields
            provenance=prov,
        ))
    return records

# ── Public entry point ─────────────────────────────────────────────
def fetch_<signal>(param1: str, *, fetcher: Fetcher = _http_get) -> list[OceanState]:
    url = build_url(param1)
    raw = fetcher(url)
    return normalize(raw)
```

## tool.py pattern

```python
"""MCP tool adapter for <Source Name> connector."""
from .connector import fetch_<signal>

TOOL = {
    "name": "<source_id>_<signal>",
    "description": "<One sentence: what data, from where, for what use>",
    "inputSchema": {
        "type": "object",
        "properties": {
            "param": {"type": "string", "description": "..."},
        },
        "required": ["param"],
    },
}

NOTICE = "<Source Name> data via <org>. License: <SPDX>. Cite the source and dataset in published work."

def handle(params: dict) -> dict:
    records = fetch_<signal>(params["param"])
    return {
        "count": len(records),
        "<signal>": [r.to_dict() for r in records],
        "notice": NOTICE,
    }
```

## test_connector.py pattern

```python
import json, pathlib, unittest
from .connector import normalize, build_url, fetch_<signal>

FIXTURE = json.loads((pathlib.Path(__file__).parent / "fixtures" / "sample_<signal>.json").read_text(encoding="utf-8"))

class TestNormalize(unittest.TestCase):
    def test_record_count(self):
        records = normalize(FIXTURE)
        self.assertGreater(len(records), 0)

    def test_required_fields(self):
        r = normalize(FIXTURE)[0]
        self.assertIsNotNone(r.variable)
        self.assertIsNotNone(r.provenance)

class TestBuildUrl(unittest.TestCase):
    def test_url_contains_param(self):
        url = build_url("test_value")
        self.assertIn("test_value", url)

class TestFetchInjected(unittest.TestCase):
    def test_returns_records(self):
        records = fetch_<signal>("test", fetcher=lambda _: FIXTURE)
        self.assertIsInstance(records, list)
```

## Registry update

After building, update `mcp/registry/connectors.yaml`:
```yaml
- id: <connector-id>
  status: implemented   # was: planned
  signals: [<signal-types>]
  test_count: <N>
```

## Auth documentation (in README.md)

Always document:
- Whether auth is required (free/registration/application)
- Which environment variable to set (`<VARNAME>`)
- Link to registration page
- Behavior when auth is absent (graceful error, not crash)

## Finish

1. Verify `python -m pytest mcp/servers/<connector-id>/ -v` passes all tests.
2. Update `mcp/registry/connectors.yaml` status to `implemented`.
3. Run `/open-artifact-pr` with the connector's README as the artifact.

> Built on SIP · Ocean Intelligence System (MIT).
