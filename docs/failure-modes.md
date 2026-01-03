# Failure Modes & Guardrails

## Field Constraints

Surveys are conducted under real-world constraints:
- no cell service (basements, garages, stairwells)
- rushed site contacts
- locked or inaccessible rooms
- poor lighting and unsafe conditions

The system is designed to fail safely under these conditions.

---

## Common Failure Scenarios

### No Connectivity
- survey remains local
- publish deferred
- no partial uploads occur

### Incomplete Access
- surveyor records partial observations
- notes and photos capture context
- missing areas flagged during review

### Rushed Data Entry
- structured fields reduce ambiguity
- photos provide later clarification
- raw data preserved without forced normalization

### Dirty or Unsafe Areas
- photo evidence captures fixture condition
- reviewer can assess assumptions later

---

## Upload Failures

### Network Failure
- upload aborted
- no server-side mutation
- retry is safe

### Validation Failure
- intake rejects upload
- receipt includes rejection reason
- survey must be corrected and re-published

---

## Review-Time Issues

- conflicting counts
- ambiguous fixture types
- unclear photos

These are handled during **review**, not by mutating raw data.

---

## Design Guardrails

- raw data is immutable post-upload
- derived data may be regenerated
- decisions are versioned
- exports are snapshots

This prevents silent data drift and preserves auditability.
