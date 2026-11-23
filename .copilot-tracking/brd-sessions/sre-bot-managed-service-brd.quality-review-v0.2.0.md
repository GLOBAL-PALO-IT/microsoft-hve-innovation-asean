Agent is idle (waiting for messages). agent_id: 4673c700-c2d5-4010-930c-2575bf79633a, agent_type: BRD Quality Reviewer, status: idle, description: Re-review BRD v0.2.0, elapsed: 167s, total_turns: 1

[Turn 0]
Both payloads are below. I read both contracts, the v0.2.0 BRD and the v0.1.0 review in full. I wrote no files and did not try to read the confidential deck (C3).

Each finding starts with a delta tag against v0.1.0, either `[NEW]`, `[UNCHANGED]` or `[RESOLVED]`. The contract has no delta field, so the tag is in the `finding` text. I reused the v0.1.0 IDs `req-qual-001` to `req-qual-023` so each finding can be traced. New findings are `req-qual-024` to `req-qual-029`.

## BRD_STANDARD_FINDINGS_V1

```yaml
schema_version: BRD_STANDARD_FINDINGS_V1
assessment_id: requirements-quality-BRD-2026-Q4-001-20261123T053431Z
assessed_at: "2026-11-23T05:34:31Z"
brd:
  id: BRD-2026-Q4-001
  version: 0.2.0
  phase: Define
  artifact_path: .copilot-tracking/brd-sessions/sre-bot-managed-service-brd.md
standard:
  skill_name: requirements-quality
  skill_version: "1.0"
mode: plan
assessment_outcome: plan
overall_status: RISK
summary_counts:
  RISK: 1
  CAUTION: 13
  COVERED: 14
  NOT_APPLICABLE: 1
findings:
  - finding_id: req-qual-001
    checklist_item: smart-business-goal-measurable
    requirement_id: BG-001
    status: RISK
    severity: HIGH
    location:
      section: "Business Goals (BG-001)"
      line_range: L99-L107
    finding: "[UNCHANGED] BG-001 still has Unknown baseline, target, timeframe, measurement source and owner (OQ-002, OQ-015, OQ-001). It cannot pass Measurable or Time-bound and Achievable cannot be judged. It anchors 6 of 10 FRs. NFR-004 and AC-015 now make the measure readable, but there is still no target. The Unknowns are honest, not invented, but they block SMART."
    recommendation: "Resolve OQ-002 (and OQ-009 for NFR-004, which asks for the same target). Record baseline, target, timeframe, source and owner for time-to-proposed-recovery, or record a human-approved waiver or dated deferral under Waivers."
  - finding_id: req-qual-002
    checklist_item: smart-business-goal-time-bound
    requirement_id: BG-002
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Business Goals (BG-002)"
      line_range: L111-L119
    finding: "[UNCHANGED] BG-002 is specific and measurable (both KPIs, source is the incident record). Timeframe and Owner are Unknown, so Time-bound fails."
    recommendation: "Add a review date or point and a named owner (OQ-001, OQ-015)."
  - finding_id: req-qual-003
    checklist_item: smart-business-goal-time-bound
    requirement_id: BG-003
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Business Goals (BG-003)"
      line_range: L123-L131
    finding: "[UNCHANGED] The workshop date is Unknown, so Time-bound fails. That demonstration is the intended goal is an Assumption, which weakens Relevant."
    recommendation: "Confirm the workshop date, owner and goal intent (OQ-015), or restate the goal."
  - finding_id: req-qual-004
    checklist_item: smart-business-goal-ownership
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Stakeholders; Business Goals; Sign-Off"
      line_range: L62-L76, L433-L451
    finding: "[UNCHANGED] All three goals have Owner Unknown. Sponsor and buyer are Unknown and every sign-off role is Unassigned. No one can accept the goals."
    recommendation: "Name a sponsor and per-goal owners (OQ-001) before Govern."
  - finding_id: req-qual-005
    checklist_item: iso-25010-category-coverage
    status: COVERED
    severity: N/A
    location:
      section: "Non-Functional Requirements"
      line_range: L183-L220
    finding: "[RESOLVED] Performance (NFR-004) and Reliability (NFR-003) now have qualitative NFRs with Unknown targets, and each absent category has a not-in-MVP note. 3 of 8 categories are present. The five missing categories are a qualitative flag only, per DD-012."
    recommendation: null
  - finding_id: req-qual-006
    checklist_item: iso-29148-complete-security-guardrails
    requirement_id: NFR-005
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Design Decisions (DD-008); Non-Functional Requirements (Security); Acceptance Criteria"
      line_range: L91, L211-L212, L280-L281
    finding: "[UNCHANGED, narrowed from HIGH] NFR-005 and NFR-006 with AC-016 and AC-017 now cover untrusted input, identity, permissions, emergency stop and action limits. All are Proposed, and DD-008 and OQ-010 still await a human decision. Until then the requirements are not baselined for a bot that ingests untrusted content."
    recommendation: "Get the human decision on DD-008 and OQ-010 and change NFR-005 and NFR-006 from Proposed to accepted. Limits may stay Unknown."
  - finding_id: req-qual-007
    checklist_item: iso-29148-unambiguous
    requirement_id: FR-002
    status: COVERED
    severity: N/A
    location:
      section: "Functional Requirements; Acceptance Criteria"
      line_range: L171, L266, L273
    finding: "[RESOLVED] FR-002 now defines recognition qualitatively (a recurrence maps to the same incident). AC-009 tests it, and the matching rule stays Unknown under OQ-006."
    recommendation: null
  - finding_id: req-qual-008
    checklist_item: iso-29148-verifiable
    requirement_id: FR-003
    status: COVERED
    severity: N/A
    location:
      section: "Functional Requirements; Acceptance Criteria"
      line_range: L172, L267
    finding: "[RESOLVED] FR-003 and AC-003 now require the analysis to name the event content it relies on. The accuracy threshold stays Unknown (OQ-005)."
    recommendation: null
  - finding_id: req-qual-009
    checklist_item: iso-29148-complete-functional-coverage
    status: COVERED
    severity: N/A
    location:
      section: "Functional Requirements; Process Models; Acceptance Criteria"
      line_range: L169-L179, L238-L257, L272-L275
    finding: "[RESOLVED] FR-008 (unsupported trigger), FR-009 (rejection) and FR-010 (failure or insufficient information) were added, each with an AC. FR-001 now states what AC-001 checks. The rejected-proposal revision path and sender notification remain Unknown (OQ-016)."
    recommendation: null
  - finding_id: req-qual-010
    checklist_item: iso-29148-necessary
    requirement_id: BR-004
    status: CAUTION
    severity: LOW
    location:
      section: "Business Rules"
      line_range: L154, L157
    finding: "[UNCHANGED, narrowed] BR-001 is now restated and its enforcing FRs are corrected (FR-004, FR-005, FR-009). BR-004's second sentence ('recovery without human approval is out of scope') still restates BR-001."
    recommendation: "Remove the overlapping sentence from BR-004, or merge BR-001 and BR-004 into one rule."
  - finding_id: req-qual-011
    checklist_item: con-br-separation
    requirement_id: CON-001
    status: COVERED
    severity: N/A
    location:
      section: "Business Rules; Constraints"
      line_range: L156, L228
    finding: "[RESOLVED] BR-003 is retired in favour of CON-001. The retired row is deliberately kept to avoid renumbering and is not flagged."
    recommendation: null
  - finding_id: req-qual-012
    checklist_item: con-imposing-source
    requirement_id: CON-004
    status: CAUTION
    severity: LOW
    location:
      section: "Constraints"
      line_range: L231
    finding: "[UNCHANGED, narrowed from MEDIUM] BR-005 is retired, so the duplication is gone. CON-004 still governs this engagement's repository handling, not the service. The imposer is the 'engagement evidence boundary (C1)', not a named party, and the text also bundles a negation about the deck marking (DD-006)."
    recommendation: "Name the imposing party. Either keep CON-004 as an engagement constraint with that boundary stated, or move it to engagement notes. Leave the DD-006 negation to DD-006."
  - finding_id: req-qual-013
    checklist_item: con-non-negotiable
    requirement_id: CON-002
    status: CAUTION
    severity: MEDIUM
    location:
      section: "Constraints"
      line_range: L229-L230, L233
    finding: "[UNCHANGED, partly addressed] A Non-negotiable column now exists. CON-002 and CON-003 are Unknown and their imposing source is 'design choice' with no external imposer. CON-006 is also Unknown. As written they are design decisions rather than confirmed constraints (OQ-007)."
    recommendation: "Resolve OQ-007. Keep CON-002 and CON-003 as constraints only if a customer or platform owner imposes them. Otherwise record them as design decisions (DD) scoped to the workshop."
  - finding_id: req-qual-014
    checklist_item: iso-29148-singular
    requirement_id: AC-001
    status: COVERED
    severity: N/A
    location:
      section: "Acceptance Criteria"
      line_range: L265, L272
    finding: "[RESOLVED] AC-001 is now positive-only. The negative case sits in AC-008 under FR-008."
    recommendation: null
  - finding_id: req-qual-015
    checklist_item: iso-29148-appropriate
    requirement_id: FR-007
    status: COVERED
    severity: N/A
    location:
      section: "Functional Requirements; Glossary"
      line_range: L176, L422-L426
    finding: "[RESOLVED] FR-007 and AC-007 now say 'conversation'. Command names moved to the Glossary, consistent with DD-002."
    recommendation: null
  - finding_id: req-qual-016
    checklist_item: nfr-verifiability
    requirement_id: NFR-001
    status: COVERED
    severity: N/A
    location:
      section: "Non-Functional Requirements (Security); Acceptance Criteria"
      line_range: L209, L276
    finding: "[RESOLVED] NFR-001 is reworded outcome-first and AC-012 verifies it. The NFR-to-AC matrix now maps all six NFRs."
    recommendation: null
  - finding_id: req-qual-017
    checklist_item: iso-25010-category-fit
    requirement_id: NFR-002
    status: COVERED
    severity: N/A
    location:
      section: "Non-Functional Requirements (Security)"
      line_range: L210, L277
    finding: "[RESOLVED] NFR-002 now sits under Security and requires each audit entry to identify the actor and the action. AC-013 checks this."
    recommendation: null
  - finding_id: req-qual-018
    checklist_item: requirement-id-integrity
    status: COVERED
    severity: N/A
    location:
      section: "All requirement sections"
    finding: "[UNCHANGED] FR-001 to FR-010, AC-001 to AC-017, NFR-001 to NFR-006, CON-001 to CON-007, BR-001 to BR-005 and BG-001 to BG-003 are canonical, unique and not cross-namespace. Frontmatter business_goal_ids match the body. Retired BR-003 and BR-005 are intentional placeholders and not counted as defects. DD and OQ are adjacent namespaces."
    recommendation: null
  - finding_id: req-qual-019
    checklist_item: fr-ac-coverage
    status: COVERED
    severity: N/A
    location:
      section: "Traceability Matrix (FR-to-AC)"
      line_range: L289-L304
    finding: "[UNCHANGED] All 10 FRs map to at least one AC, giving 100.0% against the 80.0% threshold. All 17 ACs are Not Started, as expected at Define."
    recommendation: null
  - finding_id: req-qual-020
    checklist_item: fr-bg-traceability
    status: COVERED
    severity: N/A
    location:
      section: "Traceability Matrix (FR-to-BG)"
      line_range: L306-L321
    finding: "[UNCHANGED] All 10 FRs link to at least one valid BG. The FR table and the matrix agree, giving 100.0%."
    recommendation: null
  - finding_id: req-qual-021
    checklist_item: con-imposing-source
    requirement_id: CON-005
    status: COVERED
    severity: N/A
    location:
      section: "Constraints"
      line_range: L232
    finding: "[UNCHANGED] CON-005 names an external imposer (GitHub), the affected boundary, non-negotiability, a category and its evidence. It is separate from the business rules and NFRs."
    recommendation: null
  - finding_id: req-qual-022
    checklist_item: iso-29148-conforming-evidence-labelling
    status: COVERED
    severity: N/A
    location:
      section: "Document-wide (evidence labels, DD-003)"
    finding: "[UNCHANGED] Unsupported values are Unknown, items carry Fact, Assumption, Decision or Proposed labels, and no numbers are invented. Frontmatter business_goal_smart_status 'fail' now matches the body, where v0.1.0 said 'deferred'."
    recommendation: null
  - finding_id: req-qual-023
    checklist_item: outcome-hypothesis-handoff
    status: NOT_APPLICABLE
    severity: N/A
    location:
      section: "Outcome Hypothesis Handoff Provenance"
      line_range: L144-L146
    finding: "[UNCHANGED] No OUTCOME_HYPOTHESIS_TO_BRD_HANDOFF_V1 payload was supplied and the BRD says so."
    recommendation: null
  - finding_id: req-qual-024
    checklist_item: iso-29148-correct-cross-reference
    status: CAUTION
    severity: LOW
    location:
      section: "Non-Functional Requirements (Compatibility)"
      line_range: L197
    finding: "[NEW] Compatibility says exchange formats are Unknown under OQ-015, but OQ-015 asks about the workshop date and BG-003 intent. No open question covers exchange formats."
    recommendation: "Add an open question for exchange formats, or cite the correct existing OQ."
  - finding_id: req-qual-025
    checklist_item: iso-29148-singular
    requirement_id: NFR-006
    status: CAUTION
    severity: LOW
    location:
      section: "Non-Functional Requirements (Security); Acceptance Criteria"
      line_range: L212, L281
    finding: "[NEW] NFR-006 and AC-017 bundle four needs: a defined identity, least access, an emergency stop and action limits. They cannot be tracked or accepted separately."
    recommendation: "Split into one requirement and one AC per need, for example identity and least access, emergency stop, and action limits."
  - finding_id: req-qual-026
    checklist_item: iso-29148-verifiable
    requirement_id: FR-001
    status: CAUTION
    severity: LOW
    location:
      section: "Functional Requirements; Acceptance Criteria"
      line_range: L170, L265
    finding: "[NEW] 'Accepted' is observable either on the incident record or as a conversation acknowledgement (Proposed). The alternatives leave the pass condition open, so a tester cannot tell which one must appear."
    recommendation: "Pick one observable for FR-001 and AC-001, or state the condition under which each applies."
  - finding_id: req-qual-027
    checklist_item: iso-29148-complete-functional-coverage
    requirement_id: BR-002
    status: CAUTION
    severity: LOW
    location:
      section: "Process Models; Business Rules; Functional Requirements"
      line_range: L238-L257, L155, L174-L175
    finding: "[NEW] The process model records the engineer carrying out recovery on the incident record. BR-002 and FR-006 cover only requests and service actions, and FR-005 records approval or rejection, so nothing requires recording that execution."
    recommendation: "Add a requirement (or extend FR-005 or FR-006) to record the engineer's execution outcome, or remove that dotted line from the process model."
  - finding_id: req-qual-028
    checklist_item: con-imposing-source
    requirement_id: CON-007
    status: CAUTION
    severity: LOW
    location:
      section: "Constraints; Design Decisions"
      line_range: L234, L90
    finding: "[NEW] CON-007 restates DD-007 (the MVP is shown locally). Its imposer is a user decision rather than an external party, the category is Technical though it is a demonstration scope, and its hosting implication is open (OQ-014). It overlaps CON-006 and BG-003."
    recommendation: "Keep it as DD-007 only, or reclassify CON-007 as Scope and state the boundary once OQ-014 resolves."
  - finding_id: req-qual-029
    checklist_item: iso-29148-verifiable
    requirement_id: AC-014
    status: CAUTION
    severity: LOW
    location:
      section: "Acceptance Criteria"
      line_range: L278-L279, L281
    finding: "[NEW] AC-014 tests 'the behavior defined under NFR-003', which is Unknown (OQ-009). AC-015 and AC-017 have Unknown targets and limits. The ACs are honest but cannot be run until those values exist."
    recommendation: "Mark AC-014, AC-015 and AC-017 as blocked by OQ-009 and OQ-010. Complete them when the values are decided."
iso_29148_attributes:
  necessary: 3
  appropriate: 2
  unambiguous: 2
  complete: 2
  singular: 2
  feasible: 2
  verifiable: 2
  correct: 2
  conforming: 3
iso_25010_categories:
  functional_suitability: false
  performance_efficiency: true
  compatibility: false
  usability: false
  reliability: true
  security: true
  maintainability: false
  portability: false
smart_business_goals:
  - goal_id: BG-001
    statement: "Reduce the manual effort and elapsed time for on-call engineers to reach a proposed recovery for an incident."
    specific: true
    measurable: false
    achievable: false
    relevant: true
    time_bound: false
    overall: FAIL
  - goal_id: BG-002
    statement: "Keep an engineer in control of recovery and keep a traceable record of every request and bot action."
    specific: true
    measurable: true
    achievable: true
    relevant: true
    time_bound: false
    overall: FAIL
  - goal_id: BG-003
    statement: "Demonstrate the MVP alert-to-incident-record path for the HVE workshop."
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
notes: "Unknown and Proposed values were graded as honest gaps, not penalised as missing invented numbers. Unknowns raise findings only where they block SMART (BG-001 RISK, BG-002 and BG-003 CAUTION) or leave an AC unrunnable (req-qual-029, LOW). Retired BR-003 and BR-005 are intentional and not flagged. Facts cited to the confidential deck (C3) were not read or verified, so correctness is scored on internal consistency and evidence labelling. BG-003 relevant is scored true as in v0.1.0, although the BRD itself rates it 'weakened'. ISO 25010 presence follows NFR substance, not heading. skill_version '1.0' is the default requirements rubric version, not verified against a SKILL.md spec_version."
```

## BRD_QUALITY_REPORT_V1

```yaml
schema_version: BRD_QUALITY_REPORT_V1
report_id: BRD-2026-Q4-001-quality-20261123T053431Z
generated_at: "2026-11-23T05:34:31Z"
brd:
  id: BRD-2026-Q4-001
  version: 0.2.0
  phase: Define
  artifact_path: .copilot-tracking/brd-sessions/sre-bot-managed-service-brd.md
overall_status: FAIL
decision_thresholds:
  iso_29148_core_min_score: 2
  fr_to_ac_min_pct: 80.0
  fr_to_bg_target_pct: 100.0
gate_decisions:
  define_exit: BLOCKED
  govern_exit: NOT_EVALUATED
summary_counts:
  RISK: 1
  CAUTION: 13
  COVERED: 14
  NOT_APPLICABLE: 1
severity_breakdown:
  CRITICAL: 0
  HIGH: 1
  MEDIUM: 5
  LOW: 8
standards_assessed:
  - skill_name: requirements-quality
    skill_version: "1.0"
    overall_status: RISK
    findings_count: 29
category_summaries:
  iso_29148:
    average_score: 2.22
    weakest_attribute: appropriate
    weakest_attribute_score: 2
  iso_25010:
    covered_categories: 3
    missing_categories:
      - functional_suitability
      - compatibility
      - usability
      - maintainability
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
  fr_bg_coverage:
    fr_total: 10
    fr_with_bg: 10
    bg_total: 3
    coverage_pct: 100.0
    target_pct: 100.0
    waiver_required: false
top_findings:
  - finding_id: req-qual-001
    standard: requirements-quality
    severity: HIGH
    status: RISK
    location:
      section: "Business Goals (BG-001)"
      line_range: L99-L107
    finding: "[UNCHANGED] BG-001 has Unknown baseline, target, timeframe, source and owner, so it cannot pass Measurable or Time-bound. It anchors 6 of 10 FRs."
    recommendation: "Resolve OQ-002 and record baseline, target, timeframe, source and owner, or record an approved waiver or dated deferral."
  - finding_id: req-qual-002
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Business Goals (BG-002)"
      line_range: L111-L119
    finding: "[UNCHANGED] BG-002 Timeframe and Owner are Unknown, so Time-bound fails."
    recommendation: "Add a review date or point and a named owner."
  - finding_id: req-qual-003
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Business Goals (BG-003)"
      line_range: L123-L131
    finding: "[UNCHANGED] The workshop date is Unknown, and demonstration-as-goal is an Assumption."
    recommendation: "Confirm the date, owner and goal intent (OQ-015)."
  - finding_id: req-qual-004
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Stakeholders; Business Goals; Sign-Off"
      line_range: L62-L76, L433-L451
    finding: "[UNCHANGED] Goal owners, sponsor and buyer are Unknown and every sign-off role is Unassigned."
    recommendation: "Name a sponsor and per-goal owners (OQ-001) before Govern."
  - finding_id: req-qual-006
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Design Decisions (DD-008); Non-Functional Requirements (Security)"
      line_range: L91, L211-L212, L280-L281
    finding: "[UNCHANGED, narrowed from HIGH] Guardrail NFR-005 and NFR-006 exist but remain Proposed pending a human decision on DD-008 and OQ-010."
    recommendation: "Get the human decision and accept the guardrail NFRs. Limits may stay Unknown."
  - finding_id: req-qual-013
    standard: requirements-quality
    severity: MEDIUM
    status: CAUTION
    location:
      section: "Constraints"
      line_range: L229-L230, L233
    finding: "[UNCHANGED, partly addressed] The Non-negotiable column exists. CON-002, CON-003 and CON-006 are Unknown and CON-002 and CON-003 are design choices with no external imposer."
    recommendation: "Resolve OQ-007, then keep them as constraints or reclassify them as design decisions."
  - finding_id: req-qual-010
    standard: requirements-quality
    severity: LOW
    status: CAUTION
    location:
      section: "Business Rules"
      line_range: L154, L157
    finding: "[UNCHANGED, narrowed] BR-004's second sentence still restates BR-001."
    recommendation: "Remove the overlap or merge BR-001 and BR-004."
  - finding_id: req-qual-012
    standard: requirements-quality
    severity: LOW
    status: CAUTION
    location:
      section: "Constraints"
      line_range: L231
    finding: "[UNCHANGED, narrowed] CON-004 governs repository evidence handling, not the service, and its imposer is not a named party."
    recommendation: "Name the imposer and keep it as an engagement constraint, or move it to engagement notes."
  - finding_id: req-qual-024
    standard: requirements-quality
    severity: LOW
    status: CAUTION
    location:
      section: "Non-Functional Requirements (Compatibility)"
      line_range: L197
    finding: "[NEW] Compatibility cites OQ-015 for exchange formats, but OQ-015 concerns the workshop date and BG-003 intent."
    recommendation: "Add an open question for exchange formats or cite the correct OQ."
  - finding_id: req-qual-025
    standard: requirements-quality
    severity: LOW
    status: CAUTION
    location:
      section: "Non-Functional Requirements (Security); Acceptance Criteria"
      line_range: L212, L281
    finding: "[NEW] NFR-006 and AC-017 bundle identity, least access, emergency stop and action limits."
    recommendation: "Split into one requirement and one AC per need."
recommendations:
  - id: REC-1
    priority: P1
    target_section: "Business Goals; Stakeholders; Sign-Off"
    action: "Resolve OQ-002, OQ-001 and OQ-015. Give BG-001 a baseline, target, timeframe, source and owner, or record an approved waiver or dated deferral under Waivers. Add timeframes and owners to BG-002 and BG-003, name a sponsor, and confirm that demonstration is the BG-003 goal."
    related_finding_ids:
      - req-qual-001
      - req-qual-002
      - req-qual-003
      - req-qual-004
  - id: REC-2
    priority: P2
    target_section: "Non-Functional Requirements (Security); Design Decisions"
    action: "Get a human decision on DD-008 and OQ-010 and accept NFR-005 and NFR-006. Split NFR-006 and AC-017 into single-need items."
    related_finding_ids:
      - req-qual-006
      - req-qual-025
  - id: REC-3
    priority: P2
    target_section: "Constraints; Design Decisions"
    action: "Resolve OQ-007 and reclassify CON-002, CON-003 and CON-006 as constraints or design decisions. Name the imposer for CON-004. Fold CON-007 into DD-007 or reclassify it as Scope after OQ-014."
    related_finding_ids:
      - req-qual-013
      - req-qual-012
      - req-qual-028
  - id: REC-4
    priority: P3
    target_section: "Business Rules; Functional Requirements; Acceptance Criteria; Non-Functional Requirements"
    action: "Remove the BR-004 and BR-001 overlap. Pick one observable for FR-001 and AC-001. Add or drop the recording of the engineer's recovery execution. Mark AC-014, AC-015 and AC-017 as blocked by OQ-009 and OQ-010. Fix the OQ-015 reference in Compatibility."
    related_finding_ids:
      - req-qual-010
      - req-qual-026
      - req-qual-027
      - req-qual-029
      - req-qual-024
warnings:
  - id: WARN-001
    severity: INFO
    message: "Facts cited to the confidential deck (C3) were not read or verified. Correctness is scored on internal consistency and evidence labelling only."
  - id: WARN-002
    severity: ADVISORY
    message: "0 of 3 goals pass SMART and no waiver is recorded. All goals must pass SMART by Govern exit, or an approved waiver must cover the gap."
    related_finding_ids:
      - req-qual-001
      - req-qual-002
      - req-qual-003
  - id: WARN-003
    severity: INFO
    message: "Retired BR-003 and BR-005 are intentional placeholders to avoid renumbering and were not flagged."
notes: "FAIL is driven by one RISK (BG-001), whose Unknown measure, baseline, target and timeframe block SMART. Everything else is CAUTION. FR-to-AC and FR-to-BG coverage are both 100%, no core ISO 29148 attribute is below 2, and no identifiers are malformed. The BRD should move to NEEDS_REVIEW (APPROVED_WITH_COMMENTS) once BG-001 is resolved or formally waived. Govern exit is not evaluated in the Define phase."
```

## Resolved versus remaining against v0.1.0

| | v0.1.0 | v0.2.0 |
|---|---|---|
| RISK | 1 | 1 (UNCHANGED) |
| CAUTION | 16 | 13 (7 carried over, 6 new) |
| Overall verdict | FAIL, Define BLOCKED | FAIL, Define BLOCKED |

**Resolved (9 of the 17 prior RISK or CAUTION findings):**
- `req-qual-005`: Performance (NFR-004) and Reliability (NFR-003) NFRs were added.
- `req-qual-007`: FR-002 and AC-009 define recurrence.
- `req-qual-008`: AC-003 now requires citing the event content.
- `req-qual-009`: FR-008, FR-009 and FR-010 were added.
- `req-qual-011`: BR-003 was retired in favour of CON-001.
- `req-qual-014`: AC-001 was split from AC-008.
- `req-qual-015`: FR-007 was reworded and command names moved to the Glossary.
- `req-qual-016`: AC-012 was added for NFR-001.
- `req-qual-017`: NFR-002 moved to Security with AC-013.

**Remaining, UNCHANGED (8):**
- `req-qual-001`: BG-001 RISK, Unknown measure blocks SMART. This is the gate blocker.
- `req-qual-002`, `req-qual-003`, `req-qual-004`: BG-002 and BG-003 have no timeframe, goal owners are missing, and sponsor and sign-off roles are unassigned.
- `req-qual-006`: guardrails now exist as NFR-005 and NFR-006 but are still Proposed. Severity fell from HIGH to MEDIUM.
- `req-qual-010`: BR-004 still overlaps BR-001. Narrowed.
- `req-qual-012`: CON-004 is still an engagement control without a named imposer. Narrowed to LOW.
- `req-qual-013`: the Non-negotiable column was added, but CON-002, CON-003 and CON-006 stay Unknown pending OQ-007.

**NEW (6, all LOW):**
- `req-qual-024`: the Compatibility NFR cites the wrong OQ (OQ-015).
- `req-qual-025`: NFR-006 and AC-017 bundle four needs.
- `req-qual-026`: the acceptance observable in FR-001 and AC-001 is ambiguous.
- `req-qual-027`: nothing requires recording the engineer's recovery execution, though the process model shows it.
- `req-qual-028`: CON-007 duplicates DD-007.
- `req-qual-029`: AC-014, AC-015 and AC-017 depend on Unknown values.

**Metrics:** SMART is still 0 of 3 goals passing. FR-to-AC coverage is 10 of 10 and FR-to-BG coverage is 10 of 10. ISO 25010 categories present rose from 1 to 3. The ISO 29148 average rose from 2.00 to 2.22, and the weakest attribute is now `appropriate` at 2 (it was `complete` at 1).