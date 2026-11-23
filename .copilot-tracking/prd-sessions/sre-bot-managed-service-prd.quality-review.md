Agent is idle (waiting for messages). agent_id: 9b92bee9-4f62-4484-a2af-b3bc052e129f, agent_type: PRD Quality Reviewer, status: idle, description: Review PRD draft quality, elapsed: 146s, total_turns: 1

[Turn 0]
Both payloads are below. Nothing was written to disk, and the confidential deck (C3) was not read. I could not verify the "Fact (C3 …)" evidence labels, so I accepted them as stated.

The verdict is **FAIL** because GOAL-001 has no measurable KPI (one `RISK` finding). `validate_exit` is therefore `BLOCKED`. The PRD's `Unknown` values were not penalised as missing invented numbers. They are flagged only where they stop a SMART goal or a measurable NFR from being scored. I took `skill_version` "1.0" from the contract example, because no bundle version was supplied.

```yaml
schema_version: PRD_STANDARD_FINDINGS_V1
assessment_id: requirements-quality-PRD-2026-Q4-001-20260523T050340Z
assessed_at: "2026-11-23T05:03:40Z"
prd:
  id: PRD-2026-Q4-001
  version: 0.1.0
  phase: Validate
  artifact_path: .copilot-tracking/prd-sessions/sre-bot-managed-service-prd.md
standard:
  skill_name: requirements-quality
  skill_version: "1.0"
mode: plan
assessment_outcome: plan
overall_status: RISK
summary_counts:
  RISK: 1
  CAUTION: 22
  COVERED: 7
  NOT_APPLICABLE: 1
findings:
  - finding_id: req-qual-001
    checklist_item: smart-product-goal-measurable
    requirement_id: GOAL-001
    status: RISK
    severity: HIGH
    location:
      section: "Product Goals"
      line_range: L110-L115
    finding: GOAL-001 KPI is only a candidate; baseline, target and horizon are Unknown (OQ-002), so Measurable and Achievable cannot be shown. M-01, M-02 and M-04 share the same gap.
    recommendation: Obtain a baseline and target (or a human-accepted proxy) for M-01 before Finalize. Until then record GOAL-001 as SMART-incomplete with an owner and date for OQ-002.
  - finding_id: req-qual-002
    checklist_item: smart-product-goal-time-bound
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Product Goals"
      line_range: L107-L131
    finding: GOAL-001, GOAL-002 and GOAL-003 have no dated horizon. GOAL-003 says "Workshop (date Unknown)".
    recommendation: Add a workshop date or review window to each goal, or record a waiver reason.
  - finding_id: req-qual-003
    checklist_item: smart-evaluation-completed
    status: CAUTION
    severity: LOW
    location:
      section: "Product Goals - SMART Evaluation"
      line_range: L133-L141
    finding: SMART checklist is unchecked and `product_goal_smart_status` is "deferred" while the PRD is in Validate.
    recommendation: Complete the checklist per goal from this review and update the frontmatter status.
  - finding_id: req-qual-004
    checklist_item: smart-product-goal-specific
    requirement_id: GOAL-002
    status: CAUTION
    severity: LOW
    location:
      section: "Product Goals"
      line_range: L118-L123
    finding: GOAL-002 joins two outcomes (engineer control and a traceable record). The KPI measures only record completeness.
    recommendation: Split the goal, or add an engineer-control measure (M-04 approval rate is only a candidate).
  - finding_id: req-qual-005
    checklist_item: nist-800-160-privacy-coverage
    status: CAUTION
    severity: HIGH
    location:
      section: "Non-Functional Requirements - Privacy"
      line_range: L190-L192
    finding: No privacy NFR exists, although incident content, possibly sensitive, is written to a GitHub record (RK-005, G-07). Classification and retention are Unknown (OQ-011).
    recommendation: Add a Proposed privacy NFR covering classification, redaction and retention, with a verification AC. Resolve OQ-011 before Finalize.
  - finding_id: req-qual-006
    checklist_item: nist-800-160-performance-reliability-coverage
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Non-Functional Requirements - Performance and Capacity, Reliability and Resilience"
      line_range: L176-L182
    finding: There are no performance or reliability NFRs. Latency, volume, availability and outage behaviour are Unknown (OQ-009), so none can be measured. FC-003 and FC-004 behaviour is also Unknown.
    recommendation: Add NFR placeholders marked Unknown, each with a closing owner. Define the metric to be measured even where the target stays Unknown.
  - finding_id: req-qual-007
    checklist_item: nist-800-160-scalability-portability-compatibility-coverage
    status: CAUTION
    severity: LOW
    location:
      section: "Non-Functional Requirements - Scalability, Compatibility, Portability"
      line_range: L194-L222
    finding: Scalability and portability have no NFR. Compatibility is prose only, with no NFR-### identifier.
    recommendation: Give the compatibility statement an NFR ID with an AC. State explicitly that scalability and portability are deferred.
  - finding_id: req-qual-008
    checklist_item: iso-29148-verifiable
    requirement_id: NFR-005
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Non-Functional Requirements - Observability, Usability"
      line_range: L206-L212
    finding: NFR-005 ("reports operational telemetry") names no signals or thresholds and has no AC. NFR-006 (Proposed) also has no AC or test method.
    recommendation: Add ACs naming at least the signals or a plain-text review check, and keep the thresholds Unknown if unsupported.
  - finding_id: req-qual-009
    checklist_item: iso-29148-verifiable
    requirement_id: AC-012
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Acceptance Criteria; Constraints"
      line_range: L249, L312
    finding: AC-012 and NFR-004 refer to "the five operations in CON-007", but CON-007 does not list them. They appear only in G-04, so the AC cannot be tested against CON-007 alone.
    recommendation: List the five operations in CON-007 and reference it from G-04 and AC-012.
  - finding_id: req-qual-010
    checklist_item: iso-29148-verifiable
    requirement_id: AC-004
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Acceptance Criteria"
      line_range: L304-L306
    finding: AC-004 to AC-006 check only that an outcome exists. The fingerprint and match rule (OQ-006) and the criteria for a root-cause analysis (OQ-005) are Unknown, so correctness cannot be tested.
    recommendation: Add a Proposed testable check for each (for example, same input yields the same fingerprint; the analysis cites the event content used). Keep accuracy thresholds Unknown until evidenced.
  - finding_id: req-qual-011
    checklist_item: iso-29148-unambiguous
    requirement_id: FR-003
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Functional Requirements"
      line_range: L159-L161
    finding: "Standard event form" (FR-003, AC-003) has no defined fields, and "accepted for handling" (FR-001, FR-002) has no observable meaning.
    recommendation: Define the minimum event fields, and state what "accepted" is observed as (for example, a record entry or an acknowledgement).
  - finding_id: req-qual-012
    checklist_item: iso-29148-complete
    requirement_id: FR-008
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Functional Requirements"
      line_range: L166
    finding: FR-008 has an Unknown approval mechanism. Approver authority, severity rules and approval time limits are open (OQ-010, OQ-012), so the requirement is incomplete.
    recommendation: Specify who may approve and what happens without a decision, or mark FR-008 explicitly Proposed until OQ-010 and OQ-012 close.
  - finding_id: req-qual-013
    checklist_item: iso-29148-singular
    requirement_id: FR-005
    status: CAUTION
    severity: LOW
    location:
      section: "Functional Requirements; Acceptance Criteria"
      line_range: L163, L305
    finding: FR-005 and AC-005 combine create-new-record and link-recurrence behaviours. AC-002 also combines accept and reject.
    recommendation: Split into separate FRs or ACs so each can be pass/failed independently.
  - finding_id: req-qual-014
    checklist_item: iso-29148-feasible
    requirement_id: FR-009
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Functional Requirements; Constraints"
      line_range: L167, L247
    finding: FR-009 requires "every" request and action to be recorded, but CON-005 sets GitHub rate limits and FC-003 and FC-004 behaviour is Unknown. The absolute cannot be shown feasible.
    recommendation: State the behaviour when the record system is unavailable or throttled (queue, retry or alert), or qualify "every".
  - finding_id: req-qual-015
    checklist_item: iso-29148-correct
    requirement_id: FR-006
    status: CAUTION
    severity: MEDIUM
    location:
      section: "User Journeys; Functional Requirements"
      line_range: L77-L86, L164
    finding: J2 follows the deck ("pulling the log now"), but DD-001 limits analysis to event content. The conflict is unresolved (OQ-005, RK-007), so FR-006 may not match the intended behaviour.
    recommendation: Resolve OQ-005 and correct J2 or the deck wording so J2 and FR-006 agree.
  - finding_id: req-qual-016
    checklist_item: iso-29148-appropriate
    requirement_id: FR-004
    status: CAUTION
    severity: LOW
    location:
      section: "Functional Requirements"
      line_range: L162-L165
    finding: FR-004, FR-006 and FR-007 embed deck command names (`/detect`, `/analysis`, `/recover`), which conflicts with DD-002 (avoid naming implementation).
    recommendation: Move command names to the Glossary or notes and keep FR text behavioural.
  - finding_id: req-qual-017
    checklist_item: con-non-negotiable-condition
    requirement_id: CON-002
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Constraints"
      line_range: L239-L249
    finding: The constraints table has no non-negotiability field. Only CON-002 says it is unconfirmed (OQ-007), and it conflicts with the open Jira question (OQ-003). Affected boundary appears only as "Impact".
    recommendation: Add a "Non-negotiable because" and "Affected boundary" column. Record any constraint that cannot be confirmed as Proposed.
  - finding_id: req-qual-018
    checklist_item: con-separation-from-dd
    requirement_id: CON-003
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Constraints; Design Decisions"
      line_range: L244-L245
    finding: CON-002 and CON-003 (platform, Teams, GitHub Issues, Azure OpenAI) are technology choices from the deck. Without a confirmed imposing source they read as design decisions rather than constraints (DD-002).
    recommendation: Confirm an external imposer for each, or move it to a DD-### entry.
  - finding_id: req-qual-019
    checklist_item: con-separation-from-fr
    requirement_id: CON-001
    status: CAUTION
    severity: LOW
    location:
      section: "Constraints; Scope"
      line_range: L243, L248
    finding: CON-001 restates scope, and CON-006 is a schedule constraint with no date. Both duplicate Scope and MVP framing.
    recommendation: Keep scope in the Scope section. Retain CON-006 only with a dated boundary or an explicit Unknown.
  - finding_id: req-qual-020
    checklist_item: con-imposing-source
    requirement_id: CON-004
    status: CAUTION
    severity: LOW
    location:
      section: "Constraints"
      line_range: L246
    finding: CON-004 names "Session context" as the imposing source, and it governs repository handling rather than the product boundary.
    recommendation: Name the actual imposer (for example, the deck's confidentiality marking or an owner policy) or move it to document handling notes.
  - finding_id: req-qual-021
    checklist_item: taxonomy-namespace-integrity
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Data and AI Guardrails; Proposed decisions"
      line_range: L98-L105, L224-L237
    finding: G-01 to G-08 are untagged requirement-like statements that duplicate FR-007, NFR-001, CON-003 and CON-007. They use a two-digit prefix outside the taxonomy, and PD-001 and PD-002 are off-taxonomy decisions.
    recommendation: Fold each guardrail into an FR, NFR or CON, or cross-reference it. Record proposed decisions as DD-### with a Proposed status.
  - finding_id: req-qual-022
    checklist_item: dd-product-decision-scope
    requirement_id: DD-004
    status: CAUTION
    severity: LOW
    location:
      section: "Design Decisions"
      line_range: L88-L96
    finding: DD-003, DD-004 and DD-005 are authoring or process conventions (artifact location, numbering), not product design decisions.
    recommendation: Move them to document metadata or authoring notes. Keep DD-001 and DD-002 as the design decisions.
  - finding_id: req-qual-023
    checklist_item: traceability-brd-source
    requirement_id: FR-003
    status: CAUTION
    severity: LOW
    location:
      section: "Functional Requirements; BRD-to-PRD Mapping"
      line_range: L161, L359-L372
    finding: FR-003 has no BRD source. The BRD-to-PRD mapping omits CON-007 and PRD-only NFR-003 to NFR-006.
    recommendation: Mark these as PRD-originated in the mapping, or add the corresponding BRD requirement.
  - finding_id: req-qual-024
    checklist_item: con-valid-imposed-constraint
    requirement_id: CON-005
    status: COVERED
    severity: N/A
    location:
      section: "Constraints"
      line_range: L247
    finding: CON-005 has an external imposing source (GitHub), a technical category and a named impact (FR-009). It is a genuine imposed constraint, not a requirement.
    recommendation: null
  - finding_id: req-qual-025
    checklist_item: identifier-format-and-uniqueness
    status: COVERED
    severity: N/A
    location:
      section: "Whole document"
    finding: FR-001 to FR-010, AC-001 to AC-013, NFR-001 to NFR-006, CON-001 to CON-007, GOAL-001 to GOAL-003 and DD-001 to DD-005 are unique and three-digit. There are no gaps or cross-namespace reuse.
    recommendation: null
  - finding_id: req-qual-026
    checklist_item: fr-ac-coverage
    status: COVERED
    severity: N/A
    location:
      section: "Traceability Matrix - FR-to-AC Coverage"
      line_range: L325-L340
    finding: All 10 FRs map to at least one AC (100.0%, above the 80.0% threshold). AC-to-FR references are consistent.
    recommendation: null
  - finding_id: req-qual-027
    checklist_item: fr-goal-traceability
    status: COVERED
    severity: N/A
    location:
      section: "Traceability Matrix - FR-to-GOAL Alignment"
      line_range: L342-L357
    finding: All 10 FRs link to at least one valid GOAL (100.0%, meeting the 100.0% target). No dangling goal references.
    recommendation: null
  - finding_id: req-qual-028
    checklist_item: nist-800-160-security-coverage
    status: COVERED
    severity: N/A
    location:
      section: "Non-Functional Requirements - Security"
      line_range: L184-L188
    finding: Security has NFR-001, NFR-003 and NFR-004, each verifiable through AC-011, AC-013 and AC-012. Proposed status is clearly marked.
    recommendation: null
  - finding_id: req-qual-029
    checklist_item: acceptance-criteria-form
    status: COVERED
    severity: N/A
    location:
      section: "Acceptance Criteria"
      line_range: L297-L313
    finding: AC-001 to AC-013 use Given/When/Then and each names the FR or NFR it covers.
    recommendation: null
  - finding_id: req-qual-030
    checklist_item: guardrail-consistency-human-approval
    requirement_id: FR-007
    status: COVERED
    severity: N/A
    location:
      section: "Functional Requirements; Design Decisions"
      line_range: L165, L307
    finding: DD-001, FR-007, AC-007 and G-01 consistently state that the service proposes and the engineer executes.
    recommendation: null
  - finding_id: req-qual-031
    checklist_item: feasibility-candidate-disposition
    status: NOT_APPLICABLE
    severity: N/A
    location:
      section: "Traceability Matrix - Feasibility Candidate Disposition"
      line_range: L321-L323
    finding: No feasibility-to-PRD handoff was consumed.
    recommendation: null
iso_29148_attributes:
  necessary: 3
  appropriate: 2
  unambiguous: 2
  complete: 1
  singular: 2
  feasible: 2
  verifiable: 2
  correct: 2
  conforming: 2
nist_800_160_nfr_categories:
  performance_and_capacity: false
  reliability_and_resilience: false
  security: true
  privacy: false
  scalability_and_elasticity: false
  maintainability_and_operability: true
  observability: true
  usability_and_accessibility: true
  compatibility_and_interoperability: false
  portability: false
smart_product_goals:
  - goal_id: GOAL-001
    statement: Reduce the manual effort and elapsed time for an on-call engineer to reach a proposed recovery.
    specific: true
    measurable: false
    achievable: false
    relevant: true
    time_bound: false
    overall: FAIL
  - goal_id: GOAL-002
    statement: Keep an engineer in control of recovery and keep a traceable record of every request and action.
    specific: true
    measurable: true
    achievable: true
    relevant: true
    time_bound: false
    overall: FAIL
  - goal_id: GOAL-003
    statement: Demonstrate the MVP alert-to-incident-record path at the HVE workshop.
    specific: true
    measurable: true
    achievable: true
    relevant: true
    time_bound: false
    overall: FAIL
fr_ac_coverage:
  fr_total: 10
  fr_with_ac: 10
  coverage_pct: 100.0
notes: >-
  The PRD intentionally uses Unknown and Proposed labels, and I did not penalise missing invented values. The Unknowns that block measurable NFRs or SMART goals are flagged in req-qual-001, -005, -006, -008 and -010.
  GOAL-001 achievable is marked false only because the baseline and target are Unknown (it cannot be assessed). GOAL-003 relevant is marked true, but the PRD labels the demonstration intent an Assumption.
  Deck evidence (C3) is confidential and was not read, so the Fact citations are unverified.
  NIST presence is strict. Compatibility is prose without an NFR ID, and Maintainability counts via NFR-002 (auditability). "unambiguous" and "verifiable" scored 2 at the presence level. All four core ISO attributes meet the minimum of 2.
```

```yaml
schema_version: PRD_QUALITY_REPORT_V1
report_id: PRD-2026-Q4-001-quality-20260523T050340Z
generated_at: "2026-11-23T05:03:40Z"
prd:
  id: PRD-2026-Q4-001
  version: 0.1.0
  phase: Validate
  artifact_path: .copilot-tracking/prd-sessions/sre-bot-managed-service-prd.md
overall_status: FAIL
decision_thresholds:
  iso_29148_core_min_score: 2
  fr_to_ac_min_pct: 80.0
  fr_to_goal_target_pct: 100.0
gate_decisions:
  validate_exit: BLOCKED
  finalize_exit: NOT_EVALUATED
summary_counts:
  RISK: 1
  CAUTION: 22
  COVERED: 7
  NOT_APPLICABLE: 1
severity_breakdown:
  CRITICAL: 0
  HIGH: 2
  MEDIUM: 12
  LOW: 9
standards_assessed:
  - skill_name: requirements-quality
    skill_version: "1.0"
    overall_status: RISK
    findings_count: 31
category_summaries:
  iso_29148:
    average_score: 2.0
    weakest_attribute: complete
    weakest_attribute_score: 1
  nist_800_160:
    covered_categories: 4
    missing_categories:
      - performance_and_capacity
      - reliability_and_resilience
      - privacy
      - scalability_and_elasticity
      - compatibility_and_interoperability
      - portability
  smart:
    goals_total: 3
    goals_passing: 0
    pass_rate_pct: 0.0
  fr_ac_coverage:
    fr_total: 10
    fr_with_ac: 10
    coverage_pct: 100.0
    threshold_pct: 80.0
  fr_goal_coverage:
    fr_total: 10
    fr_with_goal: 10
    goal_total: 3
    coverage_pct: 100.0
    target_pct: 100.0
    waiver_required: false
top_findings:
  - finding_id: req-qual-001
    standard: requirements-quality
    severity: HIGH
    status: RISK
    location:
      section: "Product Goals"
      line_range: L110-L115
    finding: GOAL-001 KPI is only a candidate; baseline, target and horizon are Unknown (OQ-002), so Measurable and Achievable cannot be shown.
    recommendation: Obtain a baseline and target (or a human-accepted proxy) for M-01 before Finalize, or record GOAL-001 as SMART-incomplete with an owner and date for OQ-002.
  - finding_id: req-qual-005
    standard: requirements-quality
    severity: HIGH
    status: CAUTION
    location:
      section: "Non-Functional Requirements - Privacy"
      line_range: L190-L192
    finding: No privacy NFR exists, although incident content, possibly sensitive, is written to a GitHub record (RK-005, G-07).
    recommendation: Add a Proposed privacy NFR covering classification, redaction and retention, with a verification AC. Resolve OQ-011.
  - finding_id: req-qual-002
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Product Goals"
      line_range: L107-L131
    finding: GOAL-001, GOAL-002 and GOAL-003 have no dated horizon.
    recommendation: Add a workshop date or review window to each goal, or record a waiver reason.
  - finding_id: req-qual-006
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Non-Functional Requirements - Performance and Capacity, Reliability and Resilience"
      line_range: L176-L182
    finding: There are no performance or reliability NFRs. Latency, volume, availability and outage behaviour are Unknown (OQ-009).
    recommendation: Add NFR placeholders marked Unknown, each with a closing owner. Define the metric to be measured even where the target stays Unknown.
  - finding_id: req-qual-008
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Non-Functional Requirements - Observability, Usability"
      line_range: L206-L212
    finding: NFR-005 names no signals or thresholds and has no AC. NFR-006 (Proposed) also has no AC.
    recommendation: Add ACs naming at least the signals or a plain-text review check, and keep the thresholds Unknown if unsupported.
  - finding_id: req-qual-009
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Acceptance Criteria; Constraints"
      line_range: L249, L312
    finding: AC-012 and NFR-004 refer to "the five operations in CON-007", but CON-007 does not list them.
    recommendation: List the five operations in CON-007 and reference it from G-04 and AC-012.
  - finding_id: req-qual-010
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Acceptance Criteria"
      line_range: L304-L306
    finding: AC-004 to AC-006 check only that an outcome exists. The fingerprint and match rule and the criteria for a root-cause analysis are Unknown.
    recommendation: Add a Proposed testable check for each. Keep accuracy thresholds Unknown until evidenced.
  - finding_id: req-qual-011
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Functional Requirements"
      line_range: L159-L161
    finding: "Standard event form" has no defined fields, and "accepted for handling" has no observable meaning.
    recommendation: Define the minimum event fields, and state what "accepted" is observed as.
  - finding_id: req-qual-012
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Functional Requirements"
      line_range: L166
    finding: FR-008 has an Unknown approval mechanism. Approver authority and time limits are open (OQ-010, OQ-012).
    recommendation: Specify who may approve and what happens without a decision, or mark FR-008 Proposed until OQ-010 and OQ-012 close.
  - finding_id: req-qual-014
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Functional Requirements; Constraints"
      line_range: L167, L247
    finding: FR-009 requires "every" request and action to be recorded, but CON-005 sets GitHub rate limits and outage behaviour is Unknown.
    recommendation: State the behaviour when the record system is unavailable or throttled, or qualify "every".
recommendations:
  - id: REC-1
    priority: P1
    target_section: "Product Goals; Success Metrics"
    action: Close OQ-002 for GOAL-001 (baseline, target, horizon for M-01). Add dates to all three goals. Split GOAL-002 or add a control measure. Complete the SMART checklist and update `product_goal_smart_status`.
    related_finding_ids:
      - req-qual-001
      - req-qual-002
      - req-qual-003
      - req-qual-004
  - id: REC-2
    priority: P1
    target_section: "Non-Functional Requirements - Privacy"
    action: Add a Proposed privacy NFR with a verification AC for classification, redaction and retention of incident content. Link it to OQ-011 and RK-005.
    related_finding_ids:
      - req-qual-005
  - id: REC-3
    priority: P2
    target_section: "Non-Functional Requirements"
    action: Add measurable placeholders for performance and reliability, give compatibility an NFR ID, and add ACs for NFR-005 and NFR-006. Keep thresholds Unknown where there is no evidence.
    related_finding_ids:
      - req-qual-006
      - req-qual-007
      - req-qual-008
  - id: REC-4
    priority: P2
    target_section: "Functional Requirements; Acceptance Criteria"
    action: Make ACs testable. Define the event form and the meaning of "accepted", and list the five record operations in CON-007. Specify the approval mechanism and authority (FR-008) and testable checks for fingerprint, match and analysis.
    related_finding_ids:
      - req-qual-009
      - req-qual-010
      - req-qual-011
      - req-qual-012
  - id: REC-5
    priority: P2
    target_section: "Functional Requirements; User Journeys"
    action: Resolve the log-pulling conflict between J2 and DD-001 (OQ-005). Define FR-009 behaviour under rate limiting or outage.
    related_finding_ids:
      - req-qual-014
      - req-qual-015
  - id: REC-6
    priority: P2
    target_section: "Constraints"
    action: Add non-negotiability and boundary fields. Confirm the imposing source for CON-002, CON-003 and CON-004 or move them to DD-###. Remove scope duplication in CON-001 and CON-006.
    related_finding_ids:
      - req-qual-017
      - req-qual-018
      - req-qual-019
      - req-qual-020
  - id: REC-7
    priority: P3
    target_section: "Taxonomy hygiene"
    action: Fold the G-01 to G-08 guardrails and the PD-001 and PD-002 proposals into FR, NFR, CON or DD entries. Move process-only DD-003 to DD-005 out of Design Decisions. Remove command names from FR text, split FR-005, and mark PRD-originated items in the BRD mapping.
    related_finding_ids:
      - req-qual-013
      - req-qual-016
      - req-qual-021
      - req-qual-022
      - req-qual-023
warnings:
  - id: WARN-001
    severity: WARNING
    message: Evidence labelled "Fact (C3 …)" rests on a confidential deck outside the workspace that this review did not read, so those citations are unverified.
  - id: WARN-002
    severity: WARNING
    message: The source BRD is an unreviewed draft with no BRD_TO_PRD_HANDOFF_V1, and PRD sign-off roles are unassigned. This PRD should be re-checked on any BRD change.
  - id: WARN-003
    severity: INFO
    message: Unknown and Proposed labels were respected and not penalised as missing invented numbers. The only hard block is GOAL-001 measurability. FR-to-AC (100.0%) and FR-to-goal (100.0%) coverage both meet their thresholds.
    related_finding_ids:
      - req-qual-001
notes: >-
  Validate phase, so finalize_exit is NOT_EVALUATED. FAIL is driven by one RISK (req-qual-001). The four core ISO 29148 attributes score 2 or higher, and FR-to-AC and FR-to-goal coverage both pass, so no threshold rule is breached. Re-run after REC-1 and REC-2. All three SMART goals currently fail, so Finalize will need either closed Unknowns or an approved waiver.
```