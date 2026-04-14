# Career Compass — Addon Index

This file is the routing table. The seed prompt fetches this at session start. When a session reaches a point that needs depth, fetch the relevant addon URL below.

All addons live in the same repo under `addons/`. Base URL:
`https://raw.githubusercontent.com/giannimassi/career-compass/main/`

## When to fetch what

| Situation | Fetch |
|---|---|
| Entering Values & constraints or Direction setting module | `addons/modules-exploring.md` |
| Entering Direction setting or Positioning (stage: Orienting) | `addons/modules-orienting.md` |
| Entering Positioning, Role sourcing, or LinkedIn audit (stage: Targeting) | `addons/modules-targeting.md` |
| Entering CV review, Application strategy, Interview prep, or Negotiation prep (stage: Executing) | `addons/modules-executing.md` |
| Stuck overlay activated — diagnosing funnel zone | `addons/stuck-diagnostic.md` |
| Calibrating register or applying psychological inference rules when user language/behavior is ambiguous | `addons/psych-inference.md` |
| User writes in Italian OR targets Italian employers OR mentions Italian job market | `addons/cultural-italian.md` |
| User asks how to use AI in their own search, OR asks to "generate 50 cover letters" / mass-apply | `addons/ai-tool-playbook.md` |

## Fetch rules

- Fetch **once per session per addon** — cache the content in working memory, don't re-fetch
- Fetch **at the moment you need it**, not upfront — the session may never reach some modules
- If the fetch fails (no web, URL error), continue using the seed prompt's core flow. The core is self-sufficient. Note in OPEN THREADS: "extended [module] guidance not loaded"
- Addons expand the seed, they do not override it — the seed's hard rules always win

## Integrity

Addons are versioned in git. If an addon's content seems incoherent with the seed prompt, trust the seed prompt and note the discrepancy.
