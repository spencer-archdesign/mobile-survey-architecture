# ADR-0004: Raw vs Derived Data Separation

## Status
Accepted

## Context

Survey data is collected in difficult field conditions:
- incomplete access to spaces
- poor lighting or visibility
- time pressure from site contacts
- safety and environmental constraints

As a result:
- field observations may be partial or imprecise
- terminology may vary between surveyors
- follow-up interpretation is often required during compliance review

Historically, survey data was:
- manually cleaned during transcription, or
- edited directly during compliance analysis

This made it difficult to:
- understand what was originally observed
- reproduce prior determinations
- audit decision-making after the fact
- re-evaluate surveys when rules or assumptions changed

---

## Decision

The system explicitly separates **raw survey data** from **derived and interpreted data**.

### Raw Data
- exactly what the surveyor entered in the field
- original quantities, locations, notes, and photos
- device timestamps and metadata
- treated as immutable once uploaded

### Derived Data
- normalized fixture types
- standardized space classifications
- aggregated counts and calculated values
- may be regenerated as rules or mappings evolve

### Compliance Decisions
- reviewer interpretations
- assumptions and exceptions
- references to supporting raw evidence (photos, notes)

Raw data is never overwritten. Derived data and decisions may change, but always reference their source inputs.

---

## Consequences

### Benefits
- preserves ground truth from the field
- supports audit and dispute resolution
- allows re-analysis under updated codes or interpretations
- enables deterministic regeneration of outputs
- reduces pressure to “perfect” data during field capture

### Tradeoffs
- increased data model complexity
- additional storage requirements
- reviewers must understand distinction between observation and interpretation

These tradeoffs are acceptable given regulatory, audit, and liability considerations.

---

## Rationale

This separation reflects the reality that:
- field surveys capture **observations**
- compliance review applies **interpretation**
- reports and proposals represent **decisions at a point in time**

Treating these as distinct layers reduces risk and improves long-term system resilience.
