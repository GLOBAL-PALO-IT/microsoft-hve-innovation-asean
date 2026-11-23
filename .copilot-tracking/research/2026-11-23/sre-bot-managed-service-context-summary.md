<!-- markdownlint-disable-file -->
# Context Document: SRE Bot Managed Service

> **Status:** DRAFT for human review. Summary-level synthesis of existing research. Nothing here is new evidence.

- [ ] Reviewed and validated by a qualified human reviewer

| Field | Value |
|---|---|
| Session | sre-bot-managed-service |
| Date | 2026-11-23 |
| Owner | @prabakarkesavpalo (PALO IT) |
| Evidence trail | `.copilot-tracking/research/2026-11-23/sre-bot-context-discovery-research.md` (primary; evidence IDs C# and W# below), `.copilot-tracking/research/2026-11-23/sre-bot-lean-canvas-research.md`, `.copilot-tracking/research/workshop-input/leancanvas.md` |
| Session context | `.copilot-tracking/research/2026-11-23/sre-bot-managed-service-session-context.md` |
| Evidence boundary | Source deck is CONFIDENTIAL (PALO IT), cited by slide, not copied. No real incident, customer or tenant data. |

Labels: **Fact** = stated by the source deck or a cited public source (not independently verified); **Assumption** = unsupported belief; **Decision** = confirmed by the user or recorded; **Proposed** = suggestion awaiting a human decision; **Open** = unknown.

## 1. Problem statement

| Type | Statement | Reference |
|---|---|---|
| Fact | On-call engineers run detect, diagnose and recover by hand: respond to an alert, search logs and diagnose, then recover. The deck frames this as alert fatigue. | C3 slides 2, 3 (see research Finding "Business facts") |
| Fact | The proposal automates detect, analysis and recover with an SRE Bot and keeps an engineer as approver of the final fix. | C3 slides 2, 3 |
| Open | How often it happens, how long it takes, what downtime costs. | Research risks table |

## 2. Affected users

| Type | Statement | Reference |
|---|---|---|
| Fact | On-call engineers request actions in Teams and approve the final fix. The wider team sees every action on the shared GitHub Issue. | C3 slides 3, 4 |
| Assumption | Access administrators, security reviewers and the managed-service operator are affected by the design. | Research Finding "Affected users" (inferred) |
| Open | Customer organization, buyer, sponsor and other stakeholders. | Session context open items; D5 |

## 3. Business impact

| Type | Statement | Reference |
|---|---|---|
| Fact | The deck claims faster, more consistent recovery with oversight and an auditable record. | C3 slide 2 |
| Open | Baselines, targets, volumes, cost of inaction, price and revenue model. No figures exist in the evidence. | Lean canvas unknowns |

## 4. Business rules

| Type | Rule | Reference |
|---|---|---|
| Fact | An engineer approves the final fix. Recovery without approval is a later phase. | C3 slides 2, 3, 6 |
| Fact | Every Teams request and bot action is recorded on the matching GitHub Issue. | C3 slides 2, 4 |
| Fact | The MVP accepts two triggers only: Teams requests and Azure Monitor alerts. | C3 slides 5, 6 |
| Decision | In the MVP, diagnosis uses only what arrives in the alert or Teams message, and the engineer runs any recovery. | D6, user answer 2026-11-23 |
| Assumption | A recurring alert should link to an existing issue (inferred from `search_issues` and the "existing" example). | Research Finding "Business rules" |
| Open | Severity, escalation, who may approve, approval time limits, data retention. | Research Finding "Business rules" |

## 5. Known failure cases

| Type | Statement | Reference |
|---|---|---|
| Fact | The deck's only example is a disk-full alert that fired again on a VM. No documented incident, wrong diagnosis or failed recovery exists. | C3 slide 4 |
| Assumption (unverified) | Plausible failures: wrong root cause stated with confidence, a bad fix approved under fatigue, duplicate or flooding issues, Teams or GitHub outage, dead-lettered messages, sensitive log content in an issue. | Research Finding "Known failure cases" |
| Fact | GitHub limits content creation to about 80 requests a minute and 500 an hour, which an alert burst could reach. | W3 |

## 6. AI guardrails

| Type | Statement | Reference |
|---|---|---|
| Fact | Stated guardrails: human approval, the GitHub Issue audit trail, a fixed five-operation tool set, secrets in Key Vault. | C3 slides 2, 4, 6 |
| Assumption | Gaps exist: bot identity and permissions, approval authority, an emergency stop, action limits, and handling of prompt injection through alert text, logs and comments. | C3, C6, C7 (absence in the deck; OWASP risk classes) |
| Proposed | Treat alert, log and comment content as untrusted input and require a defined identity and permission boundary. This is a recommendation for human decision, not a stated requirement. | C6, C7 |

## 7. Alternatives and counter-evidence

| Type | Statement | Reference |
|---|---|---|
| Fact | Azure SRE Agent documents overlapping capabilities: Azure Monitor alert intake, Teams and GitHub integration, Review and Autonomous run modes, tool-level allow, ask or deny, audit, usage-based billing. | W1, W2 |
| Open | Whether the offer is differentiated, and GA status and availability of that product. | D7 |

## 8. Decisions, and open questions

| ID | Item | Status | Owner |
|---|---|---|---|
| D4 | Is Jira in scope? | Decision: replaced by GitHub Issues (user, 2026-11-23) | SME |
| D5 | May the confidential deck and summaries be shared beyond the team? | Decision: user chose to ignore the confidentiality marking as a sharing constraint (2026-11-23) | Owner |
| D6 | MVP diagnosis and recovery boundary | Decision (option A) | User |
| D7 | Positioning against Azure SRE Agent | Decision: the MVP is shown locally; positioning deferred beyond the MVP (user, 2026-11-23). What "locally" means for hosting is open (BRD OQ-014). | PM |

Further open questions: target segment and buyer, baselines and success targets, minimum alert and message content, failure history, repository visibility and redaction for issues, alert volumes against GitHub limits. These are carried into the BRD as OQ items.

## Next

The BRD draft consumes this document: `.copilot-tracking/brd-sessions/sre-bot-managed-service-brd.md`.
