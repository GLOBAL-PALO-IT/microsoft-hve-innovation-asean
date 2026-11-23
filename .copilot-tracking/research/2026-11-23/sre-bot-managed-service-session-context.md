<!-- markdownlint-disable-file -->
# Session Context: sre-bot-managed-service

> **Status:** DRAFT for human review. This artifact defines the shared session context only. No research, requirements, backlog items, or architecture have been produced.

## Session

| Field | Value |
|---|---|
| Session name | sre-bot-managed-service |
| Topic | HVE Managed Service: SRE Bot for autonomous incident detection, diagnosis, and recovery with human approval, using Microsoft Teams, Azure, and GitHub Issues |
| Date | 2026-11-23 |
| Track | Subject Matter Expert (Create Context) |
| Owner | @prabakarkesavpalo (PALO IT) |

## Evidence Location

| Item | Location | Handling |
|---|---|---|
| Primary source | `/Users/pkesavan/Desktop/HVE-Managed Service.pptx` (6 slides: title, executive summary, incident response as-is/to-be, how it works, solution architecture, MVP scope) | Controlled local file. Cite by filename and slide number. Do not copy into the repository. |
| Workshop input folder | `.copilot-tracking/research/workshop-input/` | Reserved for synthetic or public files only, organized by type: `policies/`, `sops/`, `process-diagrams/`. Currently empty. |

## Evidence Boundary

* The source deck is marked **CONFIDENTIAL** (PALO IT). Treat its content as internal; do not copy the file or sensitive material into the repository or external systems.
* Only synthetic or public material may be placed in `workshop-input/`.
* Real incident logs, customer data, credentials, tenant or subscription identifiers, and production alert payloads are out of bounds for the repository. Reference them through an approved system or controlled link.
* Where evidence is missing, record `Unknown` with the source or decision needed. Do not invent customers, pricing, or metrics.

## Scenario Summary (from source deck, for orientation only)

* **Problem:** On-call engineers run detect, diagnose, and recover steps by hand (alert fatigue, manual log search, manual recovery).
* **Proposed outcome:** SRE Bot automates detection (`/detect`), root-cause analysis (`/analysis`), and recovery proposal or application (`/recover`). An engineer approves the final fix.
* **Control model:** Microsoft Teams is the interaction surface. Every request and bot action is written to a GitHub Issue as the auditable system of record.
* **MVP triggers:** Teams requests and Azure Monitor alerts.
* **MVP path:** Azure Bot Service, Azure Functions, Azure Service Bus, Foundry Agent Service with Azure OpenAI, and five GitHub Issues tools (`create_issue`, `update_issue`, `add_comment` / `list_comments`, `get_issue`, `search_issues`). Operations use Application Insights and Azure Key Vault.
* **Later phases (out of MVP scope):** file uploads and screenshots, on-prem and SNMP alerts, scheduled jobs, AWS and GCP health events, email and web portal intake, non-Azure or local LLMs, recovery without human approval.

## Production Output Roots

| Outcome | Root |
|---|---|
| Context and research | `.copilot-tracking/research/` |
| Business requirements | `.copilot-tracking/brd-sessions/` |
| Requirements and PRD | `.copilot-tracking/prd-sessions/` |
| Implementation plan | `.copilot-tracking/plans/` |
| Architecture and publication | `.copilot-tracking/details/` |
| Design and UX | `.copilot-tracking/dt/` |
| Architecture decisions | `docs/planning/adrs/` |
| GitHub backlog | `.copilot-tracking/github-issues/` |

## Open Items

* Confirm the target customer segment and decision-maker for the managed service. Currently `Unknown`.
* Confirm whether real or synthetic incident evidence (alerts, runbooks, SOPs) will be used, and where it will be stored.
* Confirm classification and sharing permissions for the source deck before it is used beyond the team. Resolved 2026-11-23: the owner decided the confidentiality marking is not a sharing constraint (D5).

## Next Step

Use this artifact as the single source for topic, evidence location, boundary, and output roots. Downstream prompts, starting with the Marketplace Lean Canvas and `/rpi-research`, should find this file rather than restate the topic.
