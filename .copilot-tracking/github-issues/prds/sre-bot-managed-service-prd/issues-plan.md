<!-- markdownlint-disable-file -->
<!-- markdown-table-prettify-ignore-start -->
# Issues Plan — SRE Bot Managed Service PRD

## Status and destination

* GitHub Issues — `GLOBAL-PALO-IT/microsoft-hve-innovation-asean`.
* Plan status: review-only; no executable create payloads.
* Framework: generic platform-native decomposition.
* Analysis: `issue-analysis.md`.
* Handoff operations follow GitHub order: Create → Update → Link → Comment → Close → No Change.
* **Blocker:** read-only issue-type, label, and identity tools were not available; sub-issue support could not be observed because searches returned zero issues. All candidates and logical relationships are `needs_review`. No tracker mutation occurred.

## Capability gates

| Item | Proposed value / operation | Validation | Current disposition |
|---|---|---|---|
| Issue type | Candidate grouping `Feature`; candidate leaves `Task` | `list_issue_types` unavailable | Do not include `type`; `needs_review`. |
| Labels | No label selected | `get_label` unavailable; no repository labels read | Do not include labels; any create requiring a label is blocked pending validation. |
| Parent-child | IS001 as logical group for IS002–IS008 | No existing repository issue to inspect; sub-issue capability not confirmed | Flatten plan; do not issue Link operations. |
| Title/body | Conventional-commit title and markdown body draft | Field vocabulary is documented in the platform reference | Locally drafted only; no create payload issued. |
| Milestone | None | No milestone evidence discovered | Omit; do not infer one from workshop scope. |
| Assignee / priority | None | No assignee or priority convention confirmed; priority is label-based | Omit. |

## Candidate issues (not yet create operations)

The candidate descriptions and checklists below are derived from supplied artifacts. PRD acceptance criteria are retained verbatim with their source IDs; criteria that depend on unresolved decisions remain contingent and are not represented as implementation-ready. The candidates are not create payloads.

| Ref | Title | Logical role | Parent | Labels | Type | State |
|---|---|---|---|---|---|---|
| IS001 | `feat: establish the reviewed SRE Bot MVP contract` | Grouping candidate | none | none proposed | Feature candidate, `needs_review` | `needs_review` |
| IS002 | `feat: accept and normalize the supported incident events` | Delivery candidate | IS001 suggested, `needs_review` | none proposed | Task candidate, `needs_review` | `needs_review` |
| IS003 | `feat: identify incidents and reuse existing records` | Delivery candidate | IS001 suggested, `needs_review` | none proposed | Task candidate, `needs_review` | `needs_review` |
| IS004 | `feat: produce grounded analysis and non-executing recovery proposals` | Delivery candidate | IS001 suggested, `needs_review` | none proposed | Task candidate, `needs_review` | `needs_review` |
| IS005 | `feat: record engineer decisions and auditable actions` | Delivery candidate | IS001 suggested, `needs_review` | none proposed | Task candidate, `needs_review` | `needs_review` |
| IS006 | `feat: report incident status and expose operational health` | Delivery candidate | IS001 suggested, `needs_review` | none proposed | Task candidate, `needs_review` | `needs_review` |
| IS007 | `test: validate accessibility and the workshop incident workflow` | Validation candidate | IS001 suggested, `needs_review` | none proposed | Task candidate, `needs_review` | `needs_review` |
| IS008 | `docs: assess provisional Marketplace and Agent Store readiness` | Readiness-assessment candidate; not publication | IS001 suggested, `needs_review` | none proposed | Task candidate, `needs_review` | `needs_review` |

### Local candidate bodies and trace

#### IS001 — `feat: establish the reviewed SRE Bot MVP contract`

Establish a human-reviewed contract for the workshop MVP before implementation is treated as approved. This is a supporting planning deliverable from implementation-plan P01-T01, not a new PRD feature.

**Candidate review outcomes (not approved PRD ACs):**

- [ ] Qualified human reviewers review the source BRD, PRD, and canonical UX journey.
- [ ] The agreed MVP scope retains only Teams requests and Azure Monitor alerts.
- [ ] Analysis remains limited to supplied event content, and the engineer performs any recovery.
- [ ] Unsupported, unvalidated, or uncertain behavior remains explicitly identified.

**Trace:** implementation plan P01-T01; PRD draft, BRD draft, research, and partial canonical UX journey.

#### IS002 — `feat: accept and normalize the supported incident events`

Accept only the MVP's Teams requests and Azure Monitor alerts, retain the accepted event in a standard form, and verify declared integrations.

**Acceptance Criteria (PRD; AC-015 depends on declared integrations):**

- [ ] **AC-001:** Given a request made to the service in Teams, When it is received, Then it is accepted and the acceptance is observable on the incident record or in the thread.
- [ ] **AC-002:** Given an Azure Monitor alert, When it is received, Then it is accepted and the acceptance is observable on the incident record.
- [ ] **AC-003:** Given an accepted event from either source, When it is handled, Then it is held in one standard form that includes the source type, the time received and the content as received.
- [ ] **AC-015:** Given the service is running, When its integrations are inspected, Then it exchanges events with Microsoft Teams, Azure Monitor alerts and the incident record system named in CON-002.
- [ ] **AC-021:** Given an event of a trigger type other than Teams requests or Azure Monitor alerts, When it arrives, Then it is not accepted for handling in the MVP.

**Trace:** FR-001, FR-002, FR-003, FR-012, NFR-010; CON-001, CON-002, CON-007. Event fields and exchange details remain contingent on OQ-015.

#### IS003 — `feat: identify incidents and reuse existing records`

Identify incidents consistently, create a record when absent, and link recurrence to the matching record without asserting an unknown matching algorithm.

**Acceptance Criteria (PRD; AC-020 is blocked on the matching decision):**

- [ ] **AC-004:** Given the same alert content presented twice, When the service identifies the incident each time, Then both identifications are the same.
- [ ] **AC-005:** Given an identified incident with no incident record, When it is handled, Then a record is created.
- [ ] **AC-020:** Given an incident that already has a record, When it recurs, Then it is linked to that record and no second record is created.

**Trace:** FR-004, FR-005, FR-011. Matching/fingerprint rule is unresolved under OQ-006; AC-020 cannot be made implementation-ready until that decision is made.

#### IS004 — `feat: produce grounded analysis and non-executing recovery proposals`

Produce analysis from the supplied event content, identify its basis and limits, and present recovery for engineer review without service-executed recovery.

**Acceptance Criteria (PRD; AC-012/013/024 include proposed controls or behavior):**

- [ ] **AC-006:** Given an identified incident, When analysis completes, Then the analysis is available to the engineer and names the event content it relies on.
- [ ] **AC-007:** Given a completed analysis, When the service proposes recovery, Then the proposal is presented for review and no recovery action is carried out by the service.
- [ ] **AC-012:** Given the service identity, When its permitted operations are listed, Then they are limited to the five operations in CON-007 and an emergency stop exists (limits `Unknown`).
- [ ] **AC-013:** Given an alert or message containing instruction-like text, When the service handles it, Then it does not act on that text and any proposal still requires engineer review.
- [ ] **AC-024:** Given a failed step or an event with too little information, When the service responds, Then it states the failure or the limit and does not present an unsupported analysis.

**Trace:** FR-006, FR-007, FR-014, NFR-003, NFR-004; DD-001. NFR-003/004 and FR-014 have proposed status; content sufficiency and accuracy remain open under OQ-005. Identity, permission, emergency-stop, and action-limit decisions remain open under OQ-010.

#### IS005 — `feat: record engineer decisions and auditable actions`

Record each request/action and engineer proposal decision on the incident record; a pending or rejected proposal is not approval. Keep credentials out of code and records, and defer record redaction/retention behavior until decided.

**Acceptance Criteria (PRD; AC-014 depends on the undecided data rule):**

- [ ] **AC-008:** Given a presented proposal, When the engineer approves or rejects it, Then the decision is recorded on the incident record.
- [ ] **AC-009:** Given any request or service action, When it occurs, Then a corresponding entry exists on the incident record.
- [ ] **AC-011:** Given the service and its incident record, When they are inspected, Then no credential or secret appears in code or on the record.
- [ ] **AC-014:** Given incident content that falls under the decided classification rule, When it is written to the incident record, Then the rule's redaction applies (rule `Unknown`, OQ-011).
- [ ] **AC-022:** Given a presented proposal with no engineer decision, When time passes, Then the proposal stays pending and is not treated as approved.
- [ ] **AC-023:** Given a proposal the engineer rejects, When the rejection is made, Then no recovery is treated as approved and the rejection is recorded.
- [ ] **AC-025:** Given an entry on the incident record, When it is read, Then it identifies who or what acted and what the action was.

**Trace:** FR-008, FR-009, FR-013, NFR-001, NFR-002, NFR-007; CON-004, CON-007. Decision authority/time limits and classification/redaction/retention remain open under OQ-010/011/012. AC-014's rule is not implementable until OQ-011 is resolved.

#### IS006 — `feat: report incident status and expose operational health`

Report status/results in the originating Teams thread, make health and event-to-proposal timing observable, and define observable behavior during unavailable or throttled integrations.

**Acceptance Criteria (PRD; AC-016/018/019 require unresolved operational details):**

- [ ] **AC-010:** Given a request made in a Teams thread, When a step completes, Then the status or result appears in that thread.
- [ ] **AC-016:** Given the service is running, When health is inspected, Then telemetry about its own health is available and the signals are listed in the observability note (signals `Unknown`).
- [ ] **AC-018:** Given an accepted event and a presented proposal, When elapsed time is reviewed, Then it can be read from the incident record. Target `Unknown`.
- [ ] **AC-019:** Given the incident record or Teams is unavailable or throttled, When the service tries to record or reply, Then the behavior defined under NFR-009 occurs and can be observed.

**Trace:** FR-010, NFR-005, NFR-008, NFR-009; CON-005. Telemetry signals, targets, volume, latency, availability and retry/queue behavior remain unknown under OQ-009.

#### IS007 — `test: validate accessibility and the workshop incident workflow`

Validate the proposed experience with synthetic inputs and an approved test environment; assess plain-text meaning, keyboard/screen-reader use, status communication, zoom and recovery from errors. This is a validation plan, not a claim of user research or conformance.

**Acceptance Criteria (PRD; test approach remains a validation proposal):**

- [ ] **AC-017:** Given a service reply, When it is read as plain text without color or images, Then its meaning is unchanged.

**Trace:** NFR-006; implementation-plan P03-T01; UX journey validation priorities. Accessibility needs/conformance are unknown under OQ-014; the workshop date and demonstration goal are unknown under OQ-017.

#### IS008 — `docs: assess provisional Marketplace and Agent Store readiness`

Prepare a reviewable readiness assessment for Microsoft Marketplace and the Microsoft 365 Copilot Agent Store. Record unresolved assumptions and evidence needs; do not publish or represent readiness as approved.

**Candidate completion evidence from the supporting implementation plan:** distinguish a workshop-local demo from publication; assess current Microsoft requirements, Partner Center state, tenant feasibility, identity/permissions, data handling, packaging and review; record Azure SRE Agent positioning as deferred/open.
**Trace:** implementation-plan P03-T02; PRD OQ-001, OQ-007, OQ-008, OQ-011, OQ-013, OQ-016 and supporting BRD/research. This is not a PRD requirement or an approval.

## Dependencies and proposed hierarchy

* Logical grouping only: IS001 → IS002–IS008. Issue types and sub-issue support/parenting are unvalidated; all links are `needs_review`, and the actual backlog remains flat until confirmed.
* Suggested sequencing: human review (IS001) → supported event/record flow (IS002–IS003) → analysis and decision/audit flow (IS004–IS006) → synthetic/accessibility validation (IS007) → separate provisional publication assessment (IS008).
* No estimate, milestone, assignee, priority label, release commitment, or production-ready status is inferred.
* Out of scope: automatic recovery or approval bypass; extra log retrieval, uploads/screenshots, non-Azure triggers/models, email/web intake, commercial launch, and actual publishing.

<!-- markdown-table-prettify-ignore-end -->
