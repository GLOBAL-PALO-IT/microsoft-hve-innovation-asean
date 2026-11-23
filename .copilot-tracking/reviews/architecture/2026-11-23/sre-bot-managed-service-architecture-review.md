<!-- markdownlint-disable-file -->
# Architecture Review Record: SRE Bot Managed Service MVP

> **DRAFT for human review.** This record preserves analysis and proposals. It is not the architecture decision authority. Decisions belong to the named owners and to future ADRs.
>
> - [ ] Reviewed and validated by a qualified human reviewer

## Review Identity and Confirmed Scope

* Review slug: `sre-bot-managed-service`
* Review date: `2026-11-23`
* System or decision under review: the SRE Bot managed service MVP, phases P01 to P03 of the [implementation plan](../../../plans/2026-11-23/sre-bot-managed-service-mvp-plan.md)
* **Architecture design plan (detail for every phase): [sre-bot-managed-service-architecture-design-plan.md](../../../details/2026-11-23/sre-bot-managed-service-architecture-design-plan.md)**
* **Architecture diagram (components, flows, trust boundaries): [sre-bot-managed-service-architecture-diagram.md](../../../details/2026-11-23/sre-bot-managed-service-architecture-diagram.md)**
* **Architecture image (Azure-style and Copilot-style icons): [sre-bot-managed-service-architecture-diagram.png](../../../details/sre-bot-managed-service-architecture-diagram.png). Human review pending.**
* Confirmed focus areas (confirmed by the user's request, 2026-11-23):
  1. Solution approach, trade-offs, cloud architecture and Well-Architected concerns for each plan phase.
  2. Microsoft Marketplace and Microsoft 365 Copilot Agent Store publication: packaging, discoverability, supportability, integration.
  3. Microsoft IQ implications.
* Review participants and decision owners: requested by @prabakarkesavpalo. Product owner, engineering lead, security reviewer and publishing owner are all `Unassigned`.
* Artifacts used: one session, one implementation plan, one experience draft. None was ambiguous, so no identification question was needed.
* Approved output roots from the session context: architecture and publication under `.copilot-tracking/details/`, ADRs under `docs/planning/adrs/`. This record sits in `.copilot-tracking/reviews/architecture/`, the architecture-review convention. The design plan is in the approved `details/` root.

## Evidence Sources and Research Artifacts

Web sources were retrieved on 2026-11-23. "Snippet" means only a search excerpt was read.

| Evidence ID | Source or artifact | Evidence state | Review use |
|---|---|---|---|
| E1 | `.copilot-tracking/plans/2026-11-23/sre-bot-managed-service-mvp-plan.md` | Verified (read in full). Critique status `pending`. | Phases, tasks, boundaries |
| E2 | `.copilot-tracking/prd-sessions/sre-bot-managed-service-prd.md` v0.2.0 | Unreviewed draft. Validate gate BLOCKED (see E7). | Requirements, constraints, invariants |
| E3 | `.copilot-tracking/brd-sessions/sre-bot-managed-service-brd.md` v0.2.0 | Unreviewed draft. Define gate BLOCKED. | Business rules, open questions |
| E4 | `.copilot-tracking/ux-artifacts/sre-bot-managed-service/on-call-incident-response/map-journey.md` | Partial. Reported and assumed experience, not validated research. | Journey, validation, Copilot ideas |
| E5 | `.copilot-tracking/research/2026-11-23/sre-bot-managed-service-session-context.md` and `sre-bot-managed-service-context-summary.md` | Verified | Output roots, evidence boundary, decisions D4 to D7 |
| E6 | `.copilot-tracking/research/2026-11-23/sre-bot-context-discovery-research.md` | Verified. Deck-derived claims are unverified (the deck sits outside the workspace). | Business facts, failure cases |
| E7 | `.copilot-tracking/prd-sessions/sre-bot-managed-service-prd.quality-review-v0.2.0.md` and the BRD equivalent | Automated reviews, FAIL, gates BLOCKED | Readiness context |
| E8 | `.copilot-tracking/github-issues/prds/sre-bot-managed-service-prd/` | Not read. Backlog planning only. | Not used |
| E9 | Learn, [Publish agents to Microsoft Copilot and Microsoft Teams (Foundry)](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/publish-copilot), page dated 2026-11-12. Plus a snippet of the REST route page. | Verified (partly truncated) | Teams path, scopes, data processing |
| E10 | Learn, [Guidelines to Validate Agents - Teams](https://learn.microsoft.com/en-us/microsoftteams/platform/concepts/deploy-and-publish/appsource/prepare/review-copilot-validation-guidelines), updated 2026-10-23 | Verified up to about 26,000 characters. Later sections not read (L2). | Store validation requirements |
| E11 | Learn, [Choose how to publish and distribute your plugin](https://learn.microsoft.com/en-us/plugins/publish), 2026-11-16 | Verified | Distribution routes, registries |
| E12 | Learn, [Publish your plugin to Microsoft Marketplace](https://learn.microsoft.com/en-us/plugins/publish-plugin-publicly), 2026-11-16 | Verified up to the certification-findings section (L3) | Public route prerequisites |
| E13 | Learn, [Publishing guide by offer type](https://learn.microsoft.com/en-us/partner-center/marketplace-offers/publisher-guide-by-offer-type), updated 2026-08-18 | Verified | Offer types |
| E14 | Learn, [How to build and publish AI apps and agents for Microsoft Marketplace](https://learn.microsoft.com/en-us/partner-center/marketplace-offers/artificial-intelligence-apps-agents-publish), updated 2026-05-27 | Verified (short page) | Azure agents versus Microsoft 365 agents |
| E15 | Learn, [Microsoft IQ documentation](https://learn.microsoft.com/en-us/microsoft-iq/), updated 2026-10-27, plus a snippet of the Microsoft IQ product page | Verified (hub page only) | Microsoft IQ assessment |
| E16 | Learn, [Create and manage action groups in Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/alerts/action-groups), 2026-07-21 | Snippet only | Alert delivery |
| E17 | Docs, [Rate limits for the REST API](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api) | Verified (first 12,000 characters) | Write and search limits |
| E18 | Learn, [Overview of Azure SRE Agent](https://learn.microsoft.com/en-us/azure/sre-agent/overview) and its pricing page | Verified overview. Pricing is a snippet. | Overlap, cost reference |
| E19 | Learn, [Design principles for AI workloads on Azure](https://learn.microsoft.com/en-us/azure/well-architected/ai/design-principles) | Snippet only. Page dated 2025-01-09. | Well-Architected framing |
| E20 | `.github/skills/copilot-agent-store-publish/SKILL.md` and `.github/skills/ms-marketplace-publish/SKILL.md` | Repository guidance, read | Publication method and boundaries |
| E21 | `.github/skills/security/owasp-llm/references/06-excessive-agency.md` and the OWASP agentic skill | Repository references, read | Injection and agency risks |

## Context, Constraints, and Assumptions

| ID | Type | Statement | Evidence IDs | Status or owner |
|---|---|---|---|---|
| CTX-1 | Verified context | The MVP takes Teams requests and Azure Monitor alerts, analyses only supplied content, proposes recovery without executing it, and records in GitHub Issues. | E1, E2, E5 | Verified |
| CTX-2 | Verified context | Publication is out of scope for the plan. Readiness is provisional (plan decision D3). | E1 | Verified |
| CTX-3 | Verified context | D4: GitHub Issues replaces Jira. D6: payload-only diagnosis. D7: MVP shown locally and positioning deferred. | E5, E6 | Verified (user decisions) |
| CTX-4 | Constraint | GitHub limits content creation to about 80 requests a minute and 500 an hour, search endpoints are stricter, and secondary limits can change without notice. | E17 | Verified |
| CTX-5 | Constraint | Real incident, customer and tenant data stay out of the repository. | E5 | Verified |
| CTX-6 | Constraint | Offer type cannot be changed after selection in Partner Center. | E13 | Verified |
| CTX-7 | Assumption | "Shown locally" is undefined (OQ-014, OQ-016). | E2, E3 | Open, SME |
| CTX-8 | Assumption | A local Teams path needs a reachable HTTPS endpoint. | E9 (indirect) | Unverified, engineering |
| CTX-9 | Assumption | The deck's architecture is the intended target shape. It names design choices, not confirmed external impositions (OQ-007). | E2 | Unverified |
| CTX-10 | Open | Agent shape and Teams path. | E9, E10, E20 | Open, product owner |
| CTX-11 | Open | Tenancy model and offer type. Revenue model is `Unknown` (OQ-013). | E3, E13 | Open, product owner |
| CTX-12 | Open | Applicability to this agent of the Store response-time, availability and consequential-action rules. | E10 | Open, publishing owner |
| CTX-13 | Unknown | Publisher enrollment, verification and roles in Partner Center. | E12 | Open, Partner Center admin |

## Pillar Findings and Non-Findings

Detail for each phase is in the [design plan](../../../details/2026-11-23/sre-bot-managed-service-architecture-design-plan.md). Priorities are proposals for human review.

### Focus area 1: phase architecture against Well-Architected concerns

| Focus area or pillar | Disposition | Evidence IDs | Finding, non-finding, or unassessed limit | Priority |
|---|---|---|---|---|
| Security (all phases) | Finding AF-1 | E2, E21 | Non-execution is safest as a structural control (no write permission on customer resources, a tool allowlist), not as prompt wording. Content from alerts, messages and comments must be treated as data, with output validated before any write. | High |
| Security (P02-T03) | Finding AF-2 | E2, E3 | Approver authority and identity mapping are `Unknown`. A pending proposal must never read as approved. | High |
| Teams path (P01, P02, P03) | Finding AF-3 | E9, E10 | The deck names Azure Bot Service. Foundry direct publishing registers the Teams bot and compiles the manifest without a separate Bot Service resource. The two paths differ in where the guards live and which publishing route is available. | High |
| Reliability (P02-T01, P02-T03) | Finding AF-4 | E17 | GitHub write and search limits make bursts a design input. A queue, idempotent consumer, backoff and defined degraded behavior are proposed. Chosen behavior is `Unknown`. | Medium |
| Performance efficiency (P02-T01) | Finding AF-5 | E17 | Recurrence lookup relies on `search_issues`, which has stricter limits. A small incident-key map may be needed. The five-operation boundary does not forbid a separate store. | Medium |
| Cost optimization (P02-T02) | Finding AF-6 | E18 | Model use scales with payload size. Per-incident caps and per-incident token reporting are proposed. No amounts are known. | Medium |
| Operational excellence (P02-T03, P03-T01) | Finding AF-7 | E2, E4 | A correlation ID from front door to issue comment, structured audit entries, a regression set of synthetic cases and a documented emergency stop. | Medium |
| Privacy (P02-T03) | Finding AF-8 | E2, E9 | Incident content lands on GitHub, and published agents cause Microsoft 365 and Teams to process data. A redaction step and a data-flow review are needed. The rule is `Unknown`. | Medium |
| Responsible AI (P02-T02) | Finding AF-9 | E4, E10 | Show cited evidence and missing context, and label AI output. Store validation expects AI label, citations and feedback for custom engine agents. | Medium |
| Local demonstration (P02) | Finding AF-10 | CTX-7 | A local demo may collapse the queue and use a direct model call, but only if the contract (event shape, tool allowlist, audit format) stays identical. | Low |
| Sustainability | Unassessed | None | Not in the confirmed scope and no evidence was gathered. | Not applicable |

### Focus area 2: Marketplace and Agent Store publication

| Focus area or pillar | Disposition | Evidence IDs | Finding, non-finding, or unassessed limit | Priority |
|---|---|---|---|---|
| Distribution versus commerce | Finding PF-1 | E13, E14 | Distribution to Copilot and Teams users and commercial sale of the service are separate decisions. The Partner Center offer type named "Managed service" is for Azure Lighthouse delegation of customer Azure resources, which the MVP does not do. This is an inference. | High |
| Differentiated value | Finding PF-2 | E10, E18 | Agent Store validation requires value beyond what Copilot offers (*Must fix*). Positioning is deferred (D7) and Azure SRE Agent overlaps, so the public route is not ready. | High |
| Local demo and hosting | Finding PF-3 | E10, E12 | Publication needs a hosted service. A 99.9 percent availability band appears in the validation guidelines. A local demonstration cannot meet it. Applicability is unconfirmed. | High |
| Agent surface versus backend | Finding PF-4 | E2, E10 | The alert-triggered path is a backend service. A Store package covers only the Teams conversation. | Medium |
| Consequential actions | Finding PF-5 | E10 | Writing to GitHub may need a user confirmation step. Applicability to a custom engine agent is unconfirmed, and alert-triggered writes have no user present. | Medium |
| Publisher prerequisites | Finding PF-6 | E12 | An enrolled, verified publisher account and named owners for certification, support, updates and retirement are required. Status is `Unknown`. | Medium |
| Customer onboarding | Finding PF-7 | E11, E16, E17 | Each customer admin installs the package and sets availability. Each customer also needs an alert-to-service connection and a GitHub credential. Tenancy model decides how much is per customer. | Medium |
| Public route eligibility | Finding PF-8 | E11, E12 | The documented public route covers plugin packages. Whether a Foundry-published or custom engine agent can follow it was not established. | Medium |
| Agent 365 | Non-finding | E10 | Observability trace rules apply only if the agent is extended for Agent 365. It is not planned. | Not applicable |
| Validation guideline sections after about 26,000 characters, and the certification-findings section of E12 | Unassessed limit | E10, E12 | Not read. Graceful error handling and security rules are unassessed. | Medium |

### Focus area 3: Microsoft IQ

| Focus area or pillar | Disposition | Evidence IDs | Finding, non-finding, or unassessed limit | Priority |
|---|---|---|---|---|
| Overall relevance | Non-finding for the MVP, finding for later phases | E15, E5 | Nothing in the session requires Microsoft IQ. The MVP boundary (payload-only, D6) excludes the retrieval these capabilities add. | Low |
| Foundry IQ | Finding IQ-1 | E15 | Best fit for grounding analysis in runbooks and policies. Revisiting D6 would be required. | Low for now |
| Work IQ | Finding IQ-2 | E15, E4 | Possible for team context and the summary ideas in the journey draft. Needs permission-aware access and residency review. | Low for now |
| Fabric IQ and Web IQ | Non-finding | E15 | No evidence of relevance. Web IQ would add external untrusted content. | Not applicable |
| Availability, licensing, pricing, admin controls | Unassessed limit | E15 | Only a hub overview was read. | Low |

## Options, Drivers, Trade-Offs, and Research Recommendation

### Decision Drivers

* Keep the engineer in control and the record auditable (I1 to I4).
* Stay within the workshop MVP while keeping a path to publication.
* Fit GitHub's write and search limits.
* Avoid claims the evidence cannot support, especially on differentiation and commerce.
* Keep the demo simple without changing the contract.

### Candidate Options

For the Teams path and agent shape (ADR-A):

| Option | Benefits | Trade-offs | Evidence IDs | Current disposition |
|---|---|---|---|---|
| A: Foundry-native Teams endpoint | No separate Bot Service. Built-in publishing with Just-you and tenant scopes. Fewer hops. | Teams requests bypass the front door and queue, so guards must live in the tool layer. Two ingestion paths. | E9 | Viable |
| B: Bot Service plus one front door (the deck's shape) | One place for normalization, trigger allowlist, audit and queue. Uniform guards for both triggers. | More resources. Manual package and manifest, unless tooling helps. | E2, E10 | Viable, provisional lean |
| C: Declarative agent with an API plugin to the backend | Copilot orchestrates. Fits the plugin package route. | The MVP supplies its own model and orchestration (CON-003). Analysis would move into Copilot. | E20, E11 | Unlikely, unverified |

For commerce (ADR-D): see Section 5.3 of the design plan. No selection is supported, because the revenue model is unknown.

### Research Recommendation

* Recommendation: Adopt the architecture invariants I1 to I7 now. Build toward a single front door with a queue, a deterministic incident key and a validated, structured agent output, and decide the Teams path and agent shape in ADR-A. Make no publication commitment.
* Evidence IDs: E1, E2, E9, E10, E17, E21
* Confidence: Medium for the invariants and pipeline shape. Low to medium for option B over A. No supported selection for agent shape beyond the lean, and none for the commercial offer type.
* Rejected-alternative rationale: Option C is unlikely because the MVP owns its model and orchestration. Inline processing is rejected for the target because GitHub limits make bursts fail, though a local demo may use it. Semantic incident matching is rejected for the MVP because AC-004 requires repeatable identification.
* Unresolved trade-offs: the Teams path (A or B), whether a separate incident-key map is justified, the tenancy model, and whether the Store validation bands apply.

## Reviewer Disposition

No human reviewer has acted. Nothing below is accepted.

| Recommendation or field | Disposition | Rationale | Decision owner |
|---|---|---|---|
| Invariants I1 to I7 | Deferred (pending human review) | Proposed from requirements that are themselves unreviewed drafts. | Product owner, security reviewer |
| Front door plus queue pipeline | Deferred (pending human review) | Depends on ADR-A and on what "shown locally" means. | Engineering lead |
| Deterministic incident key, with an optional map store | Deferred (pending human review) | Match field set is `Unknown` (OQ-006). | SME, engineering lead |
| Teams path and agent shape | Deferred (pending human review) | No supported selection yet. | Product owner |
| Degraded behavior table | Deferred (pending human review) | Chosen behavior is `Unknown` (OQ-009). | SME, engineering lead |
| Publication route and offer type | Deferred (pending human review) | Revenue model, tenancy and differentiation unknown. | Product owner, publishing owner |
| Microsoft IQ | Deferred (pending human review) | Not an MVP dependency. Revisit if D6 changes. | Product owner |

## Recommendations and Priorities

| Priority | Recommendation | Evidence IDs | Owner | Next action |
|---|---|---|---|---|
| High | Decide what "shown locally" means. | CTX-7 | SME | Answer OQ-014 and OQ-016. |
| High | Decide the Teams path and agent shape. | E9, E10 | Product owner, engineering lead | Draft ADR-A. |
| High | Enforce non-execution by permissions and a tool allowlist, and treat content as data. | E21 | Security reviewer | Threat model, then ADR-B. |
| High | Do not claim Store readiness or differentiation. Settle positioning evidence first. | E10, E18 | Product owner | Revisit OQ-008 before any public route. |
| Medium | Define approver authority and identity mapping. | E2 | Security, SME | Answer OQ-010 and OQ-012. |
| Medium | Decide the incident-key map and queue necessity against GitHub limits. | E17 | Engineering lead | Estimate alert volume (OQ-009), then ADR-C. |
| Medium | Decide the incident-data rule and review Microsoft 365 data flows. | E9 | Security, owner | Answer OQ-011. |
| Medium | Confirm which Store validation rules apply to the chosen shape. | E10 | Publishing owner | Check current guidance for the selected shape. |
| Medium | Confirm publisher enrollment and name owners for certification and support. | E12 | Partner Center admin | Verify in Partner Center. |
| Low | Revisit Foundry IQ only if D6 is reopened. | E15 | Product owner | None now. |

## Escalations and Human Decision Owners

| Escalation | Why human judgment is required | Decision owner | Required evidence or action |
|---|---|---|---|
| Commercial route and pricing | Revenue model is unknown and the offer type is irreversible | Product manager, with counsel and tax advisers for contracts and tax | Shared monetization plan via the Marketplace publishing workflow |
| Security architecture | Full threat model, identity and injection controls | Security Planner and security reviewer | Threat model and ADR-B |
| Responsible AI | AI disclosure, feedback, over-trust of generated diagnosis | RAI Planner and reviewer | RAI assessment |
| Data flows and residency | Incident content reaches GitHub, and published agents reach Microsoft 365 and Teams processing | Privacy and compliance reviewers | Data-flow review, OQ-011 |
| Publisher account | Account-specific facts must be checked by an administrator | Partner Center admin | Enrollment, verification and roles |
| Differentiation claim | No evidence supports it. Overlap with Azure SRE Agent exists | Product owner | Positioning evidence (OQ-008) |

## ADR Links

No ADR exists yet. The planned records below are for human decision owners. The ADR and its owner remain authoritative.

| Significant decision | ADR path | Status | Authority note |
|---|---|---|---|
| ADR-A: Teams path and agent shape | `docs/planning/adrs/` (not created) | Planned | The ADR and its human decision owner remain authoritative. |
| ADR-B: Agent identity, permissions and non-execution enforcement | `docs/planning/adrs/` (not created) | Planned | The ADR and its human decision owner remain authoritative. |
| ADR-C: Ingestion buffering, incident key and mapping store | `docs/planning/adrs/` (not created) | Planned | The ADR and its human decision owner remain authoritative. |
| ADR-D: Publication route, tenancy and offer type | `docs/planning/adrs/` (not created) | Planned | The ADR and its human decision owner remain authoritative. |

## Limits and Unassessed Areas

* L1: The source deck is outside the workspace and was not re-read. Deck-derived claims rely on the earlier research and are unverified.
* L2: The Agent Store validation guidelines were read to about 26,000 characters. The remaining sections (parts of action and knowledge sources, duplicate agents, agent response, graceful error handling, security) were not read.
* L3: The public-publication page was read up to its certification-findings section. Whether a Foundry-published or custom engine agent can use the public Marketplace route was not established.
* L4: Azure Monitor action groups and the Well-Architected AI principles were read as search snippets only. Service Bus duplicate detection, dead-letter behavior and any local-development options were not researched, so design statements about them are proposals, not verified facts.
* L5: Microsoft IQ was read at hub level. Availability, licensing, pricing and admin controls are unassessed.
* L6: Marketplace fees, pricing, tax and contract terms are out of scope and unassessed.
* L7: Sustainability was not assessed.
* L8: The requirements are unreviewed drafts with BLOCKED quality gates, and the implementation plan's critique is pending. Everything here inherits that uncertainty.
* L9: Nothing was built, deployed, published or tested. Web guidance changes, so recheck current Microsoft guidance before acting.
