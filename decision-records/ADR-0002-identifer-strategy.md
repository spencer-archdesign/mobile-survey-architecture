# ADR-0002: Identifier Strategy for Offline Surveys

## Status
Accepted

## Context

Surveys are created offline and must be safely ingested without duplication.

Identifiers must:
- exist before upload
- survive retries
- link photos reliably
- support deduplication

---

## Decision

- Each Survey, Measure, and Photo is assigned a **local UUID** at creation
- Upload Packages include all UUIDs
- Server stores and indexes UUIDs
- Duplicate uploads are detected via UUID matching

---

## Consequences

### Benefits
- idempotent uploads
- safe retries
- consistent photo linkage
- minimal merge logic

### Tradeoffs
- UUIDs must be treated as first-class keys
- legacy records may require backfill
