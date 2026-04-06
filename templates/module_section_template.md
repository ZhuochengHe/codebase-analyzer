# Module Section Template

Used by `module_agent` for each module in Phase 4.
Fill all fields for core modules; omit less-relevant fields for utilities.

---

```markdown
### Module: [name]

**Purpose**: What problem does this module solve? Why does it exist as a
separate module rather than being folded into something else?

**Design decisions**: What notable choices were made here? Why this approach
over alternatives? Cite PRs or comments if they document the rationale.

**Key technical details**:
- [Algorithm, pattern, or mechanism that's non-obvious]
- [Framework-specific usage that matters]
- [Concurrency model, state management, caching strategy, etc.]

**Quantitative details**:
- [Scale: how many X does this handle?]
- [Performance: latency targets, throughput, queue depths]
- [Counts: endpoints, event types, DB tables, config entries]
- [Thresholds: timeouts, retry limits, batch sizes, TTLs]

**Interfaces**:
- Inputs: [what comes in — from where, in what shape]
- Outputs: [what goes out — to where, in what shape]
- Key APIs: [most important functions/endpoints — names and brief purpose, not full signatures]
- Side effects: [DB writes, external API calls, events emitted, files written]

**Dependencies**:
- Depends on: [other modules this imports or calls]
- Depended on by: [other modules that import or call this]

**Tradeoffs / limitations**:
- [Known constraints or deliberate simplifications]
- [What this module intentionally does NOT handle]
- [Tech debt or areas flagged for future work]
```

---

## Depth Guide

| Module type | Omit these fields |
|-------------|------------------|
| Core business logic | none — fill all |
| Adapters / middleware | Quantitative details, Tradeoffs (unless notable) |
| CLI / scripts | Quantitative details, Dependencies |
| Utility / infra | Keep only Purpose; add one-sentence tech note inline |
