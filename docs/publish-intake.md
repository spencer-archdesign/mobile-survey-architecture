# Publish & Intake Specification

## Overview

This document describes how offline surveys are **published** from field devices and **ingested** into the central system of record.

This system intentionally avoids bidirectional synchronization in favor of a deterministic, auditable publish process.

---

## Upload Package

A survey is uploaded as a single logical **Upload Package**, containing:

- Survey header (building info, contacts)
- Measures (locations, quantities, fixture observations)
- Photo evidence (with metadata)
- Device metadata
- Upload metadata (timestamps, version)

All records include **locally generated UUIDs**.

---

## Publish Trigger

Publishing occurs when a surveyor:
- marks a survey as “Ready to Upload”
- initiates upload while connectivity is available

The client:
- freezes the survey state
- assembles the Upload Package
- transmits the package to the server

---

## Intake Responsibilities

The intake process is responsible for:

1. **Validation**
   - required fields present
   - UUID integrity
   - file completeness (photos, references)

2. **Deduplication**
   - check survey UUID
   - reject or ignore duplicate submissions

3. **Canonical Creation**
   - create Project record
   - import Measures into canonical tables
   - associate photos and metadata

4. **Receipt Generation**
   - generate Upload Receipt
   - record ingestion timestamp
   - record intake status

---

## Intake Outcomes

### Accepted
- survey imported successfully
- receipt issued
- survey enters Review state

### Rejected
Possible reasons:
- missing required fields
- malformed references
- duplicate UUID collision
- invalid schema version

Rejected surveys may be corrected and re-published.

---

## Idempotency

Uploading the same package multiple times:
- does not create duplicate Projects
- does not duplicate Measures
- returns the original receipt

UUIDs are the source of truth.

---

## Device Post-Publish Behavior

After receipt:
- device may mark survey as Archived
- local copy may be retained or deleted
- no server-side updates are pushed to the device

The device is not a system of record.
