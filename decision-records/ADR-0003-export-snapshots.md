# ADR-0003: Export Snapshots

## Status
Accepted

## Context

Field-generated PDFs are informational and may not reflect final compliance determinations.

Compliance and proposal artifacts must:
- be reproducible
- reflect a specific survey version
- withstand audit scrutiny

---

## Decision

- Server-generated exports are treated as **snapshots**
- Each export references:
  - survey ID
  - upload receipt
  - generation timestamp
- Snapshots are immutable once generated

---

## Consequences

### Benefits
- audit defensibility
- deterministic reporting
- clear separation of draft vs official artifacts

### Tradeoffs
- regeneration requires a new snapshot
- storage overhead increases
