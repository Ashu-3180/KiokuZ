# KiokuZ

> **Temporary project README — established product concept and architecture**
>
> This document records the current product direction and architecture decisions. Implementation details may evolve as individual components are designed and validated.

## Overview

KiokuZ is an Android-first, offline-first personal digital archive designed to help people organize and retrieve their personal photos, videos, and documents across devices and storage locations.

The long-term product goal is to provide a personal archive experience with capabilities commonly expected from a photo library, while supporting user-controlled storage such as a laptop, cloud storage, and external hard drives.

## Product structure

The project has two related operating modes:

- **KiokuZ:** the hosted, multi-user product intended for public use.
- **ZigVault:** a privately operated, local-use copy nested within the KiokuZ product architecture. It has independent database and credentials.

Both share the same core architecture and feature direction, while keeping their data and access boundaries independent.

## Established architecture decisions

### Platforms and operating model

- Android-first experience.
- Offline-first behaviour.
- Hybrid storage across supported local and remote storage locations.
- Support for external hard drives.
- Support for Android USB OTG workflows.
- Hosted multi-user KiokuZ and privately operated ZigVault remain separate operating modes.

### Data and storage

- **PostgreSQL** is the selected database technology.
- KiokuZ and ZigVault use independent database instances/databases.
- Media and document storage is separate from the catalogue/database layer; the exact storage adapter design remains to be specified.

### Indexing and processing policy

The approved indexing approach is **P2 — Balanced automatic indexing**:

1. **Tier 0:** discovery.
2. **Tier 1:** structural indexing by default.
3. **Tier 2:** opt-in processing.
4. **Tier 3:** on-demand processing.

OCR and facial-recognition-related capabilities are part of the broader product vision, but deeper processing should follow the tiered policy rather than running indiscriminately on every file.

### Search

The approved direction is **hybrid search (A1)**, combining appropriate indexed metadata and richer retrieval mechanisms as the system evolves. The exact search engine and ranking design remain open.

### File reconciliation and duplicate handling

- **Duplicates — A2:** detect byte-identical duplicates; the user decides whether to merge or link them.
- **Modified files — B2:** ask the user to reconcile detected changes rather than silently overwriting or choosing a version.

### ZigVault synchronization and trust

- **Sync topology — Option B:** primary host plus portable catalogue.
- **Conflict resolution — Option B:** preserve an explicit conflict-resolution workflow; detailed mechanics remain to be specified.
- **Deletion and recovery — Option B:** soft-deletion tombstones.
- **Device identity and trust — Option C:** explicit device approval.

## Conceptual architecture

```text
             Android / Web clients
                     |
                     v
          KiokuZ application layer
                     |
          -----------------------
          |                     |
          v                     v
   Catalogue / metadata     Storage adapters
      (PostgreSQL)          (local / remote /
          |                  external storage)
          |
          v
   Indexing and retrieval
   Tier 0 / Tier 1 by default
   Tier 2 opt-in / Tier 3 on demand
          |
          v
   Search and archive access
```

ZigVault follows the same core product architecture in a private operating mode, with independent database and credentials.

## Current status

**Product concept and the architecture decisions listed above are established.** Detailed component boundaries, APIs, storage-provider contracts, synchronization protocol, security model, and deployment design still require their own specifications.

## Design principles

1. Offline-first access should be a core architectural property, not an afterthought.
2. Keep the catalogue and physical file storage concerns separate.
3. Preserve user control over duplicate and modified-file reconciliation.
4. Make expensive or deeper indexing explicitly tiered.
5. Keep KiokuZ and ZigVault data and credentials independent.
6. Treat device trust and synchronization conflicts as explicit workflows.

---

*This README is a temporary project brief, not a complete technical specification or a claim that all planned features are implemented.*
