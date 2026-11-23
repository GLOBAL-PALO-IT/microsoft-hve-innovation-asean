<!-- markdownlint-disable-file -->
# RPI Changes: SRE Bot Managed Service — First-Pass UX Prototype

## Metadata

* Task ID: `sre-bot-managed-service-ux-prototype`
* Related plan: [microsoft-hve-innovation-asean/.copilot-tracking/plans/2026-11-23/sre-bot-managed-service-mvp-plan.md](../../../microsoft-hve-innovation-asean/.copilot-tracking/plans/2026-11-23/sre-bot-managed-service-mvp-plan.md)
* Implementation date: `2026-11-23`

## Execution Status

* Status: Complete (for bounded synthetic UX prototype scope)
* Declared invocation scope: First-pass UX prototype under `.copilot-tracking/ux-test/` using synthetic data and mock responses (non-production pre-MVP UX validation slice)
* Completed scope markers: First-pass UX prototype deliverables complete under `.copilot-tracking/ux-test/` (none of the production plan markers `P01`–`P03` are checked off, as production implementation is not authorized)
* All remaining active-plan markers: `P01`, `P01-T01`, `P02`, `P02-T01`, `P02-T02`, `P02-T03`, `P03`, `P03-T01`, `P03-T02`
* Status basis: The user-directed synthetic UX prototype is complete and browser-verified with zero runtime errors. Production plan markers in [microsoft-hve-innovation-asean/.copilot-tracking/plans/2026-11-23/sre-bot-managed-service-mvp-plan.md](../../../microsoft-hve-innovation-asean/.copilot-tracking/plans/2026-11-23/sre-bot-managed-service-mvp-plan.md) remain intentionally unchecked because the plan's standard critique, human review of draft BRD/PRD/UX map, and `D4` governance decisions remain open.

## Execution Summary

Delivered a zero-dependency, self-contained interactive HTML/CSS/JS UX prototype under [.copilot-tracking/ux-test/](../../ux-test/) modeling the **On-Call Incident Response** MVP workflow from the workshop context (`map-journey.md`, `PRD-2026-Q4-001`, `sre-bot-managed-service-mvp-plan.md`). The prototype uses synthetic data and mock responses only, preserves all MVP scope boundaries (Teams + Azure Monitor triggers only, payload-only analysis, non-executing recovery proposals, GitHub Issue 5-operation audit trail), separates Prototype UX Acceptance Criteria from deferred integration/production criteria, and passed end-to-end interactive verification in a real browser.

## Completed Work

### First-Pass On-Call Incident Response UX Prototype

* Related phase or task: Pre-MVP UX validation slice grounded in `P01-T01`, `P02-T01`, `P02-T02`, `P02-T03` (non-production; plan markers left unchecked)
* Files:
  * [.copilot-tracking/ux-test/index.html](../../ux-test/index.html)
  * [.copilot-tracking/ux-test/styles.css](../../ux-test/styles.css)
  * [.copilot-tracking/ux-test/app.js](../../ux-test/app.js)
  * [.copilot-tracking/ux-test/README.md](../../ux-test/README.md)
* Behavior or functionality changed:
  * **Microsoft Teams Desktop & Adaptive Card UI:** Redesigned from the initial generic 4-tab form into a Microsoft Teams v2 desktop layout — top Teams title bar with search and `Demo Simulator` / `Pause Bot` controls, far-left app rail (`Activity`, `Chat`, `Teams`, `SRE Bot`), channel sidebar (`Cloud Operations (SRE)` → `#on-call-incidents`, `SRE Bot (Direct)`, active incident threads), channel header tabs (`Posts`, `GitHub Issues`, `UX Criteria`), threaded conversation feed with **SRE Bot `APP` Adaptive Cards**, bottom Teams compose box, and a collapsible right-side `Workshop Simulator` drawer.
  * **Trigger intake & boundary (`FR-001`, `FR-002`, `FR-012`):** Direct Teams compose box (`FR-001`), custom Azure Monitor alert simulator (`FR-002`), 4 one-click synthetic walkthrough scenarios (`Recurring Disk-Full`, `New High-CPU Alert`, `Insufficient Info`, `Untrusted Payload`), and an unsupported-trigger tester that rejects email, AWS Health, screenshot/file upload, and SNMP inputs (`AC-021`).
  * **Data entry, lookup, normalization & grounded analysis (`FR-003`, `FR-004`, `FR-005`, `FR-006`, `FR-011`, `FR-014`):** Adaptive Card FactSet displays normalized source, incident key, recurrence count (`AC-020`), and elapsed time (`NFR-008`); visually separates **Supplied Evidence Cited**, **Generated Analysis (Unverified)**, and **Missing Context & Unknowns** (`DD-001`); refuses to invent a diagnosis or proposal when payload information is insufficient (`AC-024`); flags prompt-injection-like text as untrusted data (`NFR-003`, `AC-013`).
  * **Review, confirmation, cancel, reject & error states (`FR-007`, `FR-008`, `FR-009`, `FR-010`, `FR-013`, `NFR-002`, `NFR-008`, `NFR-009`, `CON-007`):** Adaptive Card actions stay **Pending** (`AC-022`), never execute automatically (`FR-007`, `AC-007`), and open Fluent confirmation dialogs for **Approve (Manual Execution)** (with Confirm & Cancel) and **Reject Proposal** (validated reason recording `No Recovery Approved`, `AC-023`). Simulator toggle exercises **GitHub API 429** and **Teams 503** degradation with observable banner and **Retry Sync** (`NFR-009`). Embedded `GitHub Issues` tab logs actor, action, and fixed five-operation tool names (`CON-007`).
  * **Separated acceptance criteria:** Embedded `UX Criteria` channel tab and `README.md` separate in-scope Prototype UX Acceptance Criteria (`UX-01` through `UX-07`) from deferred production/integration criteria (auth, data/redaction, live AI/SLA, security, Marketplace publishing).
* Validation: Passed — visual screenshot inspection (catching and fixing channel-tab wrapping and header-pill overlap) plus end-to-end Playwright browser verification across all scenarios, Adaptive Card Approve/Cancel/Reject modals, GitHub 429 + Retry Sync, and embedded tabs with `pageErrors: []`.

## Implementation-Time Plan Updates

* None. The workshop plan under [microsoft-hve-innovation-asean/.copilot-tracking/plans/2026-11-23/sre-bot-managed-service-mvp-plan.md](../../../microsoft-hve-innovation-asean/.copilot-tracking/plans/2026-11-23/sre-bot-managed-service-mvp-plan.md) was left unmodified so production phase/task markers remain unchecked until human review, standard critique, and `D4` decisions are completed.

## Business Alignment

### PRD-2026-Q4-001 — Synthetic UX Prototype Scope Boundary

* Source document: [microsoft-hve-innovation-asean/.copilot-tracking/prd-sessions/sre-bot-managed-service-prd.md](../../../microsoft-hve-innovation-asean/.copilot-tracking/prd-sessions/sre-bot-managed-service-prd.md) (`PRD-2026-Q4-001`, status `draft`)
* Cited requirement IDs: `FR-001` through `FR-014`, `NFR-001` through `NFR-010`, `CON-001`, `CON-007`
* Owner or deciders: `@prabakarkesavpalo`
* Related plan update or marker: Pre-MVP UX prototype (`.copilot-tracking/ux-test/`)
* What changed: Implemented a local, synthetic, non-production HTML/CSS/JS prototype to validate user flows before committing to undecided technology or policy choices; live Teams, Azure Monitor, GitHub, Azure OpenAI, and Key Vault integrations are mocked rather than built.
* Why: Explicitly directed by the user as the first step toward the MVP to review UX with stakeholders/SMEs before production integration.
* User decision: User explicitly requested a synthetic UX prototype under `.copilot-tracking/ux-test/` without finalizing undecided technology or policy choices.
* Alignment: `clarification`

## Validation Record

| Check | Scope | Status | Evidence or reason |
|---|---|---|---|
| Browser interactive & state smoke check (Playwright) | `.copilot-tracking/ux-test/index.html` | Passed | Verified initial hidden states (fixed `[hidden]` CSS rule), empty Teams form validation, unsupported trigger rejection (`FR-012`), Scenario A recurrence linking to `#42` (`AC-020`), Approve modal Cancel & Confirm (`AC-007`, `AC-022`), Scenario B new Issue `#43` (`AC-005`), Reject empty validation & Confirm (`AC-023`), Scenario C insufficient info (`AC-024`), Scenario D untrusted-input banner + GitHub 429 error & retry recovery (`AC-013`, `AC-019`), Audit tab (3 rows), Criteria tab (7 items), and `pageErrors: []`. |
| Production auth / data / AI / security / Marketplace checks | Production MVP | Skipped | Explicitly excluded from UX prototype scope per user instruction. |

## Pre-Review Reconciliation

* Plan markers and task-local context: Current (production markers `P01`–`P03` remain unchecked)
* Completed-work entries and handoff prose: Current
* Validation, blockers, remaining work, and follow-up items: Current
* Review readiness: Ready for stakeholder/SME UX walkthrough and optional `/rpi-review` of the UX prototype slice; **not** ready for production MVP review until `P01`–`P03` are authorized and implemented.

## Blockers

* None for the synthetic UX prototype.
* Production MVP implementation (`P01`–`P03`) remains blocked by unchecked human review of draft BRD, PRD, and `map-journey.md`, pending standard plan critique, and unresolved `D4` decisions (approver authority, incident matching, redaction, retention, success targets).

## Remaining Work

* Production plan markers `P01`, `P01-T01`, `P02`, `P02-T01`, `P02-T02`, `P02-T03`, `P03`, `P03-T01`, `P03-T02` remain unchecked pending stakeholder/SME UX review, human review of draft source documents, standard plan critique, and `D4` resolution.

## Follow-Up Items

* Canonical plan list: [microsoft-hve-innovation-asean/.copilot-tracking/plans/2026-11-23/sre-bot-managed-service-mvp-plan.md](../../../microsoft-hve-innovation-asean/.copilot-tracking/plans/2026-11-23/sre-bot-managed-service-mvp-plan.md), `## Follow-Up Items`
* Conduct stakeholder / SME walkthrough of [.copilot-tracking/ux-test/index.html](../../ux-test/index.html) using the `UX-01`–`UX-07` checklist before building production integrations.
* Complete standard plan critique, qualified human review of draft BRD/PRD/`map-journey.md`, and `D4` governance decisions before authorizing production `P01`–`P03` implementation.

## Return-to-Caller State

* Implementation execution status: Complete (bounded to synthetic UX prototype under `.copilot-tracking/ux-test/`)
* Declared scope and markers: Synthetic UX prototype complete; all production plan markers (`P01`, `P01-T01`, `P02`, `P02-T01`, `P02-T02`, `P02-T03`, `P03`, `P03-T01`, `P03-T02`) remain unchecked
* Validation coverage: Browser Playwright smoke test passed across all 11 interactive states with 0 page errors; production/integration guarantees intentionally not validated
* Blockers: None for UX prototype; production MVP remains blocked by pending critique, human review, and `D4`
* Current plan updates: None
* Business alignment entries: 1 (`PRD-2026-Q4-001` — `clarification`)
* Planning and critique state: Plan remains not ready for production; standard critique pending
* Follow-up items: Stakeholder/SME UX review; production readiness gates
* Review readiness or no-handoff reason: Ready for stakeholder/SME UX walkthrough (and optional `/rpi-review` on the prototype slice); production MVP review not yet eligible
* Continuation owner: User (standalone `/rpi-implement` invocation)
