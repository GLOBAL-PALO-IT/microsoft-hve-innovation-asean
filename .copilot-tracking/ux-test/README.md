<!-- markdownlint-disable-file -->
# SRE Bot Managed Service — First-Pass UX Prototype (Synthetic)

## Purpose

This self-contained, zero-dependency HTML/CSS/JS prototype validates the **On-Call Incident Response** user flow from the workshop context before any production integrations are built. It uses synthetic data and mock responses only and does **not** finalize undecided technology or policy choices.

## Source Workshop Context

* Experience draft: [map-journey.md](../../microsoft-hve-innovation-asean/.copilot-tracking/ux-artifacts/sre-bot-managed-service/on-call-incident-response/map-journey.md) (Status: `partial`, human review unchecked)
* Product requirements: [sre-bot-managed-service-prd.md](../../microsoft-hve-innovation-asean/.copilot-tracking/prd-sessions/sre-bot-managed-service-prd.md) (`PRD-2026-Q4-001` v0.2.0, Status: `draft`, human review unchecked)
* Implementation plan: [sre-bot-managed-service-mvp-plan.md](../../microsoft-hve-innovation-asean/.copilot-tracking/plans/2026-11-23/sre-bot-managed-service-mvp-plan.md) (Workshop handoff; production readiness blocked by critique, human review, and `D4`)

## How to Run Locally

Open `.copilot-tracking/ux-test/index.html` directly in a browser, or serve the folder locally:

```bash
python3 -m http.server 8765 --directory .copilot-tracking/ux-test
```

Then visit `http://localhost:8765/`.

## Included Synthetic Scenarios

1. **Scenario A — Recurring Disk-Full (Teams · J2 / `FC-001`):** Matches existing synthetic GitHub Issue `#42` via `search_issues` + `add_comment` without creating a duplicate (`FR-011`, `AC-020`).
2. **Scenario B — New High-CPU Alert (Azure Monitor · J1):** Creates a new synthetic GitHub Issue `#43` (`FR-002`, `FR-005`), normalizes source/time/content (`FR-003`), and presents a pending non-executing recovery proposal (`FR-007`).
3. **Scenario C — Insufficient Information (`FR-014` / `AC-024` / `FC-007`):** Refuses to invent a root cause or unsupported recovery proposal and states what context is missing.
4. **Scenario D — Untrusted Instruction in Payload (`NFR-003` / `AC-013` / `FC-005`):** Flags prompt-injection-like text as untrusted data, ignores embedded override instructions, and preserves human decision control.
5. **Unsupported Trigger Boundary (`FR-012` / `AC-021`):** Rejects Email, AWS Health, Screenshot/file upload, and SNMP triggers without creating an incident record.
6. **Integration Error / Throttling (`NFR-009` / `AC-019`):** Header selector simulates GitHub API 429 or Teams 503 failures with visible error banners and retry recovery.
7. **Decision States (`FR-008`, `FR-013`, `AC-008`, `AC-022`, `AC-023`):** Pending proposals stay pending; Approve opens a manual-execution confirmation modal (with Cancel); Reject requires a reason and records `No Recovery Approved`.

## Separated Acceptance Criteria

### Prototype UX Acceptance Criteria (Validate with Stakeholders / SMEs Here)

* `UX-01`: Supported triggers (Teams, Azure Monitor) are accepted and unsupported triggers are rejected cleanly (`FR-001`, `FR-002`, `FR-012`).
* `UX-02`: Normalized event fields and recurrence linking to an existing GitHub Issue are clear (`FR-003`, `FR-004`, `FR-011`).
* `UX-03`: Supplied evidence, generated analysis, and missing context/unknowns are visually and textually distinct; insufficient payloads state limits (`FR-006`, `FR-014`).
* `UX-04`: Untrusted instruction-like payload content is flagged and does not bypass human review (`NFR-003`).
* `UX-05`: Non-executing recovery proposals support Pending, Approve (Confirm/Cancel), and Reject (with reason) states (`FR-007`, `FR-008`, `FR-013`).
* `UX-06`: GitHub Issue audit log shows actor, action, and fixed five-operation tool boundary (`CON-007`, `FR-009`, `NFR-002`), and Teams thread replies remain plain-text safe (`FR-010`, `NFR-006`).
* `UX-07`: Form validation, simulated integration failures/retries (`NFR-009`), keyboard navigation, visible focus, and non-color status badges work as expected (`NFR-006`).

### Deferred Production & Integration Criteria (Do NOT Validate Against This Prototype)

* Production authentication, Entra ID / RBAC, approver authority, and approval timeouts (`OQ-010`, `OQ-012`, `NFR-004`).
* Production customer/tenant data, classification, redaction, and retention rules (`NFR-007`, `OQ-011`).
* Live Azure OpenAI in Microsoft Foundry quality, latency targets, and manual-baseline comparisons (`CON-003`, `NFR-008`, `OQ-002`).
* Production Key Vault secret management, live GitHub rate-limit queueing, and adversarial security testing (`NFR-001`, `NFR-003`, `NFR-009`).
* Marketplace or Microsoft 365 Copilot Agent Store billing, activation, deployment, publishing, or Azure SRE Agent positioning (`D3`, `D7`, `P03-T02`).
