# PAGATEC — Documentation Index

All documents in this repository, with their status. Read the status before quoting any document: **Approved** covers architectural direction only, **Pending Approval** and **DRAFT** are not decisions, and a documented product is not implemented software.

## Root documents (existing, preserved unchanged)

| Document | Status | Notes |
|---|---|---|
| `PAGATEC_Phase0_Product_Vision.pdf` | Complete §§1–23, approved 2026-07-30 by Yeney B. Herrera | Authoritative Vision text. 42 pages. Covers requirements (§7), NFRs (§8), workflows (§9), integrations (§11), security/compliance including CFDI 4.0 / IVA 16% / LFPDPPP (§12), UX es-MX WCAG 2.1 AA (§13), business rules (§14), acceptance (§15), scope (§16), roadmap (§18), risks (§19), gaps GAP-001–GAP-020 (§23). Next action per the Vision: Phase 1 Gate Review after SRS detailed elaboration. |
| `PAGATEC-INVENTORY-MANAG-BY-TECEEFCORP.txt` | Product Vision v1.0, **incomplete** | 503 lines. Ends at Section 7.21 ("Continuing with Section 8 in the next response"). Superseded by the PDF as the authoritative Vision text. |
| `PAGATEC_SAD_v1.2_Production_Architecture_Corrections.docx` | **Approved — Phase 2 Ready** (2026-07-30) | Software Architecture Document v1.2, owner Yeney B. Herrera, CEO, TECCEF.CORP. §§1–10. Local-First with Cloud Canonical Reconciliation, PostgreSQL canonical, Optimistic Concurrency with Entity Versioning, Transactional Outbox. No tech stack, hosting, AI provider, or pricing specified. Approval covers architectural direction only — not the 77-table database design. |
| `Database Design Document.txt` | **Pending Approval** (v1.0, 2026-07-30) | 1,538 lines. 77 proposed tables across 12 groups, UUID PKs, audit fields, integer version, soft delete, businessId tenancy. Approval gate (§16) unchecked. Known gaps: no explicit `OutboxEvent` table despite the SAD requiring a Transactional Outbox; no dedicated Billing/Subscription tables; Fiado defaults ($1,000 credit limit, 2% monthly interest, 30-day grace) are defaults, not confirmed policy. |

## Drafts published in this rehabilitation (2026-11-25)

| Document | Status | Notes |
|---|---|---|
| [`docs/phase2/Database_Architecture_v1.1_DRAFT.md`](phase2/Database_Architecture_v1.1_DRAFT.md) | **DRAFT — NOT APPROVED** | Phase 2 — Complete Database Architecture v1.1. Reconciles the 77-table proposal with SAD v1.2 (48 Phase 1 core / 5 deferred / 3 confirm, plus `OutboxEvent` and 3 Billing tables), Outbox design, Decimal `NUMERIC(19,4)`, 8 Mermaid ERDs, local/cloud schema, migration strategy, API outline. Q01–Q18 all OPEN. |
| [`docs/srs/SRS_v1.0_DRAFT.md`](srs/SRS_v1.0_DRAFT.md) | **DRAFT — NOT APPROVED** | Software Requirements Specification v1.0. Phase 1 only (up to 5 stores/business, up to 3 warehouses/store). All 21 modules with IDs, priorities, and acceptance criteria traced to Vision §7; NFRs, business rules, sync/Outbox, security/compliance, 10 workflows, AI assistant, acceptance/testing, risks/gaps GAP-001–020, traceability matrix, Q01–Q18 OPEN, Phase 1 Gate Review checklist (gate items 2–9 plus G-1–G-15). |

## Labels used in the drafts

- **[Verified]** — directly observable in a repository document read or extracted in full on 2026-11-25.
- **[Historical]** — previously established project decisions or context, not independently re-verified in that pass.
- **[Proposed]** — an elaboration or recommendation, subject to approval at the Phase 1 Gate Review.
- **[OPEN]** — an unknown that must not be invented; a decision is required before implementation.

## Change log

See [`../CHANGELOG.md`](../CHANGELOG.md).
