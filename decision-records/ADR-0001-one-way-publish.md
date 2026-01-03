# ADR-0001: One-Way Publish vs Bidirectional Sync

## Status
Accepted

## Context

Field surveys are conducted in environments with:
- unreliable or nonexistent connectivity
- limited time on site
- high cognitive and physical load

Bidirectional sync introduces:
- conflict resolution complexity
- unclear data ownership
- brittle offline behavior
- increased operational risk

---

## Decision

The system uses **one-way publish semantics**:
- devices publish completed surveys
- server ingests and becomes system of record
- no server-to-device updates are required

---

## Consequences

### Benefits
- simpler mental model for surveyors
- deterministic ingestion
- easier failure recovery
- lower support burden

### Tradeoffs
- devices do not receive review feedback
- corrections require re-publishing

These tradeoffs are acceptable given operational constraints.
