# PAGATEC

**Inventory and business management platform for Mexican small and medium-sized businesses.**

PAGATEC helps SMBs manage inventory, sales, operations, and business information — designed for the Mexican market first: MXN pricing, Spanish (es-MX), SAT/CFDI invoicing, and cash and *fiado* (store credit) sales.

> **Status: documented product — NOT implemented software.**
> This repository contains the product's documentation and approved architecture only. There is no code, no database, no API, and no deployed application yet. An approved architecture is not an implemented architecture. Do not treat any document here as evidence that PAGATEC software exists.

- **Owner:** Yeney B. Herrera, CEO, TECCEF.CORP
- **Repository:** [BigOn1110/pagatec-inventory](https://github.com/BigOn1110/pagatec-inventory)
- **Company:** TECCEF.CORP (founded 2022) — software development, AI, and business automation
- **Contact:** support@aionroid.com

## What PAGATEC is

PAGATEC is a business management platform for small and medium-sized businesses, initially for Mexico, with planned expansion to Colombia, Chile, Peru, Argentina, and Brazil.

Target segments: micro retail, small retail, growing businesses with 3–10 locations, and specialized retail.

Phase 1 covers: authentication, business accounts, inventory management, point of sale (POS), customers, Fiado credit management, offline operation, cloud synchronization, basic reports, an AI business assistant, and subscription management.

## Architecture overview

Approved architecture (SAD v1.2, 2026-07-30, **Approved — Phase 2 Ready**):

- **Local-First with Cloud Canonical Reconciliation** — the business keeps working offline; the cloud holds the canonical state.
- **PostgreSQL as the canonical database** (cloud).
- **Optimistic Concurrency Control with Entity Versioning** — stale updates are detected and rejected, instead of silently overwriting.
- **Transactional Outbox** — local changes are recorded as events and reliably synced to the cloud.
- **Offline-first** — sales, inventory, and Fiado continue without connectivity and reconcile when back online.

See the [Software Architecture Document](PAGATEC_SAD_v1.2_Production_Architecture_Corrections.docx) for the full architecture, and the [Phase 2 database architecture draft](docs/phase2/Database_Architecture_v1.1_DRAFT.md) for the reconciled schema design.

## Technology stack

The technology stack below is a **candidate stack from the project record, not a verified manifest**. It has not been confirmed by a working implementation, and the SAD v1.2 does not specify a tech stack, hosting, AI provider, or pricing.

- Frontend (candidate): React + TypeScript + Vite + shadcn/ui + Tailwind
- Backend (candidate): Node.js / Express
- ORM / database (candidate): Prisma + PostgreSQL (cloud); Dexie / IndexedDB (local) — local persistence technology is still an open decision (Q02/Q03)
- AI (candidate): retrieval-augmented generation; embeddings discussed as `text-embedding-3-small`, 1536-dim pgvector — not confirmed

No package manifest, lockfile, or code exists in this repository to verify any of the above.

## Documentation

Full index with the status of every document: [docs/README.md](docs/README.md).

| Document | Status |
|---|---|
| [Product Vision (PDF)](PAGATEC_Phase0_Product_Vision.pdf) | Complete §§1–23, approved 2026-07-30 |
| [Product Vision (txt)](PAGATEC-INVENTORY-MANAG-BY-TECEEFCORP.txt) | Incomplete — ends at §7.21 |
| [Software Architecture Document v1.2](PAGATEC_SAD_v1.2_Production_Architecture_Corrections.docx) | **Approved — Phase 2 Ready** (2026-07-30) |
| [Database Design Document v1.0](Database%20Design%20Document.txt) | **Pending Approval** |
| [Phase 2 Database Architecture v1.1](docs/phase2/Database_Architecture_v1.1_DRAFT.md) | **DRAFT — NOT APPROVED** |
| [Software Requirements Specification v1.0](docs/srs/SRS_v1.0_DRAFT.md) | **DRAFT — NOT APPROVED** |

## Roadmap

Per the Product Vision (§18):

- **Phase 1 — Foundation, months 1–6:** multi-store (up to 5 stores per business) and multi-warehouse (up to 3 per store), the features listed above.
- **Phase 2 — months 7–12:** native mobile app, read replicas, advanced analytics, CFDI, MFA, feature flags.
- **Phase 3 — months 13–24:** plugin marketplace, multi-region, voice assistant, local LLM, enterprise integrations.

These are planned phases, not delivered milestones.

## Open questions and Gate decisions

The following decisions are **OPEN** and must be made at the Phase 1 Gate Review before implementation. Nothing here invents an answer.

- **Q11 / G-1 — CFDI in Phase 1 (blocking):** the Vision requires CFDI 4.0 / IVA 16% as a Must for Mexico, but CFDI is deferred to Phase 2. A Phase 1 MVP cannot legally invoice in Mexico without it.
- **Q05 — Fiado in the MVP:** Fiado is in Phase 1 scope per the Vision, but its priority and policy need explicit confirmation. Documented defaults: $1,000 MXN credit limit, $50,000 max, 2% monthly interest after a 30-day grace period — defaults, not confirmed policy.
- **Q10 / G-3 — Pricing and subscription model (critical):** undefined; no Billing/Subscription tables exist in the database design.
- **Q01 — Technology stack:** final stack approval still open.
- **Q02/Q03 — Local persistence:** PostgreSQL (cloud) + SQLite (local via IndexedDB) appears only as an unchecked gate item in the Database Design Document — not an approved decision.
- **Q06/Q07 — AI provider and AI measurement/governance:** open.
- **Q17 / G-7 — Disaster recovery / RPO-RTO (critical):** undefined.
- **G-4 — Hardware matrix; G-5 — Migration strategy; G-6 — Support model (critical).**
- **Uptime target:** inconsistent in the Vision — 99.99% (TO-005) vs 99.9% (§8.2/§22.3). The drafts use 99.9% until clarified.
- **MFA:** the Vision requires it; the SAD defers it to Phase 2.
- **Q01–Q18 in full:** see the Phase 2 draft (§17) and the SRS (§16–17).

## Known gaps and conflicts

- The Database Design Document (v1.0, **Pending Approval**, 77 proposed tables) has no explicit `OutboxEvent` table, although the SAD requires a Transactional Outbox. The Phase 2 draft adds one.
- No dedicated Billing/Subscription tables — only subscription attributes on `Business`.
- Sync conflict rules: last-write-wins applies only to explicitly classified non-critical fields; inventory, pricing, credit, and settings require version-checked rejection and manual resolution.
- AI responses must be traceable to database records (AIA-007).

See [CHANGELOG.md](CHANGELOG.md) for versioned corrections.

## Contributing

No code exists yet, so there is no build, test, or contribution workflow to document. Once implementation starts, this section will describe setup, branching, and review rules.

## License

**OPEN — no license has been chosen for PAGATEC.** No `LICENSE` file is included in this repository. AIONRoid (a separate TECCEF.CORP project) is MIT-licensed, but that license and holder must not be assumed for PAGATEC without the owner's confirmation.

## What this repository is not

- Not a working application, demo, or deployment.
- Not a client list, revenue claim, or performance result. No clients, partnerships, certifications, deployments, or metrics are claimed anywhere in this repository.
