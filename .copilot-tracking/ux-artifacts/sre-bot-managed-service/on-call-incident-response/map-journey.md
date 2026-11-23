<!-- markdownlint-disable-file -->
# UX Artifact: On-Call Incident Response

## Artifact Context

* Project: sre-bot-managed-service
* Subject: On-call incident response with the SRE Bot MVP
* Mode: map-journey
* Status: partial
* Sources: .copilot-tracking/research/2026-11-23/sre-bot-managed-service-session-context.md; .copilot-tracking/research/2026-11-23/sre-bot-managed-service-context-summary.md; .copilot-tracking/research/2026-11-23/sre-bot-context-discovery-research.md; .copilot-tracking/research/2026-11-23/sre-bot-lean-canvas-research.md; .copilot-tracking/research/workshop-input/leancanvas.md; Microsoft Learn, "Overview of Microsoft Copilot Chat"; Microsoft Learn, "Intelligent recap for Teams calls, meetings, and events"; user-provided context in this conversation.

## Observed

* None. No direct observation, usability study, or analytics were provided.

## Reported

* The user describes on-call engineers as repeatedly checking for incidents even when nothing concerning is happening. The user says the pain points reflect internal experience; no participant, interview, or observation details were provided.
* The workshop research reports that on-call engineers handle detection, diagnosis, and recovery manually, including manual log search, and that the deck frames this as alert fatigue. This is a deck-stated claim, not independently verified user research (HVE-Managed Service.pptx, slides 2-3; .copilot-tracking/research/2026-11-23/sre-bot-context-discovery-research.md).
* The proposed MVP accepts Azure Monitor alerts and Teams requests, records requests and bot actions in GitHub Issues, analyzes only the alert or Teams message content, and leaves recovery execution to the engineer (HVE-Managed Service.pptx, slides 2-6; decision D6 in .copilot-tracking/research/2026-11-23/sre-bot-context-discovery-research.md).
* The context identifies the primary user as an on-call engineer. The user confirms the role as on-call engineer / SRE; experience level is unknown.

## Assumed

* A bot flow that reduces unnecessary checking or makes incoming incident information easier to assess may address the reported burden. Validate whether this helps engineers and what information they need.
* The proposed stages below describe a workshop MVP concept, not an observed or validated end-to-end user experience.
* Thoughts, emotions, and preferred interaction details are not evidenced. Do not treat the suggested opportunities as settled interface requirements.
* Users may need keyboard access, visible focus, clear labels and announced updates, sufficient contrast, non-color status cues, and usable layouts at 200% zoom. These are design requirements to validate, not evidence about a named user group.

## Unresolved

* How often engineers check, how much time this takes, and the consequences of missed or delayed response.
* Which source details support payload-only analysis, and how should the bot distinguish supplied evidence from generated interpretation or missing context?
* The engineer's actual steps and tools between receiving an alert, investigating it, and carrying out recovery.
* How can an engineer reject, correct, or escalate an incomplete, incorrect, or unavailable analysis? Who may approve or perform recovery, and how can an action be stopped?
* What the team needs from the GitHub Issue record, including notification, update, and handoff expectations.
* What source-authority, access, retention, redaction, and exclusion rules apply to incident content and GitHub Issue records, including sensitive data.
* How should the system handle untrusted instructions embedded in an alert or Teams message?
* Which interaction states across Azure Monitor, Teams, and GitHub Issues must be validated, including status updates, errors, proposal review, and handoff?
* User skill level, typical device and work environment, accessibility needs, and any differences between user roles.
* No interviews, contextual inquiry, usability studies, or operational measurements were supplied. Validate the internal-experience report with on-call engineers before treating it as representative.

## Scenario

* Users: On-call engineer / SRE. The wider team is described as following activity through a shared GitHub Issue, but no other user group is validated.
* Starting context: The engineer is monitoring for incidents, receives an Azure Monitor alert, or starts an incident request in Microsoft Teams.
* Intended outcome: The engineer gets an analysis based on the supplied alert or message, evaluates a recovery proposal, performs any recovery, and has the request and bot activity recorded in a GitHub Issue.

## Experience Outline

1. An Azure Monitor alert arrives, or an engineer starts a request in Teams.
2. The bot uses the alert or message content for its analysis and writes the request and bot activity to the matching GitHub Issue.
3. The engineer reviews the analysis and any proposed recovery. The MVP does not fetch additional Azure logs or execute recovery actions.
4. The engineer decides what to do and carries out any recovery. The GitHub Issue remains the shared activity record.

This sequence summarizes the proposed MVP boundary in the workshop evidence. It does not specify exact screens, response timing, approval mechanics, or recovery procedures.

## Journey Stages

| Stage | User actions | Thoughts or questions | Pain points | Opportunities | Basis and source |
|---|---|---|---|---|---|
| Ongoing vigilance | Repeatedly checks for incidents, including when nothing concerning is happening. | Unknown. | Frequent checking is described as an internal-experience problem; its frequency and impact are not measured. | Explore ways to reduce unnecessary checking while keeping incident status clear. | Reported: user input in this conversation. |
| Alert or request begins | Receives an Azure Monitor alert or starts an incident request in Teams. | Unknown: what signal is actionable, and what context does the engineer expect? | The workshop deck describes manual alert response; details of the current trigger and triage process are not documented. | Make the incoming signal and available context easy to assess. | Reported: HVE-Managed Service.pptx, slides 3, 5-6; context discovery research. |
| Diagnosis | In the proposed MVP, supplies or reviews the alert or Teams message content used for analysis. | Unknown: is the supplied content sufficient to trust or act on the analysis? | Current manual diagnosis and log search are reported in the deck; the MVP's analysis is limited to the supplied content. | Surface what information the analysis used and where context is missing. | Reported: HVE-Managed Service.pptx, slides 3-6; decision D6 in context discovery research. |
| Recovery decision and action | Reviews a proposed recovery, then decides and performs any recovery as the engineer. | Unknown: what evidence is needed to evaluate a proposal, and what recovery actions are allowed? | Recovery is manual in the current experience reported by the deck. No failure or recovery history was supplied. | Keep the engineer in control and make uncertainty and next steps clear. | Reported: HVE-Managed Service.pptx, slides 2-3, 6; decision D6. |
| Shared record and handoff | Engineer and team refer to the GitHub Issue containing the request and bot activity. | Unknown: what information supports team awareness and handoff? | No specific issue-record or handoff pain point is documented. | Keep actions traceable in the shared issue without assuming what the team needs to see. | Reported: HVE-Managed Service.pptx, slides 2, 4; context discovery research. |

## Cross-Journey Findings

* The reported burden begins before a confirmed incident: engineers repeatedly check even when nothing concerning is happening.
* The proposed MVP automates some analysis and recordkeeping, but does not retrieve additional logs or execute recovery. The engineer remains responsible for recovery.
* The sources describe a concept and internal experience, not a validated user journey. Thoughts, emotional states, detailed interactions, and outcomes remain unknown.

## Potential Benefits and Workflow Fit

### Supported Findings

| Experience | Potential end-user benefit | Fit with the existing workflow | Evidence |
|---|---|---|---|
| Teams as the request and approval surface | Lets on-call engineers ask for detection or diagnosis and review a proposed recovery in the collaboration tool described for the concept. | Use Teams at the incident trigger and decision points, while keeping the engineer as approver and GitHub Issue as the shared record. | HVE-Managed Service.pptx, slides 2-6; .copilot-tracking/research/2026-11-23/sre-bot-managed-service-session-context.md |
| GitHub Issue audit trail | Gives the engineer and wider team a shared record of the request and bot actions, which may reduce the need to reconstruct incident context across tools. | Link the request, bot analysis, proposed recovery, engineer decision, and later outcome to the matching issue instead of creating a separate incident narrative. | HVE-Managed Service.pptx, slides 2-4; .copilot-tracking/research/2026-11-23/sre-bot-managed-service-context-summary.md |
| Human-approved recovery | Keeps the engineer in control while the bot contributes analysis and a proposed fix. | Present the evidence and uncertainty before recovery so the engineer decides whether and how to proceed. | HVE-Managed Service.pptx, slides 2-3 and 6; D6 in .copilot-tracking/research/2026-11-23/sre-bot-context-discovery-research.md |
| Copilot grounding and drafting in organizational experiences | Microsoft 365 Copilot Chat can work across supported organizational content and draft summaries or updates in eligible applications. | A future Teams or Outlook surface could help an engineer summarize incident activity or draft a status update from the agreed source record. This is a platform capability, not evidence that it solves the incident workflow. | Microsoft Learn, [Overview of Microsoft Copilot Chat](https://learn.microsoft.com/en-us/copilot/overview); Microsoft Learn, [Microsoft Copilot hub](https://learn.microsoft.com/en-us/copilot/) |
| Teams meeting and call recap | Teams can create recaps with summaries and action items for calls or meetings. | If incident handoffs or war-room reviews are recorded in Teams, an approved recap could help the team identify decisions and next actions. | Microsoft Learn, [Intelligent recap for Teams calls, meetings, and events](https://learn.microsoft.com/en-us/microsoftteams/intelligent-recap-calls-meetings) |

### Assumptions Requiring Validation

* Microsoft 365 Copilot in Teams may reduce time spent reviewing alert context or drafting a shared update. This benefit is not supported for on-call users until measured in a realistic incident workflow.
* Microsoft 365 Copilot may help an engineer compare the alert, Teams discussion, or GitHub Issue history. Grounding, authorization, data residency, and permitted incident sources must be confirmed; this does not imply access to Azure logs or GitHub data that the MVP does not currently retrieve.
* An Outlook summary or draft update may fit an existing escalation, handoff, or stakeholder-update workflow. No user evidence establishes that Outlook is used by the intended incident team or that email is an acceptable incident surface.
* Teams meeting recaps may shorten handoffs or preserve action items. The concept evidence does not show that incident meetings are held or recapped today.
* A Copilot-generated RCA or recovery proposal could be mistaken for a verified diagnosis. The experience must distinguish sourced evidence, generated analysis, unknown context, and the engineer's decision.

### Workflow Boundaries

* Keep the MVP on the documented trigger and approval boundaries: Azure Monitor alerts and Teams requests; diagnosis from supplied alert or message content; engineer-controlled recovery; GitHub Issues as the shared record.
* Do not present Outlook as an MVP alert-intake channel. The workshop evidence lists email and web intake as later scope.
* Do not claim that Copilot or Teams is unique, faster, or more consistent until baseline incident data and user testing are available.
* Do not add automatic recovery, approval bypass, or unbounded Azure or GitHub access without a separate decision and research on control, authorization, and safety.

## Validation Priorities

### Participants

* Recruit 6-10 on-call engineers or SRE practitioners with at least two different levels of incident experience. Include one or more engineers who also hand off or review incidents.
* Include incident managers or team leads who receive escalation updates and approve or coordinate recovery boundaries.
* Include a GitHub repository or team administrator and a Microsoft 365 or Teams administrator for controls and integration feasibility.
* Include security, privacy, or compliance reviewers if synthetic incident records or organizational content may be surfaced to Copilot.
* For accessibility validation, include participants who use keyboards or screen readers if they are part of the intended population. Do not assume a cohort or need without direct recruitment and consent.

### Questions

* How do engineers detect, assess, communicate, and recover from an incident today, including quiet periods and interruptions?
* Which information is available at first alert, which context is normally missing, and how much time is spent locating it?
* What evidence is needed to trust or reject a diagnosis, proposed recovery, or escalation update?
* When would a Teams response help, and when would an engineer prefer the GitHub Issue, Azure tools, or another system?
* What should a shared incident record contain, and what information must remain in source systems or be excluded?
* What would make a Copilot summary, Teams recap, or Outlook draft more useful than creating a manual update?
* Where could an AI-generated statement cause confusion, overconfidence, permission risk, or unsafe recovery action?

### Methods

* Run a 60-90 minute contextual interview per participant about a recent incident and a quiet period. Use scenario walkthroughs rather than live production actions.
* Use synthetic alerts and an approved test Teams workspace and GitHub repository for usability sessions. Include incomplete or misleading alert content, unavailable or incorrect analysis, and a proposal that should be rejected. Ask participants to find context, identify uncertainty, review or correct the analysis, record a decision, and begin handoff.
* Compare the proposed flow with a manual baseline using the same synthetic incident. Measure first-understanding time, time to locate missing context, issue-update time, handoff questions, wrong-action risk, and subjective confidence.
* Run a small diary or shadowing study over several rotations to observe actual checking frequency and handoff patterns. Use only approved, deidentified evidence.
* Test a Copilot-in-Teams summary and a Copilot-in-Outlook draft as optional prototypes only after source permissions and data boundaries are confirmed. Ask participants to edit, correct, and explain whether they would send or approve each output.
* Conduct an accessibility pass covering keyboard use, screen-reader output, status announcements, focus, contrast, target size, zoom, and recovery from errors.

### Evidence of Success

* User need: most participants describe a recurring cost in checking, context gathering, handoff, or update writing, and they identify the proposed intervention as relevant to that cost.
* Task effectiveness: participants complete diagnosis, approval, recordkeeping, and handoff steps with fewer missing-context questions and fewer unsafe or mistaken decisions than the agreed baseline.
* Time and effort: the agreed synthetic scenario shows a meaningful reduction in time or manual steps for the measured outcome without a harmful increase in rework or approval time.
* Trust and control: participants can identify the bot's evidence, missing context, and assumptions; they understand who can approve or recover and can stop or correct the flow.
* Shared record: participants can find the incident request, bot actions, decision, and next steps in GitHub Issues without relying on an undocumented side channel.
* Content quality: edited Copilot summaries and updates contain accurate, sourced incident details and preserve uncertainty and approvals. The team sets a tolerance for unsupported or omitted information before testing.
* Accessibility: all critical incident actions remain usable by the recruited participant set at keyboard, screen-reader, zoom, and status-communication levels defined for the test.
* Failure safety: participants recognize incorrect or incomplete analysis, and the test prevents recovery execution unless an authorized engineer approves the documented action.

These success measures are proposed research thresholds, not achieved results. Establish baselines and quantitative targets with the team before a comparative study.

## Accessibility Considerations

* Validate keyboard reachability, logical and visible focus, and a clear way to reject, correct, or stop each critical step, including proposal review.
* Ensure request fields have labels, and missing-context, analysis-complete, error, and handoff updates are communicated without forcing focus. Keep the content's heading structure logical.
* Check text contrast, avoid color-only status cues, provide adequately sized targets, and verify usability at 200% text zoom.
* These are design checks for the proposed experience. They do not establish that any particular accessibility need or user cohort is present.

## Human Review

> [!CAUTION]
> **Disclaimer:** This agent is an assistive coaching tool only. It does not conduct user research, observe stakeholders, or speak for the people whose problems you are designing for, and it does not replace primary research, direct stakeholder contact, design review, or product and strategy decision authority. Personas, problem statements, journey maps, empathy maps, concept tests, and other Design Thinking artifacts produced with this tool are scaffolding for your own research and synthesis — not substitutes for real stakeholder voice or observed behavior. Validate all AI-generated assumptions, personas, themes, and insights against actual stakeholders before treating any Design Thinking artifact as a basis for product, design, or strategy commitments. Outputs from this tool do not constitute validated research findings or design approval.

- [ ] Reviewed and validated by a qualified human reviewer
