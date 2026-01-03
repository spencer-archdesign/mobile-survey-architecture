# Data Model Overview

This document describes the logical data layers used in the survey pipeline.

---

## Raw Layer

Represents field observations:
- survey header
- measures
- notes
- photos
- device metadata

Characteristics:
- created offline
- immutable once uploaded
- treated as ground truth

---

## Derived Layer

Represents normalized interpretations:
- canonical fixture types
- standardized space names
- aggregated quantities

Characteristics:
- generated server-side
- may be regenerated
- references raw inputs

---

## Decision Layer

Represents human judgment:
- compliance determinations
- assumptions
- exceptions
- reviewer notes

Characteristics:
- versioned
- evidence-linked
- time-bound

---

## Artifact Layer

Represents official outputs:
- PDFs
- schedules
- manifests

Characteristics:
- immutable snapshots
- tied to survey + decision version
- reproducible
