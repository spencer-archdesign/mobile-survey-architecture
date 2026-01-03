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

Lighting surveys are frequently conducted in **hostile field conditions**:
- no cell service (basements, stairwells, garages)
- locked or inaccessible rooms
- rushed site contacts
- dirty or unsafe environments
- incomplete visibility of fixtures and controls

Historically, surveys were captured using:
- handwritten notes transcribed later into FileMaker, or
- notes and photos recorded on phones, manually re-entered via Excel imports

These workflows introduce:
- inconsistent terminology and structure
- transcription errors
- loss of contextual detail
- delayed validation
- high operational friction

Critically, **photo context** (fixture condition, emergency ballasts, sensors, mounting type, lens state) is often as important as numeric counts — and is difficult to reconstruct after the fact.

---

## System Overview

This subsystem introduces an **offline-first survey client** with **one-way publish semantics**.

### High-Level Flow
1. Surveyor captures data on iPhone/iPad using FileMaker Go (offline)
2. Survey data is packaged locally into a publishable unit
3. Surveyor uploads (“publishes”) the survey when connectivity is available
4. Central FileMaker server ingests the data and creates a new Project
5. Office reviewers analyze compliance and generate proposals or recommendations

There is **no bidirectional sync**. The field device is not a system of record.

---

## Scope and Non-Goals

### In Scope
- offline survey capture
- structured measures (locations, quantities, fixture types)
- photo capture and association
- one-way publish to central database
- basic field-generated PDFs
- review and compliance analysis (server-side)

### Explicit Non-Goals
- real-time collaboration
- bidirectional device sync
- installation project management
- incentive submission
- proposal pricing logic

---

## Actors

- **Field Surveyor**  
  Captures survey data and publishes completed surveys

- **Compliance Reviewer**  
  Evaluates uploaded surveys for LL88 compliance

- **Operations / Admin**  
  Maintains templates, taxonomies, and validation rules

---

## Bounded Contexts and Ownership

### Field Capture (Offline Client)
Owns:
- draft surveys
- local edits
- raw observations
- photo evidence
- “ready to upload” state

### Publish / Intake
Owns:
- upload validation
- deduplication
- creation of canonical Project records
- acceptance or rejection of uploads

### Review & Compliance Workspace
Owns:
- compliance determinations
- notes and exceptions
- proposal triggers

### Artifacts
Owns:
- PDF exports
- versioned snapshots
- audit-ready packets

---

## Survey Lifecycle
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

Failure paths:
- **Upload Failed** → retry safe
- **Rejected by Intake** → correction required, re-publish

Once accepted by the server, the survey is **logically immutable** on the device.

---

## Publish Semantics (Not “Sync”)

This system uses **publish semantics**, not synchronization.

- Each survey is packaged as an **Upload Package**
- Uploads are **idempotent**
- Re-uploading the same package does not create duplicates
- Server generates an **Upload Receipt** (timestamp, IDs, status)
- Device copies may persist or be deleted without impact

The server is the **system of record**.

---

## Data Model Philosophy

### Raw Data
- what the surveyor entered
- original photos
- timestamps and device metadata

### Derived Data
- normalized fixture taxonomy
- standardized locations
- aggregated counts

### Decisions
- compliance determinations
- reviewer notes
- exceptions and assumptions

### Artifacts
- PDFs and reports
- tied to a specific survey version
- reproducible via manifests

---

## Why This Architecture

This design:
- preserves field speed and familiarity
- minimizes sync complexity
- supports offline reality
- reduces transcription errors
- retains photo-based context
- scales cleanly within an existing FileMaker architecture

---

## Decision Records

See `/decision-records` for architecture decisions, including:
- one-way publish vs sync
- identifier strategy
- export snapshotting

---

## What’s Next

Future work may include:
- intake validation rules
- automated anomaly detection
- integration with incentives and billing systems
- read-only client access for audit review
