# PAGATEC — Software Requirements Specification (SRS) v1.0 DRAFT

**Status: DRAFT — NOT APPROVED. Do not implement from this document alone.**
**Date:** 2026-11-25
**Repository:** BigOn1110/pagatec-inventory, branch `main`
**Owner:** Yeney B. Herrera, CEO, TECCEF.CORP
**Previous documents:** Phase 0 Product Vision & Business Requirements v1.0 (2026-07-30, complete in PDF, §§1–23); SAD v1.2 (2026-07-30, Approved — Phase 2 Ready); Database Design Document v1.0 (2026-07-30, Pending Approval); Phase 2 Database Architecture v1.1 DRAFT (2026-11-25, DRAFT — NOT APPROVED)

> PAGATEC is a documented product, not implemented software. An approved architecture is not an implemented architecture, and this SRS is not evidence that any code, database, API, or sync service exists. Nothing in this document claims that PAGATEC software exists.

## How to read this SRS — labels

Every statement in this SRS carries one label. Do not drop the label when quoting it.

- **[Verified]** — Directly observable in a repository document that has been read or extracted in full on 2026-11-25: the Phase 0 PDF (42 pages, 2,271 lines, §§1–23), the SAD v1.2 DOCX (74 paragraphs, §§1–10), the Database Design Document txt (1,538 lines), or the Product Vision txt (503 lines, ends §7.21).
- **[Historical]** — Previously established project decisions, company context, or requirements supplied in the project record / ChatGPT handoffs 2026-11-25. Not independently re-verified from a document in this pass.
- **[Proposed]** — An elaboration or recommendation in this SRS, subject to Yeney's approval at the Phase 1 Gate Review. Not an approved decision.
- **[OPEN]** — An unknown that must not be invented. A decision is required before implementation.

---

## 1. Introduction

### 1.1 Purpose [Verified]

This SRS is the detailed elaboration of the Phase 0 Product Vision that the Vision document itself names as the next action before the Phase 1 Gate Review: *"Phase 1 Gate Review upon completion of Software Requirements Specification (SRS) detailed elaboration."* [Verified — Vision PDF, Document Approval / End of Document]

Its purpose is to turn the Vision's product requirements (§7), non-functional requirements (§8), business rules (§14), acceptance criteria (§15), and scope (§16) into a testable, traceable specification for the Phase 1 MVP only. It does not specify Phase 2 or Phase 3 features except where they must be explicitly excluded.

### 1.2 Scope of this SRS [Verified]

This SRS covers **Phase 1 — Foundation (Months 1–6)** only, as defined in Vision PDF §16.1 and §18.1 and SAD §4. Anything listed in Vision §16.2 (Phase 2), §16.3 (Phase 3), or §17 (Out of Scope) is explicitly out of scope for this SRS and is recorded as such in §3.3 — it is not specified here, and its absence is not an oversight.

Phase 1 is the MVP for the Mexican SMB market [Verified — Vision §18.1]. Its success gate, as stated in the Vision, is 100 beta businesses actively using the product, NPS ≥ 40, and zero data-loss incidents [Verified — Vision §18.1]. Those are the Vision's stated targets, not measured results.

### 1.3 Definitions, acronyms, abbreviations [Verified — Vision §21]

| Term | Meaning |
|---|---|
| CFDI | Comprobante Fiscal Digital por Internet — Mexico's electronic invoicing standard [Verified — Vision §21.1] |
| COFEPRIS | Comisión Federal para la Protección contra Riesgos Sanitarios — Mexican health regulator [Verified — Vision §21.2] |
| COGS | Cost of Goods Sold [Verified — Vision §21.2] |
| Fiado | Customer credit; selling goods with payment deferred to a future date [Verified — Vision §21.2] |
| IVA | Impuesto al Valor Agregado — Mexico's value-added tax, 16% standard rate [Verified — Vision §21.1] |
| LFPDPPP | Ley Federal de Protección de Datos Personales en Posesión de los Particulares — Mexico's data privacy law [Verified — Vision §21.1] |
| LGPD / GDPR | Brazil / EU data protection law — future readiness only [Verified — Vision §8.7] |
| MVP | Minimum Viable Product [Verified — Vision §21.1] |
| OCC | Optimistic Concurrency Control (SAD §2) [Verified — SAD §2] |
| POS | Point of Sale [Verified — Vision §21.1] |
| RAG | Retrieval-Augmented Generation — AI architecture grounding responses in actual data [Verified — Vision §21.1] |
| RBAC | Role-Based Access Control [Verified — Vision §21.1] |
| RFC | Registro Federal de Contribuyentes — Mexican tax ID [Verified — Vision §21.1] |
| SAT | Servicio de Administración Tributaria — Mexico's tax authority [Verified — Vision §21.1] |
| SKU | Stock Keeping Unit [Verified — Vision §21.1] |
| SRS | Software Requirements Specification [Verified — Vision §21.1] |
| WCAG | Web Content Accessibility Guidelines [Verified — Vision §21.1] |

Two glossary entries in the Vision are noted as not applicable to PAGATEC and are preserved here only to avoid confusion: FEN (Forsyth-Edwards Notation, chess) and PGN (Portable Game Notation, chess), both marked "(not applicable to PAGATEC, retained for completeness)" in the Vision [Verified — Vision §21.1]. They have no bearing on this SRS.

### 1.4 References [Verified]

| ID | Document | Status | Role in this SRS |
|---|---|---|---|
| R1 | PAGATEC Phase 0 Product Vision & Business Requirements, PDF, 42 pp., §§1–23, approved 2026-07-30 | Complete, extracted 2026-11-25 | **Authoritative source for requirements.** Supersedes the truncated txt file (see §1.5). |
| R2 | SAD v1.2, Production Architecture Corrections, DOCX, §§1–10, approved 2026-07-30 | Complete, extracted 2026-11-25 | Authoritative source for architecture decisions. Approves architectural direction only, not the 77-table schema (see §1.6). |
| R3 | Database Design Document v1.0, txt, 1,538 lines, 2026-07-30 | **Pending Approval** | Source for data entities and the 77-table proposal. Reconciled in the Phase 2 draft; its §16 approval gate is still unchecked. |
| R4 | Phase 2 Database Architecture v1.1 DRAFT, 905 lines, 2026-11-25 | **DRAFT — NOT APPROVED** | Source for the reconciled table dispositions (48 core / 5 deferred / 3 confirm + OutboxEvent + 3 Billing), the OutboxEvent design, and the §14 gate items 2–9 reproduced in §17 of this SRS. |
| R5 | Product Vision txt v1.0, 503 lines, 2026-07-30 | Incomplete — ends §7.21 | Historical record only. Do not use as the requirements source; use R1. |

### 1.5 Document authority — which text governs [Verified]

The Vision txt file (R5) lists §§1–23 in its table of contents but its body ends at §7.21 with *"That covers Sections 1 through 7. Continuing with Section 8 in the next response."* [Verified — Vision txt]. The PDF (R1) contains all of §§1–23 and is approved by Yeney B. Herrera, CEO / Product Owner, and the TECCEF.CORP Engineering Team, 2026-07-30 [Verified — Vision PDF, Document Approval]. **The PDF is therefore the authoritative Vision text; the txt file is preserved as the historical record.** Conflict C-I from the Phase 2 draft is resolved on this point [Verified — Phase 2 draft §2.1, C-I].

### 1.6 Approval hierarchy [Verified — Phase 2 draft §2.1, C-A; SAD §10; DB §16]

- SAD v1.2 is **Approved — Phase 2 Ready** [Verified — SAD §10, extracted text: all nine gate items checked, "APPROVED FOR PHASE 2"]. That approval covers the architectural direction: Local-First with Cloud Canonical Reconciliation, Optimistic Concurrency with Entity Versioning, Transactional Outbox, PostgreSQL domain boundaries, AI permission filtering, and the controlled MVP scope.
- The Database Design Document v1.0 is **Pending Approval** [Verified — DB header]. Its §16 approval gate ("Before Phase 3 (UI/UX Design) begins") lists 8 items as unchecked `[ ]`: 77 tables accepted, UUID strategy, soft deletes, audit columns, tenant isolation, dual DB, indexes, constraints [Verified — DB §16, Phase 2 draft §1.1].
- **SAD approval does not approve the 77-table schema.** This SRS treats SAD decisions as binding only to the extent SAD explicitly establishes them; database entities, fields, constraints, business defaults, billing structures, and synchronisation details must be reconciled against those decisions and separately approved at the Gate Review (§17). This is the Document Authority language proposed by the independent ChatGPT audit 2026-11-25 and adopted here as **[Proposed]**.

### 1.7 Document conventions

Requirement IDs are preserved verbatim from the Vision: functional IDs use the module prefix (INV, POS, PUR, SUP, CUS, FIA, FIN, RPT, ANL, EMP, SEC, WHS, MST, AIA, NTF, SET, OFF, CLD, ADT, BKP, IEX); non-functional IDs use NFR-PER / NFR-AVL / NFR-SEC / NFR-SCL / NFR-USA / NFR-MNT / NFR-CMP; business rules use BR-INV / BR-SAL / BR-PUR / BR-FIA / BR-SEC; acceptance criteria use AC-GEN / AC-INV / AC-POS / AC-FIA / AC-SYNC; risks use R-T / R-B / R-M; gaps use GAP-001–GAP-020 [Verified — Vision §§7–8, 14–15, 19, 23].

Priorities are the Vision's own: **Must / Should / Could** [Verified — Vision §7]. "Must" means the Phase 1 Gate cannot pass without it (subject to the Gate decisions in §17, notably C-K/CFDI). "Should" means required for a complete Phase 1 unless the Gate explicitly defers it. "Could" means explicitly out of the Phase 1 baseline unless the Gate adds it — see §3.3.

---

## 2. Overall description

### 2.1 Product perspective [Verified — Vision §§2, 6; Historical — TECCEF.CORP]

PAGATEC is a commercial business-management platform developed by TECCEF.CORP, initially targeting Mexican small and medium-sized businesses, with intended expansion across Latin America [Historical — ChatGPT handoff 2026-11-25; Verified — Vision §4.1–4.2]. The Vision describes it as a complete business operating system rather than merely an inventory application [Verified — Vision §2.1]. Its central purpose is to give small businesses capabilities normally associated with expensive enterprise software without imposing enterprise-level complexity [Verified — Vision §2.1].

The fundamental problem it is intended to solve [Verified — Vision §2]: small businesses operate through disconnected systems — inventory in notebooks or spreadsheets, sales through independent cash registers, customer debts tracked informally, supplier purchases through messages and paper invoices, financial reporting assembled manually, decisions made without reliable operational data, and internet interruptions disrupting essential operations. PAGATEC is intended to consolidate these into one system.

**Relationship to the other TECCEF.CORP initiatives — do not conflate** [Historical — ChatGPT handoffs 2026-11-25]:
- **TECCEF.CORP** — the company / broader technological development initiative, founded 2022. It owns the commercial and research ambitions, not any single product.
- **PAGATEC** — this product: practical commercial business software. It must be independently testable and useful even if AION research never reaches production maturity.
- **AIONRoid** — the public-facing AI-solutions brand (AI Analytics, Process Automation, AI Consulting). Its website is separate from PAGATEC; its existence does not demonstrate that PAGATEC's backend or AI capabilities have been implemented.
- **AION** — a deeper research / cognitive architecture initiative (Persistent Cognitive Actors, memory, continuity, purposeful cooperation, controlled adaptation). PAGATEC may eventually provide a practical environment in which certain AION concepts can be tested, but AION must not become a dependency of the PAGATEC MVP.

### 2.2 Product principles [Verified — Vision §6]

The Vision establishes these principles; they are requirements on how the system behaves, not evidence that the capabilities exist:

1. **Simplicity over features:** users should not need extensive training [Verified — Vision §6.1].
2. **Offline-first:** core business operations must continue without internet [Verified — Vision §6.1, §8.2 NFR-AVL-002].
3. **Intelligence by default:** AI should help interpret business operations [Verified — Vision §6.1].
4. **Data integrity:** transactions must not disappear or become unrecoverable [Verified — Vision §6.1].
5. **Traceability:** operations must be attributable to people, times, and reasons [Verified — Vision §6.1].
6. **Recoverability:** business data must be recoverable after failures [Verified — Vision §6.1].
7. **No vendor lock-in:** customers must retain the ability to export their data [Verified — Vision §6.1].
8. **Nothing is ever truly deleted** [Verified — Vision §6.1 principle 4, §14.5 BR-SEC-002] — see §6.5 and conflict C-E.

### 2.3 User classes and characteristics [Verified — Vision §5]

Six personas are defined in the Vision; each is a user class for Phase 1:

| Persona [Verified — Vision §5] | Role in the system | Key needs |
|---|---|---|
| María González — "The First-Time Owner" (§5.1) | Business owner, first-time user | 15-minute onboarding, first sale within 15 minutes (PO-001), no training for basic POS (PO-003) |
| Carlos Méndez — "The Cashier" (§5.2) | Cashier, POS operator | Touch-optimised POS, barcode scanning, shift open/close, fast sale entry |
| Ana Reyes — "The Warehouse Manager" (§5.3) | Warehouse / inventory | Stock counts, adjustments, transfers, expiration and batch tracking |
| Roberto Vargas — "The Multi-Store Owner" (§5.4) | Multi-store owner | Consolidated reporting across up to 5 stores, store-level permissions |
| Luis Hernández — "The Restaurant Manager" (§5.5) | Restaurant manager | Recipe/BOM tracking — see GAP-012, §18: recipe tracking is mentioned for restaurants but not in the core modules |
| Dra. Elena Castillo — "The Pharmacy Owner" (§5.6) | Pharmacy owner | Expiration, batch/lot tracking, controlled-substance tracking (COFEPRIS, NFR-CMP-005) |

System roles for RBAC are defined separately in §4.11 (SEC-001): Owner, Manager, Cashier, Warehouse, Accountant, Read-Only [Verified — Vision §7.11].

### 2.4 Operating environment [Verified — Vision §20]

**Assumptions** (Vision §20.1, A-001–A-010) — these are assumptions the design relies on; if any is false, the affected requirement must be revisited at the Gate:
- A-001: target businesses have at least one Android tablet or Windows PC with Chrome.
- A-002: internet, while intermittent, is available at least once per day for sync.
- A-003: business owners are motivated to reduce inventory losses and improve profitability.
- A-004: SAT/CFDI requirements will remain stable enough to build against during Phase 1 — **in tension with C-K (§9.3); A-004 cannot be relied on until the CFDI Gate decision is made.**
- A-005: barcode usage is prevalent or growing.
- A-006: customers are willing to pay a monthly subscription (SaaS model viable) — **no pricing model is defined (GAP-001, §14); A-006 is untested.**
- A-007: AI model APIs (OpenAI, Anthropic, or local) will remain available and cost-effective — **no AI provider is selected (Q07, OPEN).**
- A-008: target users can read and write Spanish at a basic business level.
- A-009: cloud infrastructure costs will scale predictably — **no hosting provider is selected (Q08, OPEN).**
- A-010: regulatory compliance can be met without architectural redesign — **in tension with C-K; see §9.3.**

**Constraints** (Vision §20.2, C-001–C-010):
- C-001: development budget and timeline are fixed per phase gates.
- C-002: target hardware minimum: 4GB RAM, 32GB storage, Android 8+ or Windows 10+ with Chrome.
- C-003: must support intermittent connectivity; cannot assume always-online operation.
- C-004: must comply with Mexican tax and data privacy laws before launch — **in tension with C-K; a Phase 1 launch without CFDI would breach C-004 for invoicing, see §9.3.**
- C-005: AI features must operate within reasonable token/cost limits per query.
- C-006: must be deployable on standard cloud infrastructure without custom hardware.
- C-007: must support both touch and mouse/keyboard interaction.
- C-008: must function on low-bandwidth connections (minimum 1 Mbps for sync).
- C-009: cannot require customers to purchase proprietary hardware.
- C-010: must achieve break-even within 24 months of launch.

### 2.5 Design and implementation constraints [Verified — SAD §§1–8; Vision §8]

- **Architecture:** Local-First Operational Model with Cloud Canonical Reconciliation [Verified — SAD §1]. The device has temporary operational authority for sales, payments, inventory movements, and offline Fiado transactions; those are provisional business events until cloud reconciliation. The cloud PostgreSQL database is the canonical business state and decides final inventory availability, cross-device conflicts, credit-limit enforcement, and reporting totals [Verified — SAD §1].
- **Synchronisation algorithm:** Optimistic Concurrency Control with Entity Versioning, replacing Operational Transformation / version vectors [Verified — SAD §2]. Each synchronised entity carries `id, version, updatedAt, deviceId, lastModifiedBy`.
- **Events:** Transactional Outbox, not direct in-memory events [Verified — SAD §3]. A business transaction and its outbox event are persisted atomically in one PostgreSQL transaction; a background worker dispatches to inventory, AI, and notifications.
- **Database:** modular monolith, PostgreSQL, organised by domain — `auth, business, inventory, sales, customers, fiado, billing, audit, ai` — each module owning its schema, cross-module access via service contracts [Verified — SAD §5].
- **AI:** mandatory permission filtering — identity verification → permission validation → business data retrieval → RAG context filtering → LLM processing → response validation; AI never bypasses permissions, never generates unrestricted DB queries, sensitive fields need explicit authorisation, financial recommendations must cite source data [Verified — SAD §6].
- **Security hardening (mandatory before production launch):** HMAC SHA-256 sync signatures, device identification, payload integrity; JWT secrets rotated every 90 days, API credentials rotated regularly, no secrets in source control; prompt-injection filtering, structured JSON outputs, maximum token limits, AI audit logging; Content Security Policy with restricted script sources, disabled unsafe inline execution, trusted API origins only [Verified — SAD §8].
- **Stack / hosting / AI provider:** **not specified in SAD** [Verified by absence in the extracted SAD text — Phase 2 draft §1.3]. The candidate stack (React + TypeScript + Vite + Express + Prisma, Dexie/IndexedDB local, RAG) is **[Historical]** planning context only, not an approved specification. Q01, Q06–Q10 remain **[OPEN]**.

### 2.6 Phase 1 limits [Verified — Vision §16.1]

Phase 1 is explicitly bounded:
- Multi-store support: **up to 5 stores per business account** [Verified — Vision §16.1 item 13].
- Multi-warehouse support: **up to 3 warehouses per store** [Verified — Vision §16.1 item 14].
- These limits are part of the scope, not implementation details. Unlimited stores/warehouses are Phase 3 [Verified — Vision §16.3].

---

## 3. Phase 1 scope

### 3.1 In scope — Phase 1 MVP [Verified — Vision §16.1; SAD §4]

The following 16 capabilities are in scope for Phase 1. Each is elaborated in §4 (functional), §5 (non-functional), or §6 (business rules):

1. Core Inventory Management — product catalog, stock tracking, adjustments, barcode support.
2. Point of Sale — touch-optimised POS, cash/card/Fiado payments, receipts, shifts.
3. Basic Purchasing — purchase orders, goods receipt, supplier linking.
4. Customer & Supplier Management — master data, history tracking.
5. Fiado (Credit) — credit limits, balance tracking, payments, statements.
6. Basic Reporting — daily sales, inventory valuation, low-stock, Fiado aging.
7. Role-Based Security — Owner, Manager, Cashier, Warehouse roles (SEC-001 also lists Accountant and Read-Only; see §4.11).
8. Offline-First Operation — full offline functionality with sync.
9. Cloud Sync & Backup — automatic synchronisation and daily backups.
10. Audit Logging — complete action tracking.
11. Settings & Configuration — business profile, tax, receipt customisation.
12. AI Business Assistant — natural-language queries, basic insights.
13. Multi-Store Support — up to 5 stores per business account.
14. Multi-Warehouse Support — up to 3 warehouses per store.
15. Dark/Light Mode — theme switching.
16. Spanish/English — complete UI translation.

SAD §4 states the same Phase 1 list in its table (authentication, business accounts, inventory management, point of sale, customers, Fiado credit management, offline operation, cloud synchronisation, basic reports, AI business assistant, subscription management) [Verified — SAD §4]. Note: SAD lists "subscription management" in Phase 1, but the database design has no dedicated Billing/Subscription tables — only subscription attributes on Business. That gap is conflict C-H, carried into §4.10 and §17 [Verified — Phase 2 draft §2.1, C-H; Vision §23 GAP-001].

### 3.2 Phase 1 roadmap [Verified — Vision §18.1]

Phase 1 runs **Months 1–6**, goal: launch MVP for the Mexican SMB market. Deliverables: complete responsive web application, offline-first architecture, core inventory / POS / purchasing / Fiado, basic reports and dashboard, AI assistant (basic queries), cloud sync and backup, role-based security, multi-store (up to 5), Spanish/English support [Verified — Vision §18.1]. Success gate: 100 beta businesses actively using; NPS ≥ 40; zero data-loss incidents [Verified — Vision §18.1].

Phase 2 (Months 7–12) and Phase 3 (Months 13–24) are summarised in §3.3 only as exclusions; their deliverables are not specified here.

### 3.3 Out of scope for Phase 1 [Verified — Vision §§16.2, 16.3, 17; SAD §4]

**Deferred to Phase 2** (Vision §16.2; SAD §4 table): advanced analytics and dashboards; native mobile applications (iOS/Android); **electronic invoicing (CFDI) integration** — see C-K, §9.3; payment gateway integration (Mercado Pago, OpenPay); advanced promotions engine; customer loyalty program; employee scheduling and attendance; advanced AI forecasting; plugin/extension system (basic); REST API for third-party integrations.

**Deferred to Phase 3** (Vision §16.3; SAD §4 table): unlimited stores and warehouses; supplier portal; customer portal; marketplace integration; advanced multi-currency; voice assistant; accounting software integrations; advanced regulatory compliance (LGPD, GDPR); multi-region deployment; local LLM infrastructure; enterprise integrations; plugin marketplace.

**Explicitly out of scope in all phases as currently planned** (Vision §17): e-commerce website builder; payroll processing; advanced accounting (general ledger, double-entry, balance sheet / cash flow — basic P&L reporting is in scope); hardware manufacturing/sales; social media management; delivery logistics; manufacturing/MRP with work orders and production scheduling (recipe tracking for restaurants is in scope — see GAP-012, §14); franchise management (royalty calculation, franchisee billing); cryptocurrency payments; legal/contract management.

**Features whose Phase 1 status is ambiguous in the Vision and must be decided at the Gate — do not assume them in or out:**
- MFA: Vision §12.1 lists MFA support for owner/manager roles as a security requirement, but SAD §4 defers MFA to Phase 2 [Verified — Vision §12.1; SAD §4]. **[OPEN]** — see §17.
- Biometric authentication and passwordless magic-link login: listed in Vision §12.1 (biometric as a requirement, magic link as "future") [Verified — Vision §12.1]. **[OPEN]** — not in the Phase 1 baseline in this SRS.
- WhatsApp notifications: NTF-005 lists WhatsApp as "future" [Verified — Vision §7.15]. Out of Phase 1.
- Recipe/BOM: mentioned for the restaurant persona (§5.5) but not in the core modules; GAP-012 recommends adding recipe management to Phase 1 if the restaurant segment is a priority [Verified — Vision §§5.5, 23.3]. **[OPEN]** Q — see §17.
- Expense tracking beyond COGS: GAP-008 notes the Financial Dashboard is listed but no explicit expense recording exists [Verified — Vision §23.2]. **[OPEN]** — see §14.
- Supplier payment tracking: GAP-009 notes purchasing tracks POs but not actual payments to suppliers [Verified — Vision §23.2]. **[OPEN]** — see §14.

---

## 4. Functional requirements — Phase 1 elaboration

Each requirement below preserves its Vision ID, wording, priority, and acceptance criterion [Verified — Vision §7], and adds an **Elaboration** ([Proposed] unless marked [Verified]) stating what the implementation must do to satisfy it in Phase 1, and a **Phase 1 status**. Where a Vision requirement is Should/Could, the elaboration states whether Phase 1 includes it, defers it, or leaves it to the Gate.

Traceability: every requirement traces to Vision §7 (module), to the Phase 2 draft table group (§3), and to a test family in §13. The traceability matrix is in §15.

### 4.1 Inventory Management (INV) [Verified — Vision §7.1]

Purpose [Verified — Vision §7.1]: product catalog, stock tracking, adjustments, barcode support, movement history.

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status / Elaboration |
|---|---|---|---|---|
| INV-001 | Support unlimited products per business | Must | Tested with 100,000-product catalog | **Phase 1.** The catalog must not impose an arbitrary per-business product cap in Phase 1 (contrast with subscription tier maxima, which are **[OPEN]** — see §17). Test at 100,000 products per NFR-SCL-001. |
| INV-002 | Each product shall have: name, SKU, barcode, category, description, unit of measure, cost price, sale price, tax rate, minimum stock, maximum stock, initial quantity | Must | All fields validated on create/edit | **Phase 1.** All fields are required/validated on create and edit. SKU unique within a business (BR-INV-001); barcode unique within a business unless explicitly configured as a variant (BR-INV-002). |
| INV-003 | Support multiple units of measure per product (e.g. box of 24, individual unit) with automatic conversion | Should | Conversion math verified across 1,000 transactions | **Phase 1.** Include: unit conversions must be exact (Decimal, never float) and verified across 1,000 transactions. |
| INV-004 | Support product variants (size, color, flavor) under a parent product | Should | Parent/child hierarchy functional | **Phase 1.** Include: variant stock is tracked per variant, not only per parent. |
| INV-005 | Support barcode scanning via camera and external scanner | Must | Scan-to-add functional on Android tablet | **Phase 1.** Include: camera scan and external scanner both supported; barcodes conform to EAN-13 or UPC-A (Vision §10.4). |
| INV-006 | Track stock levels in real-time across all warehouses/stores | Must | Stock level accurate within 2 seconds of transaction | **Phase 1.** "Real-time" means: stock level reflects a committed transaction within 2 seconds on the same device; cross-device consistency is governed by the sync rules in §8 (CLD-001: within 5 seconds when online). |
| INV-007 | Alert when stock falls below minimum threshold | Must | Alert generated within 1 minute of threshold breach | **Phase 1.** Alert via in-app notification (NTF-001, Must); email/WhatsApp are Should/future (NTF-005). |
| INV-008 | Support inventory adjustments with mandatory reason code | Must | Every adjustment requires reason; logged in audit trail | **Phase 1.** Adjustment requires a reason code (BR-INV-004: "Other" requires text explanation); adjustment is an `InventoryMovement` + `StockAdjustment` record, never a silent overwrite of `StockLevel`. |
| INV-009 | Support physical stock counts with variance reporting | Must | Count sheet → entry → variance report workflow functional | **Phase 1.** Counts over 5% or $1,000 MXN variance require manager approval (BR-INV-008). |
| INV-010 | Track product expiration dates and alert 30/15/7 days before expiry | Should | Alert accuracy verified | **Phase 1.** Include: expiration alerts at 30/15/7 days (also NTF-002, Should). Expired products cannot be sold through POS (BR-INV-006). |
| INV-011 | Support product images (up to 5 per product) | Should | Image upload, display, delete functional | **Phase 1 — conditional.** Include only if the object-storage decision (Q09, OPEN) is made at the Gate; otherwise defer the upload path and record the deferral. Image storage must not be committed to the repository or the database as binary blobs [Proposed]. |
| INV-012 | Support product status: Active, Inactive, Discontinued | Must | Status changes reflected in POS and reports | **Phase 1.** Discontinued/inactive products must not appear as saleable in POS. |
| INV-013 | Calculate inventory value by cost method (FIFO, weighted average, LIFO) | Should | Calculation matches manual accounting verification | **Phase 1.** The valuation method is a per-business setting, not a hard-coded default [Proposed]. Which method is the default is **[OPEN]** — business-rules document required. |
| INV-014 | Support bulk import via CSV/Excel | Should | 1,000 products imported in < 60 seconds | **Phase 1.** Include, with validation errors reported (IEX-001). Data migration from CSV/Excel must complete with < 1% error rate (AC-GEN-008). |
| INV-015 | Support bulk export via CSV/Excel/PDF | Should | Export matches database state exactly | **Phase 1.** Export is the no-vendor-lock-in mechanism (Vision §6.1 principle 7). |
| INV-016 | Track product movement history (in, out, adjustment, transfer) | Must | Complete history retrievable by product | **Phase 1.** Movement history is the operational record of stock changes; `StockLevel` is a reconciled balance/projection of movements (Phase 2 draft §8.4). Movements are immutable — correct them only through traceable reversal/adjustment records. |
| INV-017 | Support serial number tracking for high-value items | Could | Serial tracked from purchase through sale | **Phase 1 — deferred.** Could priority; not in the Phase 1 baseline. The database design includes a `SerialNumber` table (Phase 2 draft §3.2) — its Phase 1 inclusion is a Gate decision if a customer segment requires it. |
| INV-018 | Support batch/lot tracking for regulated industries | Should | Batch traceable from supplier to customer | **Phase 1.** Include for pharmacy / regulated segments (persona §5.6, COFEPRIS). Batch traceability is required for controlled-substance tracking (NFR-CMP-005). |

### 4.2 Point of Sale (POS) [Verified — Vision §7.2]

Purpose [Verified — Vision §7.2]: touch-optimised sale entry, payment, receipts, shifts, returns.

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status / Elaboration |
|---|---|---|---|---|
| POS-001 | Touch-optimised POS interface | Must | All primary actions accessible via touch on 10" tablet | **Phase 1.** Touch targets minimum 44×44px (NFR-USA-004); mobile-first on 7" tablet, optimised on 10" (Vision §13.1). |
| POS-002 | Barcode scanning for rapid item entry | Must | 50 items scanned in < 60 seconds | **Phase 2 (SRS §5).** See NFR-PER-006: < 2 seconds per item. |
| POS-003 | Manual product search by name, SKU, or category | Must | Search returns results in < 500 ms | **Phase 1.** Search < 500 ms for 10,000 products (NFR-PER-003). |
| POS-004 | Calculate subtotal, taxes, and total automatically | Must | 1,000 test transactions; 100% accuracy | **Phase 1.** Sale total must equal sum of item prices minus discounts plus taxes (BR-SAL-002). Money is Decimal `NUMERIC(19,4)` in the database (Phase 2 draft §5); never float. IVA at 16% standard, with 0% and exempt categories (Vision §12.3). |
| POS-005 | Multiple payment methods: Cash, Card, Transfer, Mixed | Must | Each payment type processed and recorded correctly | **Phase 1.** Cash, Card, Transfer, Mixed are in scope. Which payment methods must work offline is **[OPEN]** (Q12) — see §8. Card processing in Phase 1 is via the payment method record, not a payment-gateway integration (gateways are Phase 2, §3.3). |
| POS-006 | Customer credit (Fiado) sales with credit-limit enforcement | Must | Sale blocked if credit limit exceeded | **Phase 1.** Fiado sale blocked if sale amount + current balance would exceed the limit (BR-SAL-006, BR-FIA-002). See §4.6. |
| POS-007 | Generate receipt with business info, items, taxes, payment method, transaction ID | Must | Receipt printable and digital (PDF/email) | **Phase 1.** Receipt template customisable (SET-004). A receipt is **not** a CFDI invoice — see C-K, §9.3. |
| POS-008 | Receipt reprinting for last transaction | Must | Reprint functional within 24 hours | **Phase 1.** |
| POS-009 | Sale cancellation/void with manager approval and reason | Must | Void requires supervisor PIN; logged | **Phase 1.** Cashier cannot void their own transaction; requires supervisor PIN (BR-SAL-003). Void is a traceable record, not a deletion. |
| POS-010 | Returns and exchanges with original transaction reference | Must | Return links to original sale; inventory restored | **Phase 1.** Return quantity cannot exceed original quantity (BR-SAL-004). Return restores inventory as a movement, not a silent stock edit. |
| POS-011 | Discounts (percentage or fixed) per item or per ticket | Should | Discount authorised per role settings | **Phase 1.** Discounts exceeding 20% require manager approval (BR-SAL-005). |
| POS-012 | Promotions (buy X get Y, bundle pricing) | Could | Promotion engine functional | **Phase 1 — deferred.** Could priority; advanced promotions are Phase 2 (§3.3, GAP-011). The database design includes `Promotion`/`PromotionItem` tables — their Phase 1 inclusion is a Gate decision. |
| POS-013 | Operate in offline mode with local queue | Must | 100 transactions processed offline; synced correctly | **Phase 1.** Core offline requirement — see §8. Offline sales are provisional business events (SAD §1) until cloud reconciliation. |
| POS-014 | Display real-time stock level during sale | Should | Stock visible without leaving POS screen | **Phase 1.** Stock shown is the local device's stock snapshot; it may be stale relative to other devices — the UI must not present it as cloud-canonical [Proposed]. |
| POS-015 | Shift management (open/close with cash count) | Must | Shift reconciliation balanced or flagged | **Phase 1.** End-of-shift cash variance exceeding $200 MXN requires manager investigation (BR-SAL-008). See workflow §10.9. |
| POS-016 | Multiple POS stations per store | Must | 5 stations operational simultaneously | **Phase 1.** 5 stations per store is the stated test; the number of simultaneous devices per business overall is **[OPEN]** (Q13). |
| POS-017 | Quick-keys for top-selling items | Should | Customisable quick-key grid | **Phase 1.** Include as a Should. |
| POS-018 | End-of-shift report (sales, returns, voids, cash count) | Must | Report accuracy matches transaction log | **Phase 1.** Report must balance to the centavo (AC-POS-003). |

### 4.3 Purchasing (PUR) [Verified — Vision §7.3]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| PUR-001 | Purchase order creation with supplier, items, quantities, expected delivery date | Must | PO created, editable, printable | **Phase 1.** PO must have at least one item and one supplier (BR-PUR-001). |
| PUR-002 | Track PO status: Draft, Sent, Partial, Received, Cancelled | Must | Status transitions logged | **Phase 1.** Every status transition is an audited event. |
| PUR-003 | Goods receipt against PO with variance capture | Must | Over/shortage flagged at receipt | **Phase 1.** Receipt quantity cannot exceed PO quantity by more than 10% without manager approval (BR-PUR-002). |
| PUR-004 | Update inventory and cost of goods upon receipt | Must | Inventory and COGS accurate post-receipt | **Phase 1.** Receipt updates inventory as a movement and triggers inventory valuation recalculation (BR-PUR-003). |
| PUR-005 | Direct purchases (without PO) | Should | Direct purchase workflow functional | **Phase 1.** Include as a Should. |
| PUR-006 | Track supplier invoices and payment status | Should | Invoice linked to PO; payment status visible | **Phase 1 — partial.** Invoice linkage is in scope. **Supplier payment tracking itself is a gap:** GAP-009 notes purchasing tracks POs but not actual payments to suppliers, and recommends adding accounts-payable tracking [Verified — Vision §23.2]. Whether accounts payable is in Phase 1 is a **Gate decision** — see §17. |
| PUR-007 | Purchase history and supplier performance reports | Should | Report shows on-time delivery, price variance | **Phase 1.** Include as a Should (also RPT-007). |

### 4.4 Supplier Management (SUP) [Verified — Vision §7.4]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| SUP-001 | Maintain supplier master data: name, contact, address, tax ID, payment terms, lead time | Must | All fields validated | **Phase 1.** |
| SUP-002 | Link suppliers to products they provide | Should | Product-supplier association functional | **Phase 1.** |
| SUP-003 | Track supplier purchase history and payment history | Should | History accurate across all transactions | **Phase 1 — partial.** Purchase history is in scope; payment history depends on the GAP-009 Gate decision in §4.3. |
| SUP-004 | Support supplier rating/notes | Could | Rating stored and visible | **Phase 1 — deferred.** Could priority; not in the Phase 1 baseline. |

### 4.5 Customer Management (CUS) [Verified — Vision §7.5]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| CUS-001 | Maintain customer master data: name, phone, email, address, tax ID (RFC) | Must | RFC format validated for Mexico | **Phase 1.** RFC must conform to the SAT format (Vision §10.4). Customers are a different entity from employees/users — the design explicitly distinguishes `Customer` from `User` [Verified — Phase 2 draft §6 / DB design]. |
| CUS-002 | Track customer purchase history | Must | History complete and searchable | **Phase 1.** |
| CUS-003 | Customer segmentation (VIP, regular, new) | Could | Segmentation functional | **Phase 1 — deferred.** Could priority; not in the Phase 1 baseline. |
| CUS-004 | Customer loyalty points program | Could | Points accrue and redeem correctly | **Phase 1 — deferred.** Loyalty is Phase 2 (§3.3). Not in the Phase 1 baseline. |

### 4.6 Fiado — Customer Credit (FIA) [Verified — Vision §7.6]

Fiado is in Phase 1 per Vision §16.1 item 5 and §18.1, and per SAD §4 [Verified]. The earlier reconsideration of Fiado's commercial priority (noted in the Phase 2 draft, C-F) must be resolved explicitly at the Gate — Q05 stays **[OPEN]** until then; this SRS specifies Fiado as in-scope because the verified documents place it in Phase 1, and flags the decision rather than assuming it away.

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status / Elaboration |
|---|---|---|---|---|
| FIA-001 | Allow credit sales with per-customer credit limit | Must | Limit enforced at POS | **Phase 1.** Limit enforced at POS (POS-006, BR-SAL-006). |
| FIA-002 | Track outstanding balance per customer | Must | Balance accurate to the centavo | **Phase 1.** Balance is derived/reconciled from the credit ledger (charges, payments, adjustments, reversals) — a credit account must have an auditable ledger, not just a balance field [Proposed — Phase 2 draft §12.2 / §8]. |
| FIA-003 | Support partial payments against credit balance | Must | Payment reduces balance; history kept | **Phase 1.** Payment allocation (to specific invoices or general balance) is specified in workflow §10.6. |
| FIA-004 | Generate customer statement (purchases, payments, balance) | Must | Statement printable and digital | **Phase 1.** Statements generated monthly and delivered automatically (BR-FIA-005). |
| FIA-005 | Alert when customer approaches or exceeds credit limit | Should | Alert at 80% and 100% of limit | **Phase 1.** |
| FIA-006 | Support credit terms (due dates, interest) | Could | Terms configurable per customer | **Phase 1 — partial.** Due dates and interest are configurable per customer as data fields (see defaults below); the interest *policy* itself is **[OPEN]** commercial policy, not a code default. |
| FIA-007 | Generate aging report (current, 30, 60, 90+ days overdue) | Should | Aging accurate per customer | **Phase 1.** Also RPT-008 (Fiado report, Must). |

**Fiado defaults — verified, but defaults only** [Verified — Vision §14.4 BR-FIA-002/004; DB §6.3]:
- Credit-limit default **$1,000 MXN**; maximum **$50,000 MXN** without additional documentation (BR-FIA-002).
- Interest **2% monthly (configurable) after the grace period** (BR-FIA-004).
- Grace period **30 days** (DB §6.3: `gracePeriodDays Int default 30`, line 825).
- Accounts 90+ days overdue are automatically flagged and new sales blocked unless payment is made (BR-FIA-003).

These are documentation defaults, not confirmed commercial policy [Verified — Phase 2 draft §2.1, C-J]. They must be per-business / per-customer configurable (`CustomerCreditLimit`, `BusinessSettings`) and confirmed in an approved business-rules document before launch. **Do not hard-code 2% interest as a universal rule, and do not silently apply it to every PAGATEC customer.**

### 4.7 Financial Dashboard (FIN) [Verified — Vision §7.7]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| FIN-001 | Dashboard shall display daily sales, expenses, profit | Must | Values update in real-time | **Phase 1 — partial.** Daily sales and profit are in scope. **Expense recording is a gap:** GAP-008 notes the Financial Dashboard is listed but no explicit expense recording (rent, utilities, salaries) exists outside inventory COGS [Verified — Vision §23.2]. Whether manual expense entry is in Phase 1 is a **Gate decision** — see §17. "Real-time" here means the dashboard reflects committed transactions on the device; cross-device totals are cloud-canonical (SAD §1). |
| FIN-002 | Display inventory value (cost basis) | Must | Calculation matches inventory module | **Phase 1.** Valuation method per INV-013. |
| FIN-003 | Display cash position per store | Must | Cash on hand accurate | **Phase 1.** |
| FIN-004 | Display top-selling products today / this week / this month | Should | Ranking accurate | **Phase 1.** |
| FIN-005 | Display low-stock alerts | Must | Alerts match inventory thresholds | **Phase 1.** |
| FIN-006 | Dashboard customisable per user role | Should | Owner sees everything; cashier sees sales only | **Phase 1.** Role-based visibility is an RBAC requirement (SEC-003). |

### 4.8 Reports (RPT) [Verified — Vision §7.8]

All reports in this module are Phase 1 except where noted. All reports must support date-range filtering (RPT-012, Must) and be exportable to PDF and Excel where stated (RPT-011, Should).

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] |
|---|---|---|---|
| RPT-001 | Daily sales report (items, quantities, revenue, payment methods) | Must | Report matches transaction data |
| RPT-002 | Weekly sales summary | Must | Aggregated correctly by week |
| RPT-003 | Monthly profit/loss statement | Must | Revenue − COGS − Expenses = Profit |
| RPT-004 | Inventory valuation report | Must | Valuation method applied consistently |
| RPT-005 | Low-stock report | Must | All items below threshold included |
| RPT-006 | Best/worst seller report | Should | Ranking by quantity and revenue |
| RPT-007 | Supplier purchase report | Should | PO and direct purchases included |
| RPT-008 | Customer credit (Fiado) report | Must | All credit customers with balances |
| RPT-009 | Tax report (IVA collected, IVA paid) | Must | Tax calculations accurate for SAT |
| RPT-010 | Employee performance report (sales per cashier) | Should | Shift data aggregated by employee |
| RPT-011 | All reports exportable to PDF and Excel | Should | Export formatting correct |
| RPT-012 | All reports support date-range filtering | Must | Filter applies correctly to data |

**Elaboration [Proposed]:** RPT-003 (profit/loss) depends on the GAP-008 expense-tracking Gate decision: if manual expenses are out of Phase 1, the profit/loss statement must state explicitly that it is Revenue − COGS only, and must not present itself as a complete P&L. RPT-009 (tax report) reports IVA collected/paid; it is **not** a CFDI invoice — see C-K, §9.3.

### 4.9 Analytics (ANL) [Verified — Vision §7.9]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| ANL-001 | Display sales trends (line chart, daily/weekly/monthly) | Should | Chart renders accurately | **Phase 1.** Basic analytics only; advanced analytics are Phase 2 (§3.3). |
| ANL-002 | Display inventory turnover rate | Should | Calculation: COGS / Average Inventory | **Phase 1.** |
| ANL-003 | Display profit margin by product/category | Should | Margin = (Revenue − COGS) / Revenue | **Phase 1.** |
| ANL-004 | Display seasonal demand patterns | Could | Historical comparison by month | **Phase 1 — deferred.** Could priority; advanced analytics are Phase 2. |
| ANL-005 | Support custom date range analytics | Should | Range applies to all charts | **Phase 1.** |

### 4.10 Employee Management (EMP) [Verified — Vision §7.10]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| EMP-001 | Maintain employee records: name, role, contact, hire date, status | Must | CRUD operations functional | **Phase 1.** Employees are `User` entities, distinct from `Customer` (§4.5). |
| EMP-002 | Support multiple roles per employee | Should | Role assignment functional | **Phase 1.** |
| EMP-003 | Track employee sales performance | Should | Performance linked to shift/POS transactions | **Phase 1.** Also RPT-010. |
| EMP-004 | Support employee schedule/attendance (basic) | Could | Clock-in/clock-out recorded | **Phase 1 — deferred.** Employee scheduling and attendance are Phase 2 (§3.3). Not in the Phase 1 baseline. |

**Subscriptions / billing — the C-H gap, stated here because it has no other module** [Verified — Phase 2 draft §2.1, C-H; Vision §23 GAP-001]: SAD Phase 1 includes "subscription management", and the Vision's business metrics assume a subscription model (ARPU $100/month, §22.2), but the database design contains **no dedicated Billing or Subscription tables** — only subscription attributes on Business (`subscriptionTier, subscriptionExpiresAt, maxStores, maxUsers, maxProducts`) [Verified — DB design; Phase 2 draft §3.10]. The Phase 2 draft proposes a 3-table Billing group (Subscription, SubscriptionHistory / Invoice, PaymentAttempt — see Phase 2 draft §3.10) as **[Proposed]**, not approved. **No pricing tiers, freemium boundaries, or payment collection mechanism are defined** (GAP-001, Critical — "Must Resolve Before Phase 1") [Verified — Vision §23.1]. Whether a billing lifecycle is in Phase 1, and what the pricing model is, are **Gate decisions** (Q10) — see §17. This SRS does not specify a billing implementation beyond the subscription attributes that already exist in the design.

### 4.11 Role & Permission System (SEC) [Verified — Vision §7.11]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status / Elaboration |
|---|---|---|---|---|
| SEC-001 | Support role-based access control (RBAC) | Must | Roles: Owner, Manager, Cashier, Warehouse, Accountant, Read-Only | **Phase 1.** All six roles are in scope. Note: Vision §16.1 item 7 lists only Owner, Manager, Cashier, Warehouse for Phase 1 — SEC-001 is the more specific requirement and governs; the Gate should confirm Accountant and Read-Only are in Phase 1 [Proposed — flag for Gate]. |
| SEC-002 | Allow custom permission granularity per role | Should | Per-module, per-action permissions | **Phase 1.** |
| SEC-003 | Enforce permissions at API and UI level | Must | API rejects unauthorised requests; UI hides unauthorised actions | **Phase 1.** UI hiding alone is not enforcement — the API must reject unauthorised requests (also NFR-SEC-010, SAD §6). |
| SEC-004 | Audit logging of all user actions | Must | Log: user, action, timestamp, IP, before/after values | **Phase 1.** See §4.19 (ADT). |
| SEC-005 | Require strong passwords (min 8 chars, complexity) | Must | Password policy enforced | **Phase 1.** Password storage is bcrypt/Argon2 (NFR-SEC-001). Password must be changed every 90 days for admin roles (BR-SEC-003, configurable). |
| SEC-006 | Support session timeout after inactivity | Should | Timeout configurable | **Phase 1.** Secure httpOnly cookies, 30-minute timeout (NFR-SEC-005). |

### 4.12 Multi-Warehouse (WHS) [Verified — Vision §7.12]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| WHS-001 | Support multiple warehouses per business | Must | Warehouse creation and management functional | **Phase 1.** Up to 3 warehouses per store (§2.6). |
| WHS-002 | Track stock per warehouse | Must | Stock levels warehouse-specific | **Phase 1.** `StockLevel` is per warehouse. |
| WHS-003 | Support stock transfers between warehouses with approval | Must | Transfer reduces source, increases destination; logged | **Phase 1.** Transfer requires source quantity availability at time of execution (BR-INV-007). See workflow §10.7. |
| WHS-004 | Support warehouse-specific reorder points | Could | Thresholds per warehouse | **Phase 1 — deferred.** Could priority; not in the Phase 1 baseline. |

### 4.13 Multi-Store (MST) [Verified — Vision §7.13]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| MST-001 | Support multiple stores per business account | Must | Store creation and management functional | **Phase 1.** Up to 5 stores per business account (§2.6). |
| MST-002 | Support consolidated reporting across stores | Must | Report aggregates all stores | **Phase 1.** |
| MST-003 | Support store-specific inventory, employees, and settings | Must | Data isolation per store | **Phase 1.** Store-level isolation is a tenancy requirement — a user must not read another store's data unless their role permits it (see §4.11, §5.3). |
| MST-004 | Support inter-store transfers | Should | Transfer workflow functional | **Phase 1.** See workflow §10.7. |
| MST-005 | Support store-level permissions (manager sees only their store) | Should | Data access restricted by store assignment | **Phase 1.** |

### 4.14 AI Business Assistant (AIA) [Verified — Vision §7.14]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status / Elaboration |
|---|---|---|---|---|
| AIA-001 | AI shall answer natural-language questions about business data | Should | 80% accuracy on common questions | **Phase 1 — basic queries only.** "Basic insights" per Vision §16.1 item 12 and §18.1. The 80% accuracy target is the Vision's stated criterion; how accuracy is measured is **[OPEN]** — see §11. |
| AIA-002 | AI shall generate sales predictions based on historical data | Could | Prediction within 20% of actual | **Phase 1 — deferred.** Could priority; advanced AI forecasting is Phase 2 (§3.3). |
| AIA-003 | AI shall recommend reorder quantities and timing | Could | Recommendation reduces stock-outs by 20% | **Phase 1 — deferred.** Could priority; advanced AI forecasting is Phase 2. |
| AIA-004 | AI shall identify slow-moving and dead stock | Should | Identification matches manual analysis | **Phase 1.** |
| AIA-005 | AI shall provide profit analysis insights (top margins, loss leaders) | Should | Analysis accurate per product | **Phase 1.** |
| AIA-006 | AI shall generate plain-language business summaries | Could | Summary generated weekly | **Phase 1 — deferred.** Could priority; not in the Phase 1 baseline. |
| AIA-007 | AI responses shall reference actual data points, not hallucinate | Must | Every claim traceable to database record | **Phase 1 — mandatory.** This is the controlling AI requirement. It is elaborated in §11 with the SAD §6 permission-filtering pipeline. No AI response in Phase 1 may present a claim that is not traceable to a database record. |

Whether AI is mandatory in the first release or initially limited is **[OPEN]** (Q06), and which AI provider/model is used is **[OPEN]** (Q07). The database design proposes `text-embedding-3-small` / 1,536-dim vectors for `ProductEmbedding` — that is a proposal, not a selection [Verified — Phase 2 draft §3.8 / §13, Q07].

### 4.15 Notifications (NTF) [Verified — Vision §7.15]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| NTF-001 | Send low-stock alerts | Must | Alert delivered within 1 minute of threshold breach | **Phase 1.** In-app delivery is mandatory (NTF-005). |
| NTF-002 | Send expiration alerts | Should | Alert 30/15/7 days before expiry | **Phase 1.** |
| NTF-003 | Send daily summary email to owner | Could | Email sent at configurable time | **Phase 1 — deferred.** Could priority; email service is Should in §11.2 but the daily summary itself is Could. |
| NTF-004 | Send sync conflict alerts | Should | Alert when manual resolution required | **Phase 1.** See §8. |
| NTF-005 | Support in-app, email, and WhatsApp (future) | Should | In-app mandatory; others configurable | **Phase 1.** In-app is mandatory; email is configurable; WhatsApp is explicitly future and out of Phase 1. |

### 4.16 Settings (SET) [Verified — Vision §7.16]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| SET-001 | Allow business profile configuration (name, address, tax info, logo) | Must | Profile saved and displayed on receipts | **Phase 1.** Logo storage depends on the object-storage decision (Q09, OPEN) — see INV-011. |
| SET-002 | Allow tax configuration (IVA rate, exempt categories) | Must | Tax calculation matches configuration | **Phase 1.** IVA 16% standard, 0% and exempt supported (Vision §12.3). |
| SET-003 | Allow currency and number format configuration | Must | MXN format default; configurable | **Phase 1.** MXN primary; number format 1,234.56; date DD/MM/YYYY; 24-hour time (Vision §13.3). |
| SET-004 | Allow receipt template customisation | Should | Header, footer, message customisable | **Phase 1.** |
| SET-005 | Support backup configuration (frequency, retention) | Should | Backup runs per schedule | **Phase 1.** Backup retention minimum 30 days (BKP-002, Should); the exact RPO/RTO is **[OPEN]** (Q17, GAP-005). |
| SET-006 | Support dark / light mode toggle | Should | Theme switch functional | **Phase 1.** In scope per Vision §16.1 item 15. |
| SET-007 | Support language switch (Spanish / English) | Should | All UI elements translated | **Phase 1.** Spanish (Mexico) primary, English secondary, complete UI translation (Vision §§13.3, 16.1 item 16). No untranslated strings in production (AC-GEN-005). |

### 4.17 Offline Synchronisation (OFF) [Verified — Vision §7.17]

Elaborated in full in §8. Summary of the Vision requirements:

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] |
|---|---|---|---|
| OFF-001 | Operate fully without internet connectivity | Must | All core features functional offline |
| OFF-002 | Queue transactions locally during offline periods | Must | Queue stores all transactions without loss |
| OFF-003 | Synchronise queued transactions when connectivity returns | Must | Sync completes successfully; no duplicates |
| OFF-004 | Detect and flag sync conflicts | Must | Conflict identified with clear resolution options |
| OFF-005 | Resolve simple conflicts automatically (last-write-wins with timestamp) | Should | Auto-resolution for non-critical conflicts |
| OFF-006 | Require manual resolution for complex conflicts (inventory, pricing) | Must | User prompted to choose correct value |
| OFF-007 | Display sync status (online / offline / pending) | Must | Status visible and accurate |
| OFF-008 | Support periodic background sync when online | Should | Sync interval configurable |

OFF-005 and OFF-006 can coexist with Optimistic Concurrency Control: OCC detects whether a record changed since it was read; LWW chooses a value when a conflict occurs; manual resolution lets an authorised person decide a complex conflict [Verified — ChatGPT independent audit 2026-11-25; Phase 2 draft §8.5]. **LWW must never be a universal policy** — it is allowed only for explicitly classified non-critical fields; inventory, price, credit, and settings require version-checked rejection plus human reconciliation [Proposed — Phase 2 draft §8.5]. The exact field classification is a Gate decision (§17, item 7).

### 4.18 Cloud Synchronisation (CLD) [Verified — Vision §7.18]

Elaborated in full in §8. Summary:

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] |
|---|---|---|---|
| CLD-001 | Synchronise data across all authorised devices in real-time | Must | Data consistent across devices within 5 seconds |
| CLD-002 | Encrypt all data in transit (TLS 1.3) | Must | Security audit passed |
| CLD-003 | Encrypt all data at rest (AES-256) | Must | Encryption verified |
| CLD-004 | Support automatic cloud backup | Must | Daily backup minimum |
| CLD-005 | Support data restore from backup | Must | Restore completes within 4 hours |
| CLD-006 | Maintain data integrity during sync (checksums, transaction logs) | Must | Zero corruption incidents in testing |

### 4.19 Audit Logs (ADT) [Verified — Vision §7.19]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status / Elaboration |
|---|---|---|---|---|
| ADT-001 | Log every create, update, delete operation | Must | Log entry for every mutation | **Phase 1.** |
| ADT-002 | Audit log shall include: timestamp, user, action, entity, old value, new value, IP address | Must | All fields populated | **Phase 1.** |
| ADT-003 | Audit logs shall be immutable and non-deletable by users | Must | Even admins cannot delete logs | **Phase 1 — mandatory.** BR-SEC-002: audit logs cannot be modified or deleted by any user, including owners [Verified — Vision §14.5]. The `AuditLog` table has no `deletedAt` and no `updatedAt` in the Phase 2 draft (conflict C-E). |
| ADT-004 | Audit logs shall be searchable and filterable | Should | Search by user, date, entity type | **Phase 1.** |
| ADT-005 | Audit logs shall be exportable | Could | CSV/PDF export functional | **Phase 1 — deferred.** Could priority; not in the Phase 1 baseline. |

### 4.20 Backups (BKP) [Verified — Vision §7.20]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| BKP-001 | Perform automatic daily cloud backup | Must | Backup completes without error | **Phase 1.** |
| BKP-002 | Retain backups for minimum 30 days | Should | Retention policy enforced | **Phase 1.** |
| BKP-003 | Allow manual backup trigger | Should | Manual backup functional | **Phase 1.** |
| BKP-004 | Allow backup restore to specific point in time | Should | Restore accurate to selected backup | **Phase 1.** Restore within 4 hours (CLD-005). The formal disaster-recovery plan, RPO, and RTO are **not defined** (GAP-005, Critical — Vision §23.1); Q17 stays **[OPEN]**. |

### 4.21 Import / Export (IEX) [Verified — Vision §7.21]

| ID | Requirement [Verified] | Priority | Acceptance criterion [Verified] | Phase 1 status |
|---|---|---|---|---|
| IEX-001 | Support bulk product import via CSV/Excel | Should | Template provided; validation errors reported | **Phase 1.** Also INV-014. |
| IEX-002 | Support bulk product export via CSV/Excel/PDF | Should | Export matches current data | **Phase 1.** Also INV-015. |
| IEX-003 | Support customer/supplier import/export | Could | Same format as products | **Phase 1 — deferred.** Could priority; not in the Phase 1 baseline. |

The data-migration strategy for customers moving from existing systems (Excel, old POS, notebooks) is **not defined** (GAP-003, Critical — Vision §23.1). Whether a migration service is offered, or self-service import only, is a **Gate decision** — see §17.

---

## 5. Non-functional requirements [Verified — Vision §8]

All NFRs below are the Vision's own IDs, wording, and targets [Verified — Vision §8.1–8.7]. Where the Vision contradicts itself, the contradiction is flagged, not silently resolved.

### 5.1 Performance [Verified — Vision §8.1]

| ID | Requirement | Target |
|---|---|---|
| NFR-PER-001 | Initial application load time | < 3 seconds on 4G connection |
| NFR-PER-002 | Screen-to-screen navigation | < 1 second |
| NFR-PER-003 | Search response time | < 500 ms for 10,000 products |
| NFR-PER-004 | Report generation (daily) | < 5 seconds |
| NFR-PER-005 | Report generation (monthly) | < 30 seconds |
| NFR-PER-006 | POS transaction processing | < 2 seconds per item |
| NFR-PER-007 | Sync operation (100 transactions) | < 10 seconds |
| NFR-PER-008 | Concurrent users per store | Minimum 20 simultaneous |
| NFR-PER-009 | Database query response (p95) | < 200 ms |

**Elaboration [Proposed]:** performance is measured on the target hardware in AC-GEN-004 (minimum: 4GB RAM Android tablet, Chrome browser). The load target (NFR-PER-001) and the technical objective TO-001 (sub-2-second load for all primary screens, Vision §3.2) are not identical — TO-001 is stricter for primary screens. Both are recorded; the Gate should confirm which is the contractual target for primary screens [Proposed — flag for Gate].

### 5.2 Availability and reliability [Verified — Vision §8.2]

| ID | Requirement | Target |
|---|---|---|
| NFR-AVL-001 | Cloud sync service uptime | **99.9% (max 8.76 hours downtime/year)** |
| NFR-AVL-002 | Offline functionality | 100% of core features |
| NFR-AVL-003 | Data loss tolerance | Zero unrecoverable transactions |
| NFR-AVL-004 | Mean Time Between Failures (MTBF) | > 720 hours |
| NFR-AVL-005 | Mean Time To Recovery (MTTR) | < 4 hours |

**Conflict C-L — uptime inconsistency [OPEN]** [Verified — Phase 2 draft §2.1, C-L]: Vision §3.2 TO-005 says **99.99%** uptime for cloud synchronisation services, while §8.2 NFR-AVL-001 says **99.9%**, and §22.3 says uptime 99.9% but sync success rate 99.99%. **This SRS uses 99.9% as the NFR target (NFR-AVL-001) until clarified, and does not promise 99.99% uptime.** The sync *success rate* target of 99.99% (§22.3) is a different metric from uptime and is retained separately in §13. The Gate must clarify which figure is the contractual target (Gate item 9, §17).

### 5.3 Security [Verified — Vision §8.3; SAD §8]

| ID | Requirement | Standard |
|---|---|---|
| NFR-SEC-001 | Password storage | bcrypt/Argon2 hashing |
| NFR-SEC-002 | Data in transit encryption | TLS 1.3 minimum |
| NFR-SEC-003 | Data at rest encryption | AES-256 |
| NFR-SEC-004 | API authentication | JWT with refresh token rotation |
| NFR-SEC-005 | Session management | Secure httpOnly cookies; 30-min timeout |
| NFR-SEC-006 | Input validation | OWASP Top 10 mitigation |
| NFR-SEC-007 | SQL injection prevention | Parameterised queries exclusively |
| NFR-SEC-008 | XSS prevention | Content Security Policy; output encoding |
| NFR-SEC-009 | Rate limiting | 100 requests/minute per IP; 1,000 per user |
| NFR-SEC-010 | Role-based access enforcement | Every endpoint validates permissions |

SAD §8 adds, as mandatory before production launch: HMAC SHA-256 sync signatures, device identification, payload integrity verification; JWT secrets rotated every 90 days, API credentials rotated regularly, no secrets in source control; prompt-injection filtering, structured JSON outputs, maximum token limits, AI audit logging; CSP with restricted script sources, disabled unsafe inline execution, trusted API origins only [Verified — SAD §8]. These are incorporated in §9.

### 5.4 Scalability [Verified — Vision §8.4]

| ID | Requirement | Target |
|---|---|---|
| NFR-SCL-001 | Products per business | 100,000+ |
| NFR-SCL-002 | Transactions per day per store | 10,000+ |
| NFR-SCL-003 | Concurrent stores per business | 50+ |
| NFR-SCL-004 | Users per business | 200+ |
| NFR-SCL-005 | Historical data retention | 7 years minimum |

**Note [Proposed]:** NFR-SCL-003 (50+ concurrent stores) is a scalability target, not a Phase 1 scope limit — Phase 1 scope is up to 5 stores per business (§2.6). The system must not be designed in a way that makes 50 stores impossible later, but Phase 1 acceptance tests the 5-store limit.

### 5.5 Usability [Verified — Vision §8.5]

| ID | Requirement | Target |
|---|---|---|
| NFR-USA-001 | New user time-to-first-sale | < 15 minutes |
| NFR-USA-002 | Task completion rate (core workflows) | > 95% without documentation |
| NFR-USA-003 | Interface language | Spanish (Mexico) primary; English secondary |
| NFR-USA-004 | Touch target size | Minimum 44×44 px |
| NFR-USA-005 | Color contrast | WCAG AA compliance |
| NFR-USA-006 | Screen reader support | WCAG 2.1 Level AA |
| NFR-USA-007 | Keyboard navigation | All features accessible without mouse |

### 5.6 Maintainability [Verified — Vision §8.6]

| ID | Requirement | Target |
|---|---|---|
| NFR-MNT-001 | Code test coverage | > 80% |
| NFR-MNT-002 | API documentation | OpenAPI/Swagger; 100% endpoint coverage |
| NFR-MNT-003 | Deployment time | < 30 minutes |
| NFR-MNT-004 | Feature toggle system | All major features toggleable |

### 5.7 Compliance [Verified — Vision §8.7]

| ID | Requirement | Jurisdiction |
|---|---|---|
| NFR-CMP-001 | CFDI (Comprobante Fiscal Digital por Internet) compliance | Mexico |
| NFR-CMP-002 | SAT (Servicio de Administración Tributaria) reporting | Mexico |
| NFR-CMP-003 | IVA (Impuesto al Valor Agregado) calculation accuracy | Mexico |
| NFR-CMP-004 | Data privacy (LFPDPPP) | Mexico |
| NFR-CMP-005 | Controlled substance tracking (pharmacies) | Mexico — COFEPRIS |
| NFR-CMP-006 | LGPD readiness | Brazil (future) |
| NFR-CMP-007 | GDPR readiness | EU customers (future) |

NFR-CMP-006/007 are future readiness only — they require the architecture to support consent management and data-subject rights later, not a Phase 1 implementation [Verified — Vision §8.7]. NFR-CMP-001/002 are the subject of conflict C-K — see §9.3. **Compliance requirements NFR-CMP-001–005 are Phase 1 requirements in the Vision, but C-K means they cannot all be satisfied in Phase 1 as currently scoped; the Gate must decide.**

---

## 6. Business rules [Verified — Vision §14]

Business rules are stated as the Vision states them. They are normative in Phase 1 unless the Gate explicitly changes them. Where a rule conflicts with another part of the Vision, the conflict is flagged.

### 6.1 Inventory rules [Verified — Vision §14.1]

| ID | Rule |
|---|---|
| BR-INV-001 | A product must have a unique SKU within a business. |
| BR-INV-002 | A product barcode must be unique within a business unless explicitly configured as a variant. |
| BR-INV-003 | Cost price cannot exceed sale price unless explicitly marked as "loss leader" with manager approval. |
| BR-INV-004 | Inventory adjustments require a reason code; "Other" requires text explanation. |
| BR-INV-005 | Negative stock is blocked by default; can be enabled per product with audit logging. |
| BR-INV-006 | Expired products cannot be sold through POS; system blocks with warning. |
| BR-INV-007 | Transfer between warehouses requires source quantity availability at time of execution. |
| BR-INV-008 | Physical stock counts must be approved by a manager if variance exceeds 5% or $1,000 MXN. |

### 6.2 Sales rules [Verified — Vision §14.2]

| ID | Rule |
|---|---|
| BR-SAL-001 | Every sale must have at least one item. |
| BR-SAL-002 | Sale total must equal sum of item prices minus discounts plus taxes. |
| BR-SAL-003 | Cashier cannot void their own transaction; requires supervisor PIN. |
| BR-SAL-004 | Returns must reference original transaction; return quantity cannot exceed original quantity. |
| BR-SAL-005 | Discounts exceeding 20% require manager approval. |
| BR-SAL-006 | Fiado sales blocked if customer credit limit would be exceeded. |
| BR-SAL-007 | Mixed payment must sum exactly to sale total; no partial payments unless Fiado. |
| BR-SAL-008 | End-of-shift cash variance exceeding $200 MXN requires manager investigation. |

### 6.3 Purchasing rules [Verified — Vision §14.3]

| ID | Rule |
|---|---|
| BR-PUR-001 | Purchase order must have at least one item and one supplier. |
| BR-PUR-002 | Goods receipt quantity cannot exceed PO quantity by more than 10% without manager approval. |
| BR-PUR-003 | Received cost price update triggers inventory valuation recalculation. |
| BR-PUR-004 | Supplier invoice must match received goods within variance tolerance before payment approval. |

**Note [Proposed]:** BR-PUR-004 assumes a supplier-payment approval flow; that flow depends on the GAP-009 accounts-payable Gate decision (§4.3). If accounts payable is out of Phase 1, BR-PUR-004 must be restated at the Gate to cover invoice matching only, without a payment-approval step.

### 6.4 Credit (Fiado) rules [Verified — Vision §14.4]

| ID | Rule |
|---|---|
| BR-FIA-001 | New credit customers require owner/manager approval and signed agreement record. |
| BR-FIA-002 | Credit limit default is $1,000 MXN; maximum $50,000 MXN without additional documentation. |
| BR-FIA-003 | Accounts overdue 90+ days automatically flagged; new sales blocked unless payment made. |
| BR-FIA-004 | Interest on overdue accounts calculated at 2% monthly (configurable) after grace period. |
| BR-FIA-005 | Customer statements generated monthly and delivered automatically. |

See §4.6 for the defaults-only warning. BR-FIA-002/004 are defaults, not confirmed commercial policy.

### 6.5 Security rules [Verified — Vision §14.5]

| ID | Rule |
|---|---|
| BR-SEC-001 | Owner role cannot be deleted; only transferred. |
| BR-SEC-002 | Audit logs cannot be modified or deleted by any user, including owners. |
| BR-SEC-003 | Password must be changed every 90 days for admin roles (configurable). |
| BR-SEC-004 | Inactive user accounts (90 days) automatically disabled. |
| BR-SEC-005 | All financial data exports require owner or accountant role. |

**Conflict C-E [Verified — Phase 2 draft §2.1, C-E]:** the database design gives every table a `deletedAt` (soft delete), but BR-SEC-002 and Vision §6.1 ("Nothing is ever truly deleted") require audit logs to be immutable and non-deletable. Resolution in this SRS [Proposed — Phase 2 draft §2.1, C-E]: entity-specific lifecycle — soft delete for master data (Product, Customer, Supplier, User); **no soft-delete-as-edit** for `AuditLog`, completed `POSTransaction` / `Payment` / `CreditTransaction` / `InventoryMovement` — correct them only through traceable reversal/adjustment records. `AuditLog` has no `deletedAt` and no `updatedAt`.

---

## 7. Data requirements [Verified — Vision §10]

### 7.1 Data entities (conceptual) [Verified — Vision §10.1]

The system shall manage these conceptual data domains: (1) Business Entity — company profile, tax information, branding, subscription status; (2) User Entity — employees, roles, permissions, authentication credentials, session data; (3) Product Entity — catalog, variants, barcodes, pricing, costs, taxes, images, units of measure; (4) Inventory Entity — stock levels, warehouse locations, movements, adjustments, transfers, counts; (5) Transaction Entity — sales, purchases, returns, payments, credit transactions; (6) Customer Entity — profiles, credit limits, balances, purchase history, loyalty data; (7) Supplier Entity — profiles, payment terms, purchase history, performance metrics; (8) Financial Entity — accounts, tax records, shift reconciliations, expense categories; (9) Audit Entity — action logs, change history, system events, security incidents; (10) AI Entity — queries, responses, data references, feedback, model configurations.

The detailed table design is the Phase 2 draft §3 (48 core / 5 deferred / 3 confirm + OutboxEvent + 3 Billing, 81 tables in the full design) [Proposed — not approved]. This SRS does not repeat the table definitions; it specifies what the data must satisfy.

### 7.2 Data volume estimates (per business, Year 1) [Verified — Vision §10.2]

| Entity | Estimated volume | Growth |
|---|---|---|
| Products | 1,000–50,000 | Linear with business growth |
| Transactions | 50,000–500,000 | Daily operations |
| Customers | 500–20,000 | Depends on loyalty program adoption |
| Audit logs | 500,000–5,000,000 | High; every action logged |
| AI interactions | 1,000–10,000 | Depends on feature adoption |

### 7.3 Data retention policies [Verified — Vision §10.3]

| Data type | Retention | Rationale |
|---|---|---|
| Transaction data | 7 years | Tax compliance (SAT requirement) |
| Audit logs | 7 years | Legal traceability |
| Customer data | Duration of relationship + 2 years | LFPDPPP compliance |
| Backup data | 30 days minimum | Disaster recovery |
| Session logs | 90 days | Security analysis |
| AI query logs | 1 year | Model improvement and debugging |

What data must remain available after subscription expiration is **[OPEN]** (Q16) — the retention table above does not answer it.

### 7.4 Data quality requirements [Verified — Vision §10.4]

1. **Accuracy:** financial calculations must be accurate to 2 decimal places (MXN centavos). Money is Decimal `NUMERIC(19,4)` in the database, never float [Proposed — Phase 2 draft §5].
2. **Consistency:** inventory quantities must never be negative unless explicitly configured to allow it (BR-INV-005).
3. **Completeness:** every transaction must have timestamp, user, items, amounts, payment method.
4. **Timeliness:** sync latency must not exceed 5 seconds for online operations (CLD-001).
5. **Validity:** barcodes must conform to EAN-13 or UPC-A; RFC must conform to the SAT format.

---

## 8. Synchronisation, conflict resolution, and the Outbox [Verified — SAD §§1–3, 7; Vision §§7.17–7.18, 9.8; Proposed — Phase 2 draft §§4, 8]

This section is the most technically sensitive part of Phase 1. It is specified here because the database and sync contracts determine whether the application can safely operate offline — the Phase 2 draft's stated reason for doing the database work before any UI [Verified — Phase 2 draft §15].

### 8.1 Authority model [Verified — SAD §1]

- The **device** has temporary operational authority for recording sales, payments, inventory movements, and offline Fiado transactions. Offline transactions are **provisional business events** until confirmed by cloud reconciliation.
- The **cloud PostgreSQL database** is the canonical business state. It determines final inventory availability, cross-device conflicts, credit-limit enforcement, and business reporting totals.
- The user must be able to distinguish, for every transaction: saved locally, pending synchronisation, synchronised, failed, conflict requiring attention [Proposed — ChatGPT handoff §10.6, adopted here]. The sync status display (OFF-007) must show exactly these states, not a single "synced" flag.

### 8.2 Optimistic Concurrency Control [Verified — SAD §2]

Every synchronised entity carries `id, version, updatedAt, deviceId, lastModifiedBy` [Verified — SAD §2]. The protocol is:

```
Client sends version N
Server checks current version
If server version = N:  apply update, increment version
If server version > N:  create conflict record, require reconciliation
```

**Elaboration [Proposed — Phase 2 draft §8]:** every locally generated operation has a globally unique operation ID (idempotency); the system must not rely on device clocks alone for global ordering — an explicit event-ordering strategy is required and is **[OPEN]** as a design detail within the approved OCC approach (it must be specified in the sync protocol deliverable, not invented here). Duplicate delivery of the same operation ID / idempotency key must be applied once.

### 8.3 Transactional Outbox [Verified — SAD §3; Proposed — Phase 2 draft §4]

A business transaction and its corresponding synchronisation event must be persisted atomically in one PostgreSQL transaction; a background worker (Event Worker) then dispatches to inventory, AI, and notifications [Verified — SAD §3]. Events survive server restart; no lost notifications; supports future horizontal scaling; creates reliable audit history [Verified — SAD §3].

The 77-table design has **no OutboxEvent table** [Verified — Phase 2 draft §2.1, C-C]. The Phase 2 draft adds one as **[Proposed]**: `OutboxEvent` with UUID, businessId, aggregate/entity type and ID, event type, JSONB payload, entity version, per-business sequence, unique idempotency key, created/published timestamps, processing status, retry count, last error; dispatcher and 90-day retention are **[Proposed]**, retention itself **[OPEN]** (see Phase 2 draft §4.3). **Do not simply rename SyncQueue to Outbox** — they serve distinct responsibilities: the Outbox records committed domain events; a synchronisation queue manages delivery and retry work; conflict records capture unresolved state divergence [Verified — ChatGPT independent audit 2026-11-25, §4.3].

### 8.4 Conflict classification [Proposed — Phase 2 draft §8.5, building on OFF-005/006 and SAD §2]

| Conflict | Treatment [Proposed] |
|---|---|
| Duplicate delivery | Idempotent acceptance (apply once) |
| Product description change (non-critical field) | Automatic reconciliation under defined rules — the only case where LWW (OFF-005) may apply |
| Product price change | Version-checked rejection + authorised / manual resolution (OFF-006) |
| Inventory quantity | Movement-based reconciliation — preserve all valid transactions, reconcile stock movement history; negative inventory creates a reconciliation event requiring manager approval (SAD §1 Inventory Conflict Rule) |
| Simultaneous sales of the same stock | Preserve both sales and reconcile stock; never silently delete a valid transaction |
| Customer credit | Transaction ledger, not simple overwrite |
| Business settings | Field-level or version-based reconciliation |

The exact field classification (which fields are "non-critical" for LWW) is a **Gate decision** (§17, item 7). Until it is approved, the implementation must treat every conflict as requiring reconciliation unless the field is on an approved LWW list — i.e. fail closed, not open [Proposed].

### 8.5 Inventory — the hard case [Proposed — Phase 2 draft §8.4]

Stock is a movement ledger, not a single overwritable number. `StockLevel` is a reconciled balance/projection of `InventoryMovement` records. A simple last-write-wins update to `StockLevel` could erase a legitimate movement and must not be used. When multiple offline devices modify the same inventory item: all valid transactions are preserved; the server reconciles stock movement history; negative inventory situations create a reconciliation event; manager approval is required for correction [Verified — SAD §1, Inventory Conflict Rule].

**Mandatory test (from the Phase 2 draft, §12):** two devices sell the final unit of the same product while disconnected. Both sales must be preserved, the stock discrepancy must be recorded, and no data may be silently lost. This test must pass before the Phase 1 Gate (see §13).

### 8.6 Device identity and recovery [Verified — SAD §7; Vision §9.8]

- Every authorised installation receives a persistent device identifier [Proposed — Phase 2 draft §8.1]. `Device.deviceId` is described in the design as a unique hardware identifier, but hardware identifiers are not necessarily stable or safe authentication credentials — device identity and device authentication must be specified separately in the sync protocol [Proposed — ChatGPT handoff §12.4, adopted here].
- **Lost-device recovery** [Verified — SAD §7]: user authenticates on a replacement device; cloud verifies business permissions; device downloads the authorised business dataset; pending SyncQueue operations are restored; transaction hashes are validated; synchronisation resumes. If the device contained unsynchronised transactions, recovery depends on an exported local backup, and conflicts enter the reconciliation workflow.
- **Offline sync recovery workflow** [Verified — Vision §9.8]: on reconnection, the system uploads the local queue, downloads updates from other devices, compares timestamps and versions, auto-resolves simple conflicts (different fields, LWW), flags complex conflicts (same inventory item, same field) for the user, and resumes from a checkpoint without duplicate uploads if sync is interrupted.

### 8.7 Local / cloud schema relationship [Proposed — Phase 2 draft §9]

- **Cloud (canonical):** the full reconciled schema (Phase 2 draft §3), PostgreSQL.
- **Local (device):** a subset for offline operations only — products, variants, barcodes, stores, warehouses, stock snapshots, sales, sale items, payments, customers, Fiado transactions (if retained in Phase 1), device identity, pending operations, synchronisation checkpoints, conflict records, outbox events. The local model does not need to replicate every cloud administrative table, but it must support every operation promised to work offline.
- **Local persistence technology:** **[OPEN]** (Q02/Q03). The Database Design Document's approval gate lists, as an unchecked item, "[ ] Dual DB: PostgreSQL (cloud) + SQLite (local via IndexedDB)" (DB line 1536) [Verified — Beacon extraction 2026-11-25, correcting the ChatGPT audit's claim that the phrase appears in neither txt file; it appears exactly once, in the DB doc, and 0 times in the Vision txt]. That gate item is **not an approved decision**. The Phase 2 draft proposes IndexedDB/Dexie as the initial candidate [Proposed]. The Gate must choose one explicit local persistence strategy and document it; different modules must not independently choose IndexedDB, SQLite, and localStorage [Proposed — ChatGPT handoff §4.4].
- **Local schema versioning strategy:** **[OPEN]** (Q15).

---

## 9. Security, compliance, and the CFDI decision

### 9.1 Authentication and authorisation [Verified — Vision §12.1; SAD §8; NFR-SEC-001–010]

- Passwords: minimum 8 characters with complexity (SEC-005), stored bcrypt/Argon2 (NFR-SEC-001), changed every 90 days for admin roles (BR-SEC-003, configurable).
- Sessions: JWT with refresh token rotation (NFR-SEC-004), secure httpOnly cookies, 30-minute timeout (NFR-SEC-005). JWT secrets rotated every 90 days (SAD §8).
- Account lockout after 5 failed attempts, 15-minute cooldown; password reset via verified email only [Verified — Vision §12.1].
- RBAC on every endpoint (NFR-SEC-010, SEC-003); tenant isolation on every business-data query — never trust a client-supplied `businessId` as proof of authorisation; validate tenant membership and permissions on the server; store-level permissions enforced [Proposed — ChatGPT handoff §10.5, adopted here].
- Inactive accounts (90 days) automatically disabled (BR-SEC-004). Owner role cannot be deleted, only transferred (BR-SEC-001).
- **MFA:** Vision §12.1 lists MFA support for owner and manager roles, but SAD §4 defers MFA to Phase 2 [Verified]. MFA is therefore **[OPEN]** for Phase 1 — see §17. Biometric authentication and passwordless magic-link login are likewise **[OPEN]** / future (Vision §12.1).
- The complete authentication design (password hashing parameters, token lifecycle, refresh rotation, session revocation, rate limiting, account recovery, MFA recovery, permission inheritance, tenant authorisation, device authorisation) is **not specified** by the presence of a Session table [Verified — ChatGPT handoff §12.3]. It must be specified in a security model deliverable before implementation; this SRS records the requirement, not the design.

### 9.2 Data protection [Verified — Vision §12.2]

- All customer personal data encrypted at rest (AES-256, NFR-SEC-003).
- Right to data export (portability) per LFPDPPP; right to data deletion **with audit-log retention exception** (BR-SEC-002); data processing consent tracking for marketing; no sale of customer data to third parties (explicit policy) [Verified — Vision §12.2].
- Data breach notification within 72 hours (LFPDPPP) [Verified — Vision §12.3].
- Formal privacy and retention policies: **[OPEN]** (Q18).

### 9.3 Regulatory compliance and conflict C-K — the CFDI Gate decision [Verified — Vision §§11.2, 12.3, 16, 18; SAD §4]

**The conflict, stated plainly** [Verified — Phase 2 draft §2.1, C-K]:
- Vision §11.2 (External Integrations, Current Phase) lists **SAT/CFDI — electronic invoicing compliance — as Must (Mexico)**.
- Vision §12.3 requires the system to **generate CFDI 4.0 compliant invoices** with correct XML structure, digital stamp, and QR code, and to calculate IVA automatically at 16% (standard) with support for 0% and exempt categories.
- Vision §8.7 (NFR-CMP-001/002) likewise requires CFDI compliance and SAT reporting for Mexico.
- **But** Vision §16.2 and §18.2 and SAD §4 defer **CFDI integration to Phase 2**, along with payment gateways, native mobile apps, and MFA.
- Assumption A-004 (SAT/CFDI requirements will remain stable enough to build against during Phase 1) and constraint C-004 (must comply with Mexican tax and data privacy laws before launch) both assume compliance is achievable in Phase 1.

**Consequence [Proposed — Phase 2 draft C-K]: a Phase 1 MVP without CFDI cannot issue legal electronic invoices in Mexico.** The system can still record sales, calculate IVA (RPT-009, SET-002), and issue receipts (POS-007) — but a receipt is not a CFDI invoice, and this SRS must not describe receipts as invoices.

**Gate decision Q11 [OPEN] — the Gate must choose explicitly, and the choice must be recorded in writing:**
- **Option A:** move CFDI into Phase 1 (changes scope, timeline, and cost; requires a PAC integration, certificates, and regulatory workflows, none of which are specified in the current documents — they must be separately specified [Verified — Phase 2 draft §3.7]).
- **Option B:** launch Phase 1 without invoicing, with that limitation stated explicitly to customers and in the product documentation (the MVP records sales and taxes but does not issue CFDI invoices).

**This SRS does not resolve C-K by assumption.** Until the Gate decides, no implementation work on invoicing may be treated as in scope, and no customer-facing claim may promise CFDI invoicing in Phase 1.

### 9.4 Other compliance [Verified — Vision §§8.7, 12.3]

- IVA: automatic calculation at 16% standard, 0% and exempt supported (Vision §12.3, SET-002).
- LFPDPPP: privacy notice, consent management, breach notification within 72 hours (Vision §12.3, §9.2 above).
- COFEPRIS (pharmacies): controlled-substance tracking with daily reporting capability (Vision §12.3, NFR-CMP-005); batch/lot traceability (INV-018).
- LGPD (Brazil) / GDPR (EU): architecture must support consent management and data-subject rights in future; not a Phase 1 implementation (Vision §12.3, NFR-CMP-006/007).

---

## 10. Workflows — use cases [Verified — Vision §9]

Each workflow below is the Vision's own workflow (§9.1–9.10), restated as a use case with actors, trigger, steps, post-conditions, and exceptions. They are acceptance-level descriptions, not UI designs.

### 10.1 Product onboarding [Verified — Vision §9.1]
Actor: Owner/Manager. Trigger: new product needs to be sold. Steps: Inventory → Products → New; enter name, select category; enter/scan barcode (auto-lookup if it exists); set unit, cost, sale price, tax rate; set min/max stock; enter initial quantity and warehouse; upload image (optional); save — system generates SKU if not provided. Post-condition: product available in POS and inventory reports. Exception: duplicate barcode → prompt to merge or differentiate.

### 10.2 Purchase order [Verified — Vision §9.2]
Actor: Manager/Warehouse. Trigger: inventory below reorder point or scheduled restock. Steps: low-stock alert or manual PO; select supplier; add products (system suggests quantities based on sales velocity); review quantities, prices, delivery date; submit (status Draft); send to supplier (print/email/export); on delivery, open PO → Receive Goods; scan/enter received quantities, system flags variances; confirm receipt — inventory updated, COGS recalculated; supplier invoice recorded and linked. Post-condition: stock increased, PO = Received, payable recorded. Exception: partial delivery → PO = Partial, remaining items tracked.

### 10.3 Sales / POS [Verified — Vision §9.3]
Actor: Cashier. Trigger: customer presents items. Steps: open POS; scan/search each item; system shows name, price, running total; apply discounts if authorised; select payment (Cash, Card, Transfer, Mixed, Fiado); if Fiado, select customer and validate credit limit; if Mixed, enter amounts per method; system calculates change (cash); confirm sale; receipt prints/emails; inventory decremented in real-time. Post-condition: sale, inventory reduction, payment, and receipt all recorded. Exception: item out of stock → warning displayed; cashier decides to proceed or cancel.

### 10.4 Inventory adjustment [Verified — Vision §9.4]
Actor: Warehouse Manager. Trigger: physical count differs from system; damage; theft; expiration. Steps: Inventory → Adjustments → New; select product and warehouse; enter actual vs system quantity; select reason (Physical Count, Damage, Theft, Expiration, Other); enter notes; submit — manager approval required if variance > threshold; on approval, inventory updated, audit log records old/new values. Post-condition: inventory corrected, reason documented, audit trail complete. Exception: negative stock would result → system blocks or warns per configuration (BR-INV-005).

### 10.5 Customer credit (Fiado) [Verified — Vision §9.5]
Actor: Cashier + Customer. Trigger: customer requests credit purchase. Steps: at POS select Fiado; search/select customer; system shows current balance and limit; if sale + balance ≤ limit, proceed; if it exceeds the limit, system blocks and cashier informs the customer; complete sale, balance updated; customer signs digital or paper receipt. Post-condition: sale recorded as credit; customer balance increased. Exception: new customer without a credit account → prompt to create with owner approval (BR-FIA-001).

### 10.6 Credit payment (Fiado collection) [Verified — Vision §9.6]
Actor: Cashier/Manager. Trigger: customer pays against credit balance. Steps: Customers → [Customer] → Make Payment; display balance and outstanding invoices; enter amount and method (cash, transfer, card); apply payment to specific invoices or general balance; record payment, receipt generated; balance updated, aging recalculated. Post-condition: payment recorded, balance reduced, receipt issued.

### 10.7 Multi-store transfer [Verified — Vision §9.7]
Actor: Warehouse Manager / Store Manager. Trigger: Store A has excess stock, Store B has a shortage. Steps: Inventory → Transfers → New; select source and destination; add products and quantities; system validates source stock; submit (status Pending); destination manager approves/rejects; if approved, source reduced, destination increased; transfer logged, audit trail complete. Post-condition: stock rebalanced, both locations updated. Exception: source stock insufficient at approval time → transfer blocked, notification sent.

### 10.8 Offline sync recovery [Verified — Vision §9.8]
Actor: System (automatic) + User (if conflict). Trigger: device reconnects after offline period. Steps: detect restoration; upload local queue; download updates from other devices; compare timestamps and versions; auto-resolve simple conflicts (different fields, LWW); flag complex conflicts (same inventory item, same field) for the user; user reviews local vs server values, selects the correct value, system applies and logs the resolution; sync status returns to Synchronised. Post-condition: all devices consistent, no data loss. Exception: sync interrupted → resume from checkpoint, no duplicate uploads. See §8.

### 10.9 End-of-shift [Verified — Vision §9.9]
Actor: Cashier. Trigger: shift end. Steps: select Close Shift; system shows start time, total sales, returns, voids, expected cash; cashier counts physical cash and enters the amount; system calculates variance; if within tolerance, shift closes and report is generated; if outside tolerance, manager approval required; report saved, new shift can open. Post-condition: shift closed, cash accountability recorded. Cash variance exceeding $200 MXN requires manager investigation (BR-SAL-008).

### 10.10 AI insight generation [Verified — Vision §9.10]
Actor: Business Owner / Manager. Trigger: user asks a question or scheduled insight generation. Steps: user asks a natural-language question (e.g. "What should I reorder this week?"); AI parses intent and identifies relevant data entities; AI queries the database for historical sales, current stock, lead times; AI applies prediction model or analytical logic; AI generates a response with specific data references; response displayed with confidence level and data sources; user can drill down into supporting data or refine the question. Post-condition: insight delivered, user makes an informed decision. Exception: insufficient data → AI responds "I need more data to answer accurately" and specifies what is missing. See §11 — in Phase 1, prediction/reorder (AIA-002/003) are deferred; the workflow applies to the in-scope AI requirements (AIA-001, AIA-004, AIA-005, AIA-007).

---

## 11. AI Business Assistant — elaboration [Verified — Vision §7.14; SAD §6]

### 11.1 What Phase 1 AI must do

In Phase 1, the AI assistant answers natural-language questions about business data (AIA-001, Should, 80% accuracy on common questions), identifies slow-moving and dead stock (AIA-004, Should), and provides profit analysis (AIA-005, Should) — all subject to the controlling requirement AIA-007 (Must): **AI responses shall reference actual data points, not hallucinate; every claim traceable to a database record** [Verified — Vision §7.14]. Sales predictions (AIA-002, Could) and reorder recommendations (AIA-003, Could) are deferred to Phase 2; plain-language weekly summaries (AIA-006, Could) are deferred.

### 11.2 The mandatory pipeline [Verified — SAD §6]

Every AI interaction must pass through: Identity Verification → Permission Validation → Business Data Retrieval → RAG Context Filtering → LLM Processing → Response Validation → User Response. Rules: AI never bypasses user permissions; AI never generates unrestricted database queries; sensitive fields require explicit authorisation; financial recommendations must cite source data [Verified — SAD §6].

**Elaboration [Proposed]:** the permission filter must run before retrieval, not after generation — an AI response must never contain data the asking user is not permitted to see, even if the model generated it. AI audit logging is mandatory (SAD §8): every AI query, its data sources, and its response must be logged (`AIQuery`, `AIInsight` tables in the design) and retained per §7.3 (AI query logs, 1 year).

### 11.3 What is NOT specified [OPEN]

- Which AI provider/model is used (Q07) — the design's `text-embedding-3-small` / 1,536-dim proposal is not a selection.
- Whether AI is mandatory in the first release or initially limited (Q06).
- How the 80% accuracy target (AIA-001) is measured, and what happens when the AI cannot answer accurately (the Vision's workflow says it must state what data is missing — §10.10 — but the measurement method is not specified).
- The AI security requirements that the independent ChatGPT audit listed as OPEN pending SAD extraction — tenant isolation in AI queries, prompt-injection protection, unauthorised tool execution, access to confidential data, AI-generated financial actions, human approval requirements, RAG retrieval boundaries, logging/retention of AI conversations, model-provider data handling, output validation and hallucination controls — are now partly verified in SAD §6 (permission filtering, no unrestricted queries, response validation, audit logging), but the detailed controls must still be specified in a security/AI governance document before implementation [Proposed — ChatGPT audit §8, adopted here].

---

## 12. Interfaces

### 12.1 Internal integrations [Verified — Vision §11.1]

| Integration | Description | Priority |
|---|---|---|
| POS ↔ Inventory | Real-time stock deduction on sale | Must |
| POS ↔ Customer Credit | Balance validation and update | Must |
| Inventory ↔ Purchasing | Reorder point triggers PO suggestions | Should |
| AI ↔ All Modules | Data access for insights and predictions | Should |
| Reports ↔ All Modules | Aggregation across all data sources | Must |

"Real-time" in this table means the POS and Inventory modules share the same local transaction: a committed sale decrements stock on the same device immediately (INV-006, 2-second rule). Cross-device real-time is governed by CLD-001 (within 5 seconds when online) — see §8.

### 12.2 External integrations — current phase [Verified — Vision §11.2]

| Integration | Description | Priority |
|---|---|---|
| SAT/CFDI | Electronic invoicing compliance | **Must (Mexico)** — see C-K, §9.3 |
| Email Service | Transaction receipts, reports, alerts | Should |
| SMS/WhatsApp | Notifications and marketing | Could (future) |

### 12.3 External integrations — future roadmap [Verified — Vision §11.3]

Payment gateways (Stripe, Mercado Pago, OpenPay) — Phase 2; accounting software (QuickBooks, Contpaq, Aspel) — Phase 2; e-commerce (Shopify, WooCommerce, Mercado Libre) — Phase 3; banking (automatic reconciliation) — Phase 3; delivery platforms (Uber Eats, Rappi, Didi Food) — Phase 3. None of these is specified in this SRS.

### 12.4 API contract outline [Proposed — Phase 2 draft §11, outline only]

No API exists [Verified — repository listing: no API contracts]. The Phase 2 draft proposes this outline, not a full OpenAPI spec: `/api/v1/auth, businesses, users, stores, warehouses, products, categories, barcodes, inventory, sales, payments, customers, credit, reports, sync, devices, outbox, ai`. Each contract, when written, must define: HTTP method, request/response schema, authentication, permission, tenant scope, validation, error responses, idempotency behaviour, and version/concurrency behaviour; OpenAPI must cover 100% of endpoints (NFR-MNT-002). API rate limits and versioning strategy are not defined (GAP-015) and are **[OPEN]** for the Phase 2 API work.

---

## 13. Acceptance criteria and testing [Verified — Vision §15; Proposed — Phase 2 draft §12]

### 13.1 General acceptance criteria [Verified — Vision §15.1]

| ID | Criterion |
|---|---|
| AC-GEN-001 | Every functional requirement must have a corresponding test case. |
| AC-GEN-002 | Every test case must pass in both online and offline modes. |
| AC-GEN-003 | No critical or high-severity bugs may exist at phase gate. |
| AC-GEN-004 | Performance benchmarks must be met on target hardware (minimum: 4GB RAM Android tablet, Chrome browser). |
| AC-GEN-005 | All user-facing text must be in Spanish (Mexico) with no untranslated strings. |
| AC-GEN-006 | No placeholder text, dummy data, or "TODO" comments in production code. |
| AC-GEN-007 | Security audit (OWASP ZAP or equivalent) must show zero critical vulnerabilities. |
| AC-GEN-008 | Data migration from CSV/Excel must complete with < 1% error rate. |

### 13.2 Module-specific acceptance criteria [Verified — Vision §15.2]

- **Inventory:** AC-INV-001 — 10,000 products imported; search returns the correct result in < 500 ms. AC-INV-002 — barcode scan adds a product to cart in < 1 second. AC-INV-003 — inventory adjustment with reason; audit log shows before/after values.
- **POS:** AC-POS-001 — cashier completes a 50-item sale in < 3 minutes. AC-POS-002 — offline sale queues; syncs correctly upon reconnection. AC-POS-003 — end-of-shift report balances to the centavo.
- **Fiado:** AC-FIA-001 — credit sale blocked at exactly credit limit + $0.01. AC-FIA-002 — customer statement shows all transactions chronologically.
- **Sync:** AC-SYNC-001 — 1,000 offline transactions sync without loss or duplication. AC-SYNC-002 — conflict resolution UI presents local vs server values clearly.

### 13.3 Test families that must pass before the Gate [Proposed — Phase 2 draft §12]

Unit tests; database constraint tests; API integration tests; tenant isolation tests; concurrent update tests (two clients updating the same product version — one succeeds, one gets a version conflict); offline transaction tests; duplicate event tests (same operation ID / idempotency key delivered twice — applied once); sync retry / interruption tests (interrupted sync resumes without duplication); device replacement tests (SAD §7 recovery flow, pending SyncQueue restored, hashes validated); data recovery tests (restore from backup within the stated objective — **[OPEN]** Q17); permission / security tests (a user cannot read another business's data; AI cannot bypass permissions).

**Mandatory two-device test [Proposed — Phase 2 draft §12]:** two devices sell the final unit of the same product while disconnected. Both sales must be preserved, the stock discrepancy must be recorded, and no data may be silently lost. This test must pass before the Phase 1 Gate.

### 13.4 Success criteria [Verified — Vision §22]

- **Product:** time to first sale < 15 min; feature adoption > 70% within 30 days; retention 80% (30-day) / 60% (90-day); NPS ≥ 50; CSAT ≥ 4.5/5; < 1 support ticket per user per month; time to resolve < 24 h [Verified — Vision §22.1]. (Phase 1 gate in §18.1 uses NPS ≥ 40 for beta — the Vision states both; the Gate should confirm which NPS target governs Phase 1 [Proposed — flag for Gate].)
- **Business (Phase 1):** 100 active businesses; $10,000 USD MRR; churn < 5%/month; ARPU $100/month; CAC < $50; LTV > $1,000; LTV:CAC > 3:1 [Verified — Vision §22.2].
- **Technical:** load < 3 s; API p95 < 200 ms; uptime 99.9% (see C-L, §5.2); sync success rate 99.99%; 0 data-loss incidents; 0 critical vulnerabilities; test coverage > 80%; daily deploys; MTTR < 4 h [Verified — Vision §22.3].

These are the Vision's stated targets, not measured results.

---

## 14. Risks, assumptions, and gaps [Verified — Vision §§19, 20, 23]

### 14.1 Risks [Verified — Vision §19]

**Technical risks (R-T):**
- R-T-001 — Offline sync conflicts corrupt inventory (Medium probability, Critical impact). Mitigation: robust conflict detection, manual resolution for complex cases, extensive testing with simulated network partitions.
- R-T-002 — AI assistant hallucinates financial advice (Medium, High). Mitigation: RAG with strict data grounding, confidence scores, human review for high-stakes recommendations.
- R-T-003 — Performance degrades with large product catalogs > 50k (Medium, High). Mitigation: database indexing, pagination, lazy loading, performance testing with synthetic large datasets.
- R-T-004 — Cloud service downtime prevents sync (Low, Medium). Mitigation: graceful degradation, extended offline capability, redundant infrastructure.
- R-T-005 — Security breach exposes customer business data (Low, Critical). Mitigation: OWASP compliance, penetration testing, encryption, least-privilege access, regular security audits.
- R-T-006 — Mobile app battery drain due to sync (Medium, Medium). Mitigation: optimised sync intervals, batch processing, background task management.

**Business risks (R-B):** R-B-001 — SAT changes CFDI requirements, breaking invoicing (Medium, High; mitigation: modular CFDI integration, monitoring of regulatory changes, rapid update capability — **this mitigation presupposes the CFDI decision in §9.3**). R-B-002 — competitor (Square, Clip) launches a similar LATAM-focused product (High, Medium). R-B-003 — target customers lack internet/devices to use cloud features (Medium, Medium). R-B-004 — customer data privacy concerns, LFPDPPP (Medium, High). R-B-005 — key personnel (founder, lead developer) unavailable (Low, Critical). R-B-006 — pricing model fails to achieve profitability (Medium, High).

**Market risks (R-M):** R-M-001 — economic downturn reduces SMB software spending (Medium, High). R-M-002 — Latin American currency volatility affects pricing (High, Medium). R-M-003 — low digital literacy in the target segment slows adoption (Medium, Medium). R-M-004 — piracy / cracked software competition (High, Low).

### 14.2 Gaps that must be resolved [Verified — Vision §23]

**Critical gaps — must resolve before Phase 1 (Vision §23.1):**
- **GAP-001 — Pricing & Subscription Model:** no pricing tiers, freemium boundaries, or payment collection mechanism defined. Decision required: free tier? What features are paid? Monthly price per store/user? (Linked to Q10, §17.)
- **GAP-002 — Hardware Compatibility Matrix:** no specification of supported barcode scanners, receipt printers, cash drawers, or label printers. Decision required: which brands/models are certified? USB, Bluetooth, or network printers?
- **GAP-003 — Data Migration Strategy:** no plan for migrating customers from existing systems (Excel, old POS, notebooks). Decision required: migration service offered? Self-service import only? Data mapping templates? (See §4.21.)
- **GAP-004 — Customer Support Model:** no definition of support channels, hours, SLAs, or escalation paths. Decision required: WhatsApp support? Phone? Email? Business hours only or 24/7?
- **GAP-005 — Disaster Recovery Plan:** backups are mentioned, but no formal DR plan exists. Decision required: RPO and RTO definitions? Geographic redundancy? (Linked to Q17.)

**High-priority gaps — resolve during Phase 1 (Vision §23.2):**
- GAP-006 — Training & Onboarding Program: no curriculum, video tutorials, or knowledge base structure defined.
- GAP-007 — Multi-Currency Support: MXN and USD mentioned but no mechanism for dual-currency transactions or exchange-rate handling.
- GAP-008 — Expense Tracking: Financial Dashboard listed but no explicit expense recording outside inventory COGS. (See §4.7.)
- GAP-009 — Supplier Payment Tracking: purchasing tracks POs but not actual payments to suppliers. (See §4.3.)
- GAP-010 — Customer Communication: no SMS, email, or WhatsApp integration for marketing or payment reminders.

**Medium-priority gaps — resolve during Phase 2 (Vision §23.3):** GAP-011 advanced promotions; GAP-012 recipe/BOM for restaurants; GAP-013 appointment/service booking; GAP-014 advanced analytics; GAP-015 API rate limits and documentation.

**Low-priority gaps — future consideration (Vision §23.4):** GAP-016 franchise management; GAP-017 manufacturing/MRP; GAP-018 white-labelling; GAP-019 offline data retention limits; GAP-020 accessibility certification (third-party audit, Phase 2).

---

## 15. Traceability matrix [Proposed]

Each row traces a requirement group to its Vision source, its SAD source where one exists, its database design group in the Phase 2 draft, and its test family in §13. "—" means no corresponding source exists in that document (itself a finding, e.g. Billing).

| Requirement group | Vision source [Verified] | SAD source [Verified] | Phase 2 draft table group | Test family (§13) |
|---|---|---|---|---|
| Inventory (INV-001–018) | §7.1 | §5 inventory | §3.2 Inventory (16 tables) | AC-INV-001–003, unit, constraint |
| POS (POS-001–018) | §7.2 | §5 sales | §3.3 POS (9 tables) | AC-POS-001–003, offline, two-device |
| Purchasing (PUR-001–007) | §7.3 | §5 inventory/sales | §3.4 Purchasing (5 tables) | Integration, workflow §10.2 |
| Suppliers (SUP-001–004) | §7.4 | §5 business | §3.6 Supplier (3 tables) | Integration |
| Customers (CUS-001–004) | §7.5 | §5 customers | §3.5 Customer/Fiado (6 tables) | Integration, tenant isolation |
| Fiado (FIA-001–007) | §7.6, §14.4 | §1, §4 | §3.5 Customer/Fiado | AC-FIA-001–002, ledger reconciliation |
| Financial dashboard (FIN-001–006) | §7.7 | §5 sales | §3.7 Financial (5 tables) | Integration (GAP-008 open) |
| Reports (RPT-001–012) | §7.8 | — | §3.7 Financial; §3.12 Supporting (ReportTemplate, ReportExecution) | Report accuracy, export |
| Analytics (ANL-001–005) | §7.9 | — | §3.8 AI (3 tables, partial) | Chart accuracy |
| Employees (EMP-001–004) | §7.10 | §5 auth | §3.1 Core (User, Role, UserRole) | RBAC, permission |
| Security / RBAC (SEC-001–006) | §7.11, §12.1 | §6, §8 | §3.13 Security (3 tables); §3.1 Roles | Permission, security audit |
| Warehouses (WHS-001–004) | §7.12 | §5 business | §3.1 Warehouse; §3.2 StockLevel | Integration, transfer |
| Stores (MST-001–005) | §7.13 | §5 business | §3.1 Store | Tenant isolation, consolidated reporting |
| AI assistant (AIA-001–007) | §7.14 | §6 | §3.8 AI (3 tables) | Traceability (AIA-007), permission |
| Notifications (NTF-001–005) | §7.15 | §3 (Event Worker) | §3.11 Settings (Notification, NotificationPreference) | Alert timing |
| Settings (SET-001–007) | §7.16 | §5 business | §3.11 Settings (5 tables) | Configuration |
| Offline sync (OFF-001–008) | §7.17, §9.8 | §§1–2, 7 | §3.9 Sync (3 tables) + §4 OutboxEvent | Offline, sync retry, two-device |
| Cloud sync (CLD-001–006) | §7.18 | §§1–3 | §3.9 Sync; §4 Outbox | Sync success, backup/restore |
| Audit (ADT-001–005) | §7.19, §14.5 | §5 audit | §3.1 AuditLog | Immutability, search |
| Backups (BKP-001–004) | §7.20 | — | §3.12 Supporting (BackupLog) | Restore within 4 h (Q17 open) |
| Import/Export (IEX-001–003) | §7.21 | — | §3.12 Supporting (ImportLog, ExportJob) | AC-GEN-008 (< 1% error) |
| Billing / subscriptions | §22.2 (metrics assume subscriptions); §23 GAP-001 | §4 (Phase 1 list), §5 billing | §3.10 Billing — **ADD [Proposed]**, not in DB v1.0 | **No test family — Gate decision (Q10)** |
| CFDI / invoicing | §11.2, §12.3 (Must); §16.2/§18.2 (deferred) | §4 (deferred to Phase 2) | — (no tables) | **No test family — Gate decision (Q11, C-K)** |

---

## 16. Open questions — do not invent (Q01–Q18) [OPEN]

All 18 questions from the Phase 2 draft §13 are carried forward unchanged, with their status preserved. None has been answered by inference in this SRS.

| ID | Question | Why it matters |
|---|---|---|
| Q01 | Is React + TypeScript + Vite + Express + Prisma the final approved stack? | Determines implementation foundation. |
| Q02 | Is IndexedDB / Dexie the chosen local database? | Determines offline architecture. |
| Q03 | Is SQLite-WASM still under consideration? | Prevents competing local storage designs. |
| Q04 | Is the full 77-table design approved? | Determines database scope. The Phase 2 draft proposes 48 core + 5 deferred + 3 confirm, plus 4 added tables — that proposal itself needs approval. |
| Q05 | Is Fiado included in the first commercial MVP? | Changes financial and sync requirements. |
| Q06 | Is AI mandatory in the first release, or initially limited? | Determines integration and operational cost. |
| Q07 | Which AI provider will be used? | Determines API and privacy architecture. |
| Q08 | Which hosting provider is approved? | Determines deployment design. |
| Q09 | Which object storage provider is approved? | Determines image/document storage. |
| Q10 | What is the subscription and payment provider? | Determines billing integration. Vision §23 GAP-001: no pricing model defined. |
| Q11 | Will PAGATEC integrate with SAT / CFDI in its first release? | Determines fiscal compliance scope. See C-K, §9.3. |
| Q12 | Which payment methods must work offline? | Affects POS and reconciliation. |
| Q13 | How many simultaneous devices per business must be supported initially? | Affects sync testing. |
| Q14 | How should stock conflicts be resolved? | Protects inventory integrity. |
| Q15 | What is the authoritative local schema versioning strategy? | Prevents incompatible devices. |
| Q16 | What data must remain available after subscription expiration? | Determines customer-data policy. |
| Q17 | What is the exact backup and recovery objective? | Determines infrastructure and cost. Vision §23 GAP-005: no formal DR plan; RPO/RTO undefined. |
| Q18 | Are there formal privacy and retention policies? | Determines data governance. |

Additional commercial-policy questions, also **[OPEN]**: free subscription limits; maximum users / stores / products per tier; Fiado interest rates and grace periods; inventory valuation method; negative-stock permissions; subscription-expiration behaviour; refund authorisation requirements. These belong in an approved business-rules document, not in code defaults.

---

## 17. Phase 1 Gate Review checklist

The Phase 1 Gate Review is the next action the Vision names after this SRS [Verified — Vision PDF, End of Document]. No implementation may begin until the Gate is passed. This checklist combines the Phase 2 draft §14 gate items 2–9 (item 1 — PDF/DOCX extraction — is done) with the SRS-specific decisions identified in this document.

### 17.1 Items carried from the Phase 2 draft §14 [Verified — Phase 2 draft §14]

| # | Gate item | Status |
|---|---|---|
| 1 | The PDF and DOCX have been extracted and compared, and the Phase 2 draft §2 updated with the result. | **[x] Done 2026-11-25** (Phase 2 draft §1.2/§1.4). |
| 2 | The disposition of the 77 tables (48 core / 5 deferred / 3 confirm) and the 4 added tables (OutboxEvent + 3 Billing). | **[ ] OPEN** — Q04. |
| 3 | The OutboxEvent design and retention policy (Phase 2 draft §4). | **[ ] OPEN**. |
| 4 | The local persistence choice — IndexedDB/Dexie vs SQLite-WASM (Q02/Q03). | **[ ] OPEN**. |
| 5 | Fiado in the first release (Q05). | **[ ] OPEN**. |
| 6 | The stack (Q01) and the AI provider / scope (Q06/Q07). | **[ ] OPEN**. |
| 7 | The conflict classification in Phase 2 draft §8.5, especially inventory and credit — including the approved LWW field list (see §8.4 of this SRS). | **[ ] OPEN** — Q14. |
| 8 | The migration strategy (Phase 2 draft §10) and the testing list (Phase 2 draft §12; §13 of this SRS). | **[ ] OPEN**. |
| 9 | The CFDI decision (C-K) and the uptime target clarification (C-L). | **[ ] OPEN** — Q11; C-L. |

### 17.2 SRS-specific Gate decisions [Proposed]

| # | Decision | Source in this SRS | Status |
|---|---|---|---|
| G-1 | CFDI: Option A (move into Phase 1) or Option B (launch without invoicing, limitation stated). | §9.3, C-K, Q11 | **[ ] OPEN — Gate decision** |
| G-2 | Uptime target: 99.9% (NFR-AVL-001) or 99.99% (TO-005). Draft uses 99.9%. | §5.2, C-L | **[ ] OPEN** |
| G-3 | Pricing and subscription model (GAP-001): tiers, freemium boundaries, payment collection, provider (Q10). | §4.10, §14.2 | **[ ] OPEN — critical gap** |
| G-4 | Hardware compatibility matrix (GAP-002): certified scanners, printers, drawers. | §14.2 | **[ ] OPEN — critical gap** |
| G-5 | Data migration strategy (GAP-003): migration service vs self-service import only. | §4.21, §14.2 | **[ ] OPEN — critical gap** |
| G-6 | Customer support model (GAP-004): channels, hours, SLAs. | §14.2 | **[ ] OPEN — critical gap** |
| G-7 | Disaster recovery plan (GAP-005): RPO and RTO (Q17). | §4.20, §14.2 | **[ ] OPEN — critical gap** |
| G-8 | MFA in Phase 1: Vision §12.1 requires it, SAD §4 defers it to Phase 2. | §9.1 | **[ ] OPEN** |
| G-9 | Accountant and Read-Only roles in Phase 1: SEC-001 lists them, Vision §16.1 item 7 does not. | §4.11 | **[ ] OPEN** |
| G-10 | Expense tracking in Phase 1 (GAP-008) and supplier payment tracking (GAP-009). | §§4.3, 4.7 | **[ ] OPEN** |
| G-11 | Recipe/BOM in Phase 1 if the restaurant segment is a priority (GAP-012). | §3.3 | **[ ] OPEN** |
| G-12 | Inventory valuation default method (INV-013) and product-image upload path (INV-011, Q09). | §§4.1 | **[ ] OPEN** |
| G-13 | NPS target for the Phase 1 beta gate: ≥ 40 (§18.1) or ≥ 50 (§22.1). | §13.4 | **[ ] OPEN** |
| G-14 | Performance target for primary screens: < 3 s (NFR-PER-001) or sub-2 s (TO-001). | §5.1 | **[ ] OPEN** |
| G-15 | AI accuracy measurement for AIA-001 (80% on common questions) and the AI security/governance specification. | §11 | **[ ] OPEN** |

**Until the Gate is passed, this SRS is a working draft for review, not an implementation blueprint.** The Gate outcome, including any scope changes (notably G-1), must be recorded in writing and the SRS revised to match it before code is written.

---

## 18. Requirements that could not be elaborated from the verified documents

The following could not be elaborated, and are recorded here rather than filled in by assumption. Each needs a Gate decision, a further document, or both.

1. **Tech stack, hosting, AI provider, pricing** — not specified in SAD or the Vision [Verified by absence in the extracted texts — Phase 2 draft §1.3]. Q01, Q06–Q10 remain **[OPEN]**. The SRS specifies behaviour the stack must satisfy (NFRs, §5), not the stack itself.
2. **CFDI/PAC integration design** — the Vision requires CFDI 4.0 generation but contains no PAC integration, certificate, or regulatory workflow specification [Verified — Phase 2 draft §3.7]. Cannot be elaborated until G-1 is decided.
3. **Billing lifecycle** — no dedicated Billing/Subscription tables exist in the design, and no pricing model is defined (GAP-001). The Phase 2 draft's 3-table Billing proposal is **[Proposed]**, not a specification. Cannot be elaborated until G-3 is decided.
4. **Complete authentication design** — password hashing parameters, token lifecycle, refresh rotation, session revocation, rate limiting, account recovery, MFA recovery, permission inheritance, tenant authorisation, device authorisation are not specified by the presence of a Session table [Verified — ChatGPT handoff §12.3]. Requires a security model deliverable.
5. **Local persistence implementation** — the choice between IndexedDB/Dexie and SQLite-WASM is unresolved (Q02/Q03), and the local schema versioning strategy is unresolved (Q15). The SRS specifies the local subset and its obligations (§8.7), not the engine.
6. **API contracts** — no OpenAPI spec exists; only the outline in §12.4. Rate limits and versioning are undefined (GAP-015). Requires an API contract deliverable in Phase 2 work.
7. **Hardware compatibility** — no certified scanner/printer/drawer matrix exists (GAP-002). Cannot be elaborated until G-4 is decided.
8. **Disaster recovery** — no formal DR plan, RPO, or RTO exists (GAP-005, Q17). Cannot be elaborated until G-7 is decided.
9. **Data migration** — no migration plan from Excel / old POS / notebooks exists (GAP-003). Cannot be elaborated until G-5 is decided.
10. **AI measurement and governance** — no accuracy measurement method for AIA-001, and no complete AI security/governance specification (see §11.3). Cannot be elaborated until G-15 is decided.
11. **Commercial policy** — free-tier limits, per-tier maxima, Fiado interest/grace policy, valuation method, negative-stock permissions, subscription-expiry behaviour, refund authorisation. These belong in an approved business-rules document; the SRS records the documented defaults (Fiado $1,000 / $50,000 / 2% / 30 days) as defaults only (§4.6).
12. **Phase 2/3 requirements** — advanced analytics, native mobile apps, payment gateways, loyalty, scheduling, plugin system, REST API, portals, voice assistant, accounting integrations, LGPD/GDPR implementation. Explicitly out of scope for this SRS (§3.3); they require their own elaboration when their phase begins.

---

## 19. Sources and limitations

- **Sources used in this SRS:** Vision PDF (42 pp., §§1–23, extracted with pdftotext 2026-11-25, see Phase 2 draft §1.4); SAD v1.2 DOCX (extracted with python-docx 2026-11-25, §§1–10, see Phase 2 draft §1.3); Database Design Document v1.0 (read in full 2026-11-25); Product Vision txt v1.0 (read in full; used only as the historical record — the PDF supersedes it, §1.5); Phase 2 Database Architecture v1.1 DRAFT (905 lines, 2026-11-25, DRAFT — NOT APPROVED), including its §2 conflicts C-A–C-L, §3 table dispositions, §4 OutboxEvent design, §8 sync/conflict rules, §12 testing list, §13 Q01–Q18, and §14 gate items; ChatGPT PAGATEC handoff (2026-11-25, 1m 4s) and independent audit (2026-11-25, 40s) for the conflict analysis, the LWW/OCC coexistence analysis, and the Document Authority language adopted in §1.6.
- **Sources NOT used / still open:** none outstanding for the repository documents — all five files have now been read or extracted. Remaining unverified items are the open decisions in §16 (Q01–Q18) and §17 (Gate items 2–9, G-1–G-15), which need Yeney's decisions, not more document reading.
- **What this SRS is not:** it is not an approved specification, not a design document, not a Prisma schema, not an API contract, not a test suite, and not evidence that any PAGATEC software exists. It is a detailed, traceable draft for the Phase 1 Gate Review.

*End of SRS v1.0 DRAFT — 2026-11-25. Awaiting: the Phase 1 Gate Review (§17). Do not implement from this document alone.*
