# LL88 Survey Pipeline  
*Offline-First Field Survey Capture and One-Way Publish Subsystem*

## Purpose

This repository documents the **survey capture and ingestion subsystem** used to collect lighting field data for LL88 compliance workflows.

The system is designed to:
- support **offline field conditions**
- reduce transcription errors and inconsistency
- preserve **photo-based evidence** alongside quantitative data
- publish surveys into a central system of record for compliance review and proposal development

Rather than replacing the existing FileMaker platform, this subsystem enables **fast, scalable field data capture** while preserving:
- a familiar interface for surveyors
- compatibility with existing data models
- minimal operational disruption

---

## Problem Statement

Lighting surveys are conducted under real-world constraints:
- no cell service (basements, stairwells, garages)
- locked or inaccessible rooms
- rushed site contacts
- dirty or unsafe environments
- incomplete visibility of fixtures and controls

Historically, surveys were captured via:
- handwritten notes later transcribed into FileMaker, or
- notes and photos captured on phones, imported through Excel and manually cleaned

These approaches introduce:
- inconsistent terminology and structure
- transcription and interpretation errors
- loss of contextual detail
- delayed validation and review
- high operational friction

Critically, **photo context** (fixture condition, emergency ballasts, sensors, mounting type, lens state) is often as important as numeric counts — and cannot be reliably reconstructed after the fact.

---

## System Overview

This subsystem introduces an **offline-first survey client** with **one-way publish semantics**.

### High-Level Flow

1. Surveyor captures data on iPhone/iPad using FileMaker Go (offline)
2. Survey data is assembled into a publishable package
3. Surveyor publishes the survey when connectivity is available
4. Central FileMaker server ingests the package and creates a canonical Project
5. Office reviewers analyze compliance and generate proposals or recommendations

There is **no bidirectional synchronization**.  
The field device is not a system of record.

---

## Scope and Non-Goals

### In Scope
- offline survey capture
- structured measures (locations, quantities, fixture types)
- photo capture and association
- one-way publish to central database
- field-generated informational PDFs
- server-side review and compliance analysis

### Explicit Non-Goals
- real-time collaboration
- bidirectional device sync
- installation project management
- incentive submission
- proposal pricing logic

---

## Actors

- **Field Surveyor**  
  Captures surveys and publishes completed packages

- **Compliance Reviewer**  
  Evaluates uploaded surveys and determines LL88 compliance

- **Operations / Admin**  
  Maintains templates, taxonomies, and validation rules

---

## Architectural Boundaries

The system is modeled as four bounded contexts:

### Field Capture (Offline Client)
Owns:
- draft surveys
- local edits
- raw observations
- photo evidence
- “ready to upload” state

### Publish / Intake
Owns:
- validation
- deduplication
- canonical record creation
- acceptance or rejection of uploads

### Review & Compliance Workspace
Owns:
- compliance determinations
- assumptions and exceptions
- proposal triggers

### Artifacts
Owns:
- PDFs and reports
- versioned export snapshots
- audit-ready packets

---

## Survey Lifecycle
```text
Draft
↓
In Progress
↓
Ready to Upload
↓
Uploaded
↓
Accepted (Server)
↓
In Review
↓
Completed
```
Failure paths:
- **Upload Failed** → retry safe
- **Rejected by Intake** → correction required, re-publish

Once accepted by the server, the survey is **logically immutable** on the device.

---

## Publish Semantics (Not “Sync”)

This system uses **publish semantics**, not synchronization.

- Surveys are packaged as **Upload Packages**
- Uploads are **idempotent**
- Re-uploading the same package does not create duplicates
- Server issues an **Upload Receipt**
- Device copies may persist or be deleted without impact

The server is the **system of record**.

---

## Data Model Philosophy

The system separates **observation**, **interpretation**, and **decision**.

- **Raw Data**  
  Field observations, notes, photos, timestamps (immutable post-upload)

- **Derived Data**  
  Normalized fixtures, standardized spaces, calculated values (regenerable)

- **Decisions**  
  Compliance determinations, assumptions, exceptions (versioned)

- **Artifacts**  
  PDFs and reports tied to a specific survey and decision version

---

## Repository Structure
```text
/
├── README.md
├── decision-records/
│   ├── ADR-0001-one-way-publish.md
│   ├── ADR-0002-identifier-strategy.md
│   ├── ADR-0003-export-snapshots.md
│   └── ADR-0004-raw-vs-derived-data.md
├── docs/
│   ├── publish-intake.md
│   ├── failure-modes.md
│   └── data-model.md
├── examples/
│   ├── sample-upload-package.json
│   ├── sample-upload-receipt.json
│   ├── sample-derived-normalization.json
│   ├── sample-compliance-decision.json
│   └── sample-export-manifest.json
└── diagrams/
└── (system context, lifecycle, data flow diagrams)
```
---

## Decision Records

Key architectural decisions are documented in `/decision-records`, including:
- one-way publish vs bidirectional sync
- offline-safe identifier strategy
- raw vs derived data separation
- export snapshot immutability

---

## Status

This repository documents the **target architecture** for the survey pipeline.  
Implementation details evolve independently within the existing FileMaker platform.

---

## What’s Next

Potential extensions include:
- automated intake validation rules
- anomaly detection during review
- integration with incentives, billing, and compliance filing
- read-only audit access
