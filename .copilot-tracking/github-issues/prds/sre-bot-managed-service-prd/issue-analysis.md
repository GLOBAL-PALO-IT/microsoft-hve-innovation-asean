<!-- markdownlint-disable-file -->
<!-- markdown-table-prettify-ignore-start -->
# Issue Analysis — SRE Bot Managed Service PRD

## Planning basis

* **Platform / scope:** GitHub Issues, `GLOBAL-PALO-IT/microsoft-hve-innovation-asean`.
* **Lens:** generic platform-native decomposition.
* **PRD:** `sre-bot-managed-service-prd.md`, PRD-2026-Q4-001 v0.2.0, draft.
* **Supporting evidence:** session context and context summary; 2026-11-23 context discovery research; draft BRD; canonical partial `map-journey.md`; `sre-bot-managed-service-mvp-plan.md`.
* **Evidence boundary:** the journey is a proposed experience, not validated research. The source deck is cited by slide and is not reproduced here. No customer, tenant, incident, credential, pricing, or performance data was supplied.
* **Readiness limitation:** PRD validation has not been re-evaluated, human review is outstanding, BRD and UX are drafts, and the supporting RPI plan's independent critique is pending. This plan is a draft handoff, not production readiness or implementation approval.

## Blocking scope reconciliation

The supporting RPI plan records prior user direction to include **permission-aware retrieval, human escalation, and confirmation before external actions**. PRD DD-001 instead limits MVP analysis to supplied alert/Teams content, and the confirmed MVP boundary says the engineer performs recovery; the service does not execute it. The PRD does not define a retrieval source or scope, escalation behavior, or a service action that would invoke external-action confirmation.

Do not invent a retrieval scope, imply that the RPI direction was withdrawn, or silently treat it as covered by the payload-only PRD. **IS001, IS004, and IS005 remain `needs_review` and non-executable for this conflict.** The required evidence is a user/owner decision that reconciles the directions in an **approved PRD**, including whether retrieval is in or out of scope, what human escalation means, and how confirmation applies while the no-service-executed-recovery boundary remains in force. Until that evidence exists, preserve the current `map-journey.md` as the canonical experience artifact; do not replace or duplicate it.

## Candidate outcomes and requirement trace

The candidates below are a flattened planning view. Feature/Task type suggestions and parent relationships are not validated; they are not create payloads. Candidate ACs refer to the PRD's existing AC IDs; no acceptance tests or thresholds are invented.

| Candidate | Conventional issue title | PRD requirement / AC mapping | Logical grouping suggestion | Readiness |
|---|---|---|---|---|
| IS001 | `feat: establish the reviewed SRE Bot MVP contract` | Supporting handoff task from implementation-plan P01-T01; no new PRD FR or AC. Preserve the supported triggers, payload-only analysis, engineer-controlled recovery, traceability, evidence distinctions, and source review gate. | Candidate Feature/tracking issue only if issue types and sub-issues are confirmed; otherwise omit parent. | `needs_review` — supporting artifacts require human review and scope conflict must be reconciled in an approved PRD. |
| IS002 | `feat: accept and normalize the supported incident events` | FR-001, FR-002, FR-003, FR-012, NFR-010; AC-001, AC-002, AC-003, AC-015, AC-021. | Candidate Task beneath IS001. | `needs_review` — proposed fields and parenting unverified; OQ-015 affects event schema and integration. |
| IS003 | `feat: identify incidents and reuse existing records` | FR-004, FR-005, FR-011; AC-004, AC-005, AC-020. | Candidate Task beneath IS001. | `needs_review` — OQ-006 leaves matching/fingerprint rule unknown. |
| IS004 | `feat: produce grounded analysis and non-executing recovery proposals` | FR-006, FR-007, FR-014, NFR-003, NFR-004; AC-006, AC-007, AC-012, AC-013, AC-024. | Candidate Task beneath IS001. | `needs_review` — NFR-003/004 are proposed; OQ-005 and OQ-010 leave evidence, accuracy, identity, permission, emergency-stop and action-limit decisions open; permission-aware retrieval direction conflicts with payload-only DD-001. |
| IS005 | `feat: record engineer decisions and auditable actions` | FR-008, FR-009, FR-013, NFR-001, NFR-002, NFR-007; AC-008, AC-009, AC-011, AC-014, AC-022, AC-023, AC-025. | Candidate Task beneath IS001. | `needs_review` — FR-008/NFR-007 are proposed; OQ-010/011/012 leave decision authority, data rules, severity and time limits unknown; escalation and external-action confirmation direction needs reconciliation. |
| IS006 | `feat: report incident status and expose operational health` | FR-010, NFR-005, NFR-008, NFR-009; AC-010, AC-016, AC-018, AC-019. | Candidate Task beneath IS001. | `needs_review` — NFR-008/009 are proposed; OQ-009 leaves latency, volume, availability and unavailable/throttled behavior undefined. |
| IS007 | `test: validate accessibility and the workshop incident workflow` | NFR-006; AC-017. Also supports GOAL-003 and implementation-plan P03-T01 using the UX artifact's proposed synthetic scenarios and accessibility checks. | Candidate Task beneath IS001. | `needs_review` — NFR-006 is proposed; OQ-014 and the workshop date/goal OQ-017 remain open. No user validation has occurred. |
| IS008 | `docs: assess provisional Marketplace and Agent Store readiness` | Not a PRD requirement. Supporting implementation-plan P03-T02 only: record assumptions and evidence still needed; do not publish. | Candidate Task beneath IS001. | `needs_review` — current Microsoft guidance, Partner Center state, tenant feasibility, packaging, security and review requirements are unverified. |

### Complete requirement coverage

* **FRs:** FR-001–FR-003 → IS002; FR-004–FR-005 and FR-011 → IS003; FR-006–FR-007 and FR-014 → IS004; FR-008–FR-009 and FR-013 → IS005; FR-010 → IS006; FR-012 → IS002.
* **NFRs:** NFR-001, NFR-002, NFR-007 → IS005; NFR-003, NFR-004 → IS004; NFR-005, NFR-008, NFR-009 → IS006; NFR-006 → IS007; NFR-010 → IS002.
* **ACs:** AC-001–AC-003, AC-015, AC-021 → IS002; AC-004, AC-005, AC-020 → IS003; AC-006, AC-007, AC-012, AC-013, AC-024 → IS004; AC-008, AC-009, AC-011, AC-014, AC-022, AC-023, AC-025 → IS005; AC-010, AC-016, AC-018, AC-019 → IS006; AC-017 → IS007.
* **Explicit gap:** no issue is created for commercial pricing, a customer/buyer segment, differentiation, or publication. These are deferred/unknown in the PRD and are not MVP requirements. IS008 is a readiness-assessment deliverable from the implementation plan, not an approval or publication claim.

## Codebase context

The target workspace is the HVE Core methodology/tooling repository (`package.json` identifies the package as `hve-core`), containing documentation, skills, tests and tooling. A workspace search found no SRE Bot product source. It supports the artifact workflow and readiness handoff, but does not validate the proposed product architecture or implementation boundaries.

## Related GitHub Issues

All three scoped semantic searches recorded in `planning-log.md` returned zero results. No issue was available to hydrate. Therefore there are no assessed Match, Similar, Distinct, or Uncertain item pairs, and no existing labels, fields, or issue hierarchy could be observed.

## GitHub capability and field validation

| Capability / field | Read-only evidence | Planning decision |
|---|---|---|
| Target repository | Explicit user-specified owner/repo; scoped search calls succeeded. | Destination known; identity preflight remains incomplete. |
| Authenticated identity (`get_me`) | Tool was not surfaced by tool discovery. | `needs_review`; no identity claim. |
| Issue types (`list_issue_types`) | Tool was not surfaced by tool discovery; no type values returned. | Do not put an issue type in any create candidate. Logical `Feature` / `Task` suggestions remain unvalidated. |
| Labels (`get_label`) | Tool was not surfaced by tool discovery; no label values returned. | No label is proposed or assigned. Any create payload needing a repository label must wait for validation. |
| Sub-issue support / parenting | Search found no existing issues; read tools cannot confirm a parent/child pattern without an issue endpoint. | Flatten candidates; all suggested IS001-to-child links are `needs_review`; no Link operations. |
| `title`, `body` | Named fields and conventional-commit title/body semantics are specified in the GitHub backlog reference. | Candidate titles and local body drafts only; creates remain blocked by unresolved repository-required label/type/hierarchy validation. |
| Milestone | No milestone was returned by issue searches; no milestone strategy override was found. | No milestone proposed; don't infer a workshop milestone. |
| Assignees / priority | No named owner or validated priority field; GitHub reference describes priority as label-based. | None proposed. |
| Operation semantics | GitHub reference defines Create, Update, Link, Comment, Close, No Change; relationship links follow both endpoint creates. | No operation is dispatched; if later approved, use creates before links. |

Tool discovery returned only issue search and issue-read operations. Missing read-only capability is recorded rather than replaced with a different CLI or an assumed repository convention.

## Marketplace and Microsoft 365 Copilot Agent Store assumptions

Publication status is **provisional and unverified**. The workshop-local demonstration is not evidence of Marketplace or Agent Store eligibility or readiness. No Partner Center account/state, tenant, offer type, package, identity, permission, data-handling approval, current submission rule, or storefront review evidence was supplied or verified. The supporting implementation plan calls for a readiness assessment only.

Do not claim Partner Center approval, tenant feasibility, Marketplace/Agent Store approval, publication, commercial readiness, or differentiation from Azure SRE Agent. Positioning is deferred; the research notes capability overlap and leaves differentiation open. Resolve the appropriate owner, offer type/route, current requirements, tenant and identity feasibility, data/security controls, packaging, and review gates in the separate readiness assessment before any publication work.

## Human review and unresolved decisions

* Complete qualified review of the draft PRD, BRD, and partial UX journey; re-evaluate the blocked PRD validation gate and complete the implementation plan's pending independent critique.
* Obtain a user/owner decision, reflected in an approved PRD, reconciling permission-aware retrieval, human escalation, and confirmation before external actions with DD-001's supplied-content-only analysis and the engineer-executes-recovery boundary. Do not infer retrieval sources or behavior before that decision.
* Resolve the applicable PRD OQs before turning contingent criteria into production-ready work: OQ-001/002 (owner, customer, success targets), OQ-005/006 (payload and matching), OQ-009 (capacity/reliability), OQ-010/011/012 (identity, control, data rules, approvals), OQ-014/015/016 (accessibility, integration/event schema, local demonstration and edge behavior), OQ-017 (workshop timing and demo purpose).
* Confirm GitHub identity, org issue types, every label intended for use, repository issue type requirements, required create fields, and parent/sub-issue support.
* Reassess the hierarchy and convert candidates to executable payloads only after these gates. Keep source ACs marked proposed/contingent where the PRD does.

<!-- markdown-table-prettify-ignore-end -->
