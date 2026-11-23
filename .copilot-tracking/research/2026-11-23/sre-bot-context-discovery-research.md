<!-- markdownlint-disable-file -->
# Task Research: SRE Bot Managed Service Context Discovery

| Field              | Value                                                                  |
|--------------------|------------------------------------------------------------------------|
| Date               | 2026-11-23                                                             |
| Researcher / agent | rpi-research                                                           |
| Output mode        | analysis (context discovery; does not prepare planning or recommend an implementation) |

## Executive Summary

* Bottom line: The approved evidence gives a clear concept and a clear MVP boundary, but it is one internal deck. The deck states a few business rules (human approves the fix, every action is logged to a GitHub Issue). It gives no affected-user detail beyond on-call engineers, no known failure cases, no data on volume or impact, and no AI guardrails beyond human approval.
* Why this matters: Two points matter for scope. The five tools are GitHub Issues operations only, and the user confirmed the MVP diagnoses from the alert or Teams message alone while the engineer runs any recovery. And Microsoft's first-party Azure SRE Agent already covers much of the same ground, which weakens any "unique advantage" claim.
* Research status: Complete for the approved evidence and a bounded external check on alternatives. Gaps remain that only the SME can fill.
* Confidence and uncertainty: High on what the deck states and on the cited public documentation. Medium to high on the MVP diagnosis and recovery boundary, which the user confirmed. Low on failure modes, which are unverified hypotheses.

## What You May Not Know

* The MVP's five tools are `create_issue`, `update_issue`, `add_comment` or `list_comments`, `get_issue` and `search_issues`. None reads Azure logs or changes an Azure resource, yet the deck says the bot "root-causes from the logs" and "applies or proposes" a fix (C3 slides 3, 5, 6). The user confirmed that the MVP diagnoses from the alert or Teams message alone and the engineer runs any recovery (D6).
* Azure SRE Agent already offers Azure Monitor alert intake, Teams and GitHub integration, a Review mode where an administrator approves write actions, per-tool allow, ask or deny controls, and an audit trail (W1). It also bills per use with a fixed always-on charge (W2).
* GitHub limits content creation to about 80 requests a minute and 500 an hour (W3). An alert storm could hit that limit if every alert becomes an issue or comment.
* The deck's only worked example is a disk-full alert that "fired again" on a VM, so recurring alerts, not just first occurrences, are the case it demonstrates (C3 slide 4).

## Findings

### Business facts: the deck states a clear concept but no impact data

On-call engineers currently run detect, diagnose and recover by hand. The proposal is a bot that runs each step through `/detect`, `/analysis` and `/recover` while an engineer approves the final fix. The MVP is one Azure path from Teams requests and Azure Monitor alerts to GitHub Issues, scoped "for the HVE workshop". The deck has no incident volume, time, cost or customer figures, so the benefit cannot be sized.

* Questions: Q8
* Evidence state: Evidence-backed finding (concept); Unresolved possibility (impact)
* Evidence: C3 slides 2, 3, 5, 6; C8
* Confidence and limits: High for the stated concept. Single, unverified, confidential source.

### Affected users: only the on-call engineer is named

The on-call engineer asks the bot for actions in Teams and approves the fix. The rest of the team gets awareness through the shared GitHub Issue ("the whole team stays aware"). People the deck does not name but the design implies: whoever administers the Azure and GitHub access the bot uses, security or compliance reviewers of that access, and the managed-service operator running the bot. The customer's buyer and stakeholders are not stated.

* Questions: Q9
* Evidence state: Evidence-backed finding (engineer, team); Partially supported claim (implied administrator and operator roles)
* Evidence: C3 slides 3, 4, 5, 6
* Confidence and limits: Medium. The implied roles are inference.

### Business rules the deck states

* An engineer approves the final fix. Recovery without human approval is a later phase (C3 slides 2, 3, 6).
* Every Teams request and every bot action is written to the matching GitHub Issue, which is the single source of truth (C3 slides 2, 4).
* The MVP accepts two triggers only (C3 slides 5, 6), uses Azure OpenAI in Foundry only, and rejects non-Azure or local models until later (C3 slide 6).
* The bot works through a fixed set of five GitHub tools (C3 slides 5, 6).
* The incident is fingerprinted automatically, and the example shows an existing issue being reused (`Issue #52 (existing)`), which suggests deduplication against existing issues. This is inferred from `search_issues` and the example and is not stated as a rule (C3 slides 3, 4, 5).

Not stated: severity or priority rules, escalation, who may approve, approval time limits, what counts as a recoverable action, and data retention.

* Questions: Q10
* Evidence state: Evidence-backed finding (stated rules); Partially supported claim (deduplication)
* Evidence: C3 slides 2 to 6
* Confidence and limits: High for the stated rules.

### Constraints: scope, platform and confidentiality are clear; capacity is not

* The MVP is Azure and Microsoft Teams only, with named Azure services (C3 slide 6).
* The source deck is CONFIDENTIAL and nothing real (incidents, tenants, credentials) may enter the repository (C1).
* Service Bus sits between ingestion and the agent, which would help buffer bursts (inference from C3 slide 5).
* GitHub's REST limits apply to the system of record: 5,000 requests an hour for a user or app token, and about 80 content-creating requests a minute and 500 an hour, with secondary limits that can change without notice (W3). The deck does not mention these, nor how many alerts a customer would produce.
* Not stated: latency or availability targets, data residency, the GitHub plan and repository visibility, and where incident data in issues may be stored.

* Questions: Q11
* Evidence state: Evidence-backed finding (scope, GitHub limits); Unresolved possibility (capacity fit)
* Evidence: C1, C3 slides 5, 6, W3
* Confidence and limits: High for the limits as documented. The effect depends on unknown alert volumes.

### The MVP diagnosis and recovery mechanism: payload only, engineer executes

The tool list is five GitHub Issues operations. The architecture feeds the bot an event "payload + context" and returns GitHub results, with no step that queries Azure logs or runs an Azure action. So "root-causes from the logs" relies only on what arrives in the alert or Teams message, and "applies the fix" has no visible tool. This began as an inference from absence, and the user confirmed it in D6.

* Questions: Q12
* Evidence state: Evidence-backed finding (inference confirmed by the user in D6, option A)
* Evidence: C3 slides 3, 4, 5, 6; user answer 2026-11-23
* Confidence and limits: Medium to high. The user confirmed the MVP boundary but did not supply a source. Root-cause quality now depends on what the alert and Teams message carry, which the deck does not specify. `/recover` in the MVP proposes a fix and the engineer carries it out.

### Known failure cases: none documented

The deck records the problem as alert fatigue and manual log search, and shows one recurring-alert example. It documents no past incident, wrong diagnosis, failed recovery, duplicate issue or outage. Plausible failure modes exist but are unverified hypotheses: a wrong root-cause analysis presented with confidence, a bad proposed fix approved by a tired engineer, duplicate or flooding issues, notification loss when Teams or GitHub is down, a dead-lettered Service Bus message, and sensitive log content landing in a GitHub Issue.

* Questions: Q13
* Evidence state: Unverified hypothesis/conjecture
* Evidence: C3 slide 4 (example only); no source for the failure modes
* Confidence and limits: Low. These need real incident history or SME input, subject to the evidence boundary.

### AI guardrails: human approval and an audit trail, little else

Stated guardrails are the human approval gate, the GitHub Issue audit trail, a fixed tool set and Key Vault for secrets (C3 slides 2, 4, 6). The repository's OWASP references show where the deck is silent. Excessive agency applies directly to a bot that can propose or apply recovery (C6). Alert text, log lines and issue comments feed the model and can carry indirect prompt injection, and the agentic list adds tool misuse and rogue agents (C7). The deck does not define the bot's identity and permissions, approval authority, an emergency stop, action limits, or how model output is validated before a tool call.

* Questions: Q14
* Evidence state: Evidence-backed finding (stated guardrails); Partially supported claim (gaps, from absence in the deck and OWASP risk descriptions)
* Evidence: C3 slides 2, 4, 6, C6, C7
* Confidence and limits: Medium. The deck is a summary, so some controls may exist elsewhere.

### Counter-evidence: Azure SRE Agent overlaps the concept

Microsoft documents Azure SRE Agent as an alert-driven agent that queries Azure Monitor, Application Insights and Log Analytics, proposes root causes and mitigations, and either waits for approval (Review mode) or acts (Autonomous mode). It integrates Azure Monitor alerts, Teams, GitHub repositories and issues, PagerDuty and ServiceNow, offers tool-level allow, ask or deny, and keeps an audit trail (W1). Billing combines an always-on charge with variable usage, with no general free tier (W2). The deck does not mention it. The product's GA status and regional availability were not confirmed from the pages retrieved.

What may still differ is a PALO IT managed-service wrapper, GitHub Issues as the system of record, a Teams-first workflow, and cost or control preferences. None of that is evidenced.

* Questions: Q15
* Evidence state: Evidence-backed finding (overlap exists); Unresolved possibility (whether the offer is differentiated)
* Evidence: W1, W2
* Confidence and limits: High that the capability overlap is documented. Low on commercial fit.

## Recommendation and Alternatives

* Recommendation or decision state: Analysis mode, so no selection. Decision state: the MVP's diagnosis and recovery boundary is settled (D6, option A), GitHub Issues replaces Jira (D4), the confidentiality marking is not a sharing constraint (D5), and the MVP is shown locally with positioning against Azure SRE Agent (D7) deferred beyond the MVP.
* Rationale: The overlap (W1) changes what the product is and cannot be resolved from the approved evidence. The tool-list gap is closed by the user's answer.
* What could change this result: SME confirmation of how the bot gets logs and applies fixes, customer demand evidence, or a comparison with Azure SRE Agent.

| Option | Benefits | Costs and risks | Evidence | Disposition |
|---|---|---|---|---|
| Build the SRE Bot as in the deck | Control over the workflow, GitHub audit trail, Teams chat flow | Overlaps a first-party product, guardrails to design, GitHub rate limits | C3, W1, W3 | Viable, not evaluated further |
| Adopt Azure SRE Agent, offered as a managed service | Mature governance, run modes, integrations | Usage billing, less customization, differentiation is the service wrapper only | W1, W2 | Viable, unverified |
| Combine: SRE Agent for investigation, GitHub Issues for the audit trail | Reuses governed tooling | Integration effort unknown | W1 | Viable, unverified |
| Non-AI automation (runbooks, alert action groups, existing PagerDuty or ServiceNow) | Lower AI risk | Does not address manual diagnosis; not researched here | W1 (partial) | Deferred, outside scope |

## Scope and Questions

* Goal: Discover business facts, affected users, business rules, constraints, known failure cases, AI guardrails, open questions, and credible alternatives or counter-evidence.
* Audience and use: SME and PM roles preparing the workshop tracks.
* In scope: session context, the approved source deck, existing session research, plus public documentation for alternatives and counter-evidence.
* Out of scope: requirements, architecture, plans, implementation, pricing analysis, and a full competitor survey.
* Decision and evidence criteria: separate stated facts, inference and hypothesis; no unsupported claims; no copying the confidential deck.
* Requested output: this dated primary artifact.
* Numbering continues from: `.copilot-tracking/research/2026-11-23/sre-bot-lean-canvas-research.md` (C1 to C5, Q1 to Q7, D1 to D5).

| ID | Question | Source | Status |
|----|----------|--------|--------|
| Q8 | What business facts are evidenced? | explicit | answered |
| Q9 | Who is affected? | explicit | answered (partly) |
| Q10 | What business rules apply? | explicit | answered |
| Q11 | What constraints apply? | explicit | answered |
| Q12 | How do diagnosis and recovery work in the MVP? | inferred | answered (D6, option A) |
| Q13 | What failure cases are known? | explicit | answered (none documented) |
| Q14 | What AI guardrails exist and are missing? | explicit | answered |
| Q15 | What alternatives or counter-evidence exist? | explicit | answered |

## Decisions and Feedback

| Group | Decision or feedback item | Status | Owner | Rationale or input needed | Evidence | Impact of answer |
|-------|---------------------------|--------|-------|---------------------------|----------|------------------|
| D4 | Is Jira in scope? (the `/recover` part is settled by D6) | confirmed: replaced by GitHub Issues (user, 2026-11-23) | user (SME) | Speaker notes mention Jira; slides do not. The user decided GitHub Issues replaces Jira. | C3, user answer | Offering scope settled |
| D5 | Sharing permission for the confidential deck and summaries | confirmed: user chose to ignore the confidentiality marking as a sharing constraint (2026-11-23) | user (owner) | Open item in the session context, now decided by the owner. | C1, user answer | Summaries may be used and shared; real incident, customer and tenant data still stay out of the repository |
| D6 | How does the MVP read logs and apply recovery? | confirmed (option A) | user (SME) | User answered: diagnosis uses only what arrives in the alert or Teams message, and the engineer runs any recovery. | C3, user answer 2026-11-23 | Settles Q12. `/recover` in the MVP proposes only, and diagnosis depth depends on payload quality. |
| D7 | Positioning against Azure SRE Agent | confirmed: MVP shown locally; positioning deferred beyond the MVP (user, 2026-11-23) | user (PM) | The user decided positioning is not needed for the MVP. What "locally" means for hosting is a new open question (BRD OQ-014, PRD OQ-016). | W1, W2, user answer | Advantage and revenue areas of the canvas wait for a later release |

### D6: How does the MVP diagnose and recover?

How does the bot get diagnostic data, and what can it do on recovery in the MVP?

* A: Diagnosis uses only what arrives in the alert or Teams message, and the engineer runs any recovery.
* B: The bot also reads Azure logs or applies fixes through tools not shown in the deck.
* C: Not yet decided.

## Risks and Open Questions

| Priority | Type | Risk, question, or research item | Impact | Smallest action or evidence needed | Owner |
|----------|------|----------------------------------|--------|------------------------------------|-------|
| M | open question | Payload-only diagnosis: what the alert and Teams message carry is unspecified | Root-cause quality | SME defines minimum event content | user |
| H | risk | First-party overlap with Azure SRE Agent | Weak differentiation and revenue | PM comparison and customer evidence | user |
| H | risk | Prompt injection and excessive agency | Wrong or harmful action | Define identity, permissions, approval authority, input handling | downstream |
| M | risk | GitHub content-creation limits during alert bursts | Dropped or delayed updates | Estimate alert volumes, design batching | user |
| M | risk | Sensitive log content stored in issues | Confidentiality breach | Decide repository visibility and redaction | user |
| M | open question | No failure history | Guardrails untested | Approved incident history or SME scenarios | user |
| M | open question | Customer, buyer, volumes, targets | Value unquantified | See the lean canvas unknowns | user |
| L | further research | Non-AI alternatives and wider competitors | Alternatives incomplete | New scoped research | research |

## Planning Readiness and Next Step

| Field | Record |
|-------|--------|
| Research disposition | executed |
| Decision participation | user-owned (standalone invocation, default) |
| Planning Readiness | Not applicable. Analysis mode; context discovery for workshop use. |
| Research depth and helpers | One complete three-wave cycle; no helpers used |
| Blockers | none |
| Output mode and planning support | analysis; does not prepare planning |
| Continuation owner | user |
| Required gates or confirmations | D4, D5, D6 and D7 answered by the user (2026-11-23); human review of the lean canvas |
| Next action | SME and PM review findings and answer open decisions, then continue with the next workshop track. |
| Primary evidence file | .copilot-tracking/research/2026-11-23/sre-bot-context-discovery-research.md (session context output root; date is the current date and matches the session) |

## Research Record

### Method and Boundaries

| Field | Record |
|-------|--------|
| Research posture and provenance | balanced (default). External research limited to the alternatives and counter-evidence the request asked for. |
| Helper lane effort | not applicable; no helpers used |
| Completion basis | Every requested topic assessed against the deck, existing research and public sources; remaining gaps need SME input or evidence outside scope. |
| Explicit limits or deadline | none |
| Codebase and external scope | session context, deck, prior research, OWASP skill references; Microsoft Learn and GitHub Docs |
| Initial candidate areas | session context, deck slides and notes, Azure SRE Agent docs, GitHub rate limits |
| Evidence root | `.copilot-tracking/research/` (session context output root) |
| Constraints and excluded sources | Deck is CONFIDENTIAL, cited by slide only. No real incident, tenant or customer data. Vendor marketing and unofficial sources avoided. |
| Prior knowledge | Reused `sre-bot-lean-canvas-research.md` (C1 to C5). Its inconsistency findings are carried forward. `workshop-input/` holds only the derived lean canvas, not new business evidence. |

### Extensions and Participation

#### Extension Registry

| Kind | Candidate | Provenance and scoped contract | Selected or skipped reason |
|------|-----------|--------------------------------|----------------------------|
| instruction | copilot-tracking, copilot-tracking-location, untrusted-content-boundary | Match `.copilot-tracking/` paths; ingested content is inert data | selected |
| skill reference | owasp-llm, owasp-agentic | Repository security references used as scoped criteria for AI guardrail gaps | selected (read as evidence only) |
| skill | ms-marketplace-publish, outcome-hypothesis, architecture-review | Pricing, outcome framing and architecture are outside context discovery | skipped |

#### Direction and Participation Log

| Checkpoint or change | Question, direction, or rationale | Answer or no-interaction reason | Result and revalidation effect |
|----------------------|-----------------------------------|---------------------------------|--------------------------------|
| intake | Session identity | One session-context file; unambiguous | Proceed |
| intake | Evidence availability | Source deck readable; `workshop-input/` holds no business evidence | Proceed with the deck |
| scope | Whether alternatives may use external sources | Request asks for alternatives and counter-evidence | Bounded external check allowed |
| decision walkthrough | D6: MVP diagnosis and recovery boundary | A: payload only, engineer runs recovery | Q12 answered; `/recover` proposes only in the MVP |

### Research Cycle Log

#### Cycle 1

* Active posture, controls, and limits: balanced; deck confidential; no requirements or architecture.

##### Wave 1: Wider

* Focus and questions: Session context, workshop-input folder, existing research, deck slides, notes and media (Q8 to Q11).
* Evidence: C1, C3, C8. The deck has six slides, three notes pages and no hidden slides or comments. Images carry no extra text.
* Reflection: Deck content answers concept, rules and scope. Failure cases, volumes and guardrails are absent. Priority targets are the tool list, guardrails and alternatives.

##### Wave 2: Deeper

* Focus and questions: Tool and data flow (Q12), guardrail gaps (Q14), Azure SRE Agent overview and pricing, GitHub limits, OWASP references.
* Evidence: C3 slides 5 and 6 list five GitHub tools only. W1 documents overlapping capabilities and run modes. W2 documents the billing structure. W3 documents rate and content limits. C6 and C7 describe excessive agency, prompt injection, tool misuse and rogue agents.
* Reflection: The missing diagnosis and recovery tooling is a material open question. Overlap with SRE Agent is the strongest counter-evidence.

##### Wave 3: Contrarian

* Focus and questions: Challenge the claim that the bot diagnoses from logs and recovers; challenge uniqueness; look for evidence that the failure modes are real.
* Evidence: No evidence in the approved sources supports log retrieval or recovery tooling. W1 shows first-party coverage of the deck's selling points. No failure history exists, so failure modes stay hypotheses.
* Reflection: Downgraded "advantage" further, kept diagnosis and recovery as an inference with a confirmation question, and labelled failure modes unverified.

##### Synthesis and Re-entry

| Material or claim | Evidence | Disposition | Rationale | User-facing effect |
|-------------------|----------|-------------|-----------|--------------------|
| Concept, rules, MVP scope | C3 | accepted (deck-stated) | Consistent across slides | Findings 5, 7 |
| Diagnosis and recovery mechanism | C3, user answer | accepted | Absent from tool list; confirmed by D6 | Finding 9, D6 |
| Failure modes | none | deferred as hypotheses | No evidence | Finding 10 |
| Guardrail gaps | C3, C6, C7 | accepted | Absent in deck, risk classes apply | Finding 11 |
| Azure SRE Agent overlap | W1, W2 | accepted | First-party docs | Finding 12 |
| Pricing comparison, wider competitors | none | deferred | Outside scope | Risk row |

* Another complete three-wave cycle needed: no
* Trigger or stop basis: scope coverage; remaining gaps need SME input or out-of-scope evidence.
* Readiness or revalidation effect: D6 answered with no scope change; re-enter only if D7 changes scope.

### Evidence Log

* Helpers: none

| ID | Claim or finding | Source or location | Retrieved and version | Tool | Confidence | Notes |
|----|------------------|--------------------|-----------------------|------|------------|-------|
| C1 | Session topic, evidence boundary, output roots, open items | .copilot-tracking/research/2026-11-23/sre-bot-managed-service-session-context.md `Evidence Boundary`, `Open Items` | not applicable | read | high | Reused from prior session work |
| C3 | Concept, rules, MVP scope, tools, architecture, speaker notes | `/Users/pkesavan/Desktop/HVE-Managed Service.pptx` slides 1 to 6 and notes (controlled local file outside the workspace) | not applicable | zip and XML text extraction, no copy | medium | Numbering continues from the earlier document; single unverified source |
| C6 | Excessive agency: unintended actions via excess permissions, autonomy or injection | .github/skills/security/owasp-llm/references/06-excessive-agency.md `06 Excessive Agency` | not applicable | read | high | Repository reference, used for risk framing |
| C7 | Agentic risks include tool misuse and rogue agents | .github/skills/security/owasp-agentic/SKILL.md risk list | not applicable | grep, read | high | Repository reference |
| C8 | Prior findings on canvas, inconsistencies, confidentiality | .copilot-tracking/research/2026-11-23/sre-bot-lean-canvas-research.md `Findings` | not applicable | read | high | Existing session research |
| W1 | Azure SRE Agent: alert intake, run modes, integrations, tool access, audit | [Overview of Azure SRE Agent](https://learn.microsoft.com/en-us/azure/sre-agent/overview) | 2026-11-23; page dated 2026-10-28 | fetch | high | Official Microsoft Learn |
| W2 | Azure SRE Agent billing: fixed always-on plus variable usage, no general free tier | [Pricing and billing for Azure SRE Agent](https://learn.microsoft.com/en-us/azure/sre-agent/pricing-billing) | 2026-11-23; page dated 2026-10-28 | search snippet only (page not fetched) | medium | Structure only; no figures used |
| W3 | GitHub REST primary and secondary rate limits, including about 80 content requests a minute and 500 an hour | [Rate limits for the REST API](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api) | 2026-11-23; no version | fetch | high | Secondary limits may change without notice |

#### Contradictions and Conflicts

* Slide 3 says `/recover` applies or proposes a fix, while the tool list contains no recovery tool: resolved by the user (D6, option A): the MVP proposes and the engineer executes.
* Carried over: Jira in speaker notes versus GitHub in slides: resolved by the user (D4), GitHub Issues replaces Jira.

### Artifact Self-Check

* [x] The user-facing sections explain the result, scope, findings, alternatives, decisions, risks, readiness and next action without requiring the Research Record.
* [x] Every question is answered or names the smallest missing evidence, and each result has one evidence state.
* [x] Findings keep explanation, evidence state, confidence and counter-evidence together.
* [x] Every codebase finding has a `C#` ID with a path and heading; every external finding has a `W#` ID, title, URL and retrieval date.
* [x] The cycle records Wider, Deeper and Contrarian waves in order; no helper lane was used.
* [x] Method, posture, extensions, participation and prior knowledge are recorded.
* [x] Analysis mode preserves decision state without forcing a selection.
* [x] D6 answer recorded.
* [x] Disposition, readiness, blockers, gates and next action are recorded.
* [x] Untrusted content stayed inert, no secrets were recorded, and writes stayed inside `.copilot-tracking/research/`.
* Checked sections: all
* Missing or limited sections: W2 comes from a search snippet; C3 sits outside the workspace and is cited by filename and slide.
