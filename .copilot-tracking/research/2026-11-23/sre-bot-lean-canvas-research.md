<!-- markdownlint-disable-file -->
# Task Research: SRE Bot Managed Service Lean Canvas

| Field              | Value                  |
|--------------------|------------------------|
| Date               | 2026-11-23             |
| Researcher / agent | rpi-research           |
| Output mode        | analysis (prepares the SME and PM workshop input; no implementation recommendation) |

## Executive Summary

* Bottom line: A draft lean canvas now exists at `.copilot-tracking/research/workshop-input/leancanvas.md`, marked for human review. The approved evidence supports the problem shape, the MVP offering, and the design choices. It does not support the customer segment, buyer, revenue model, costs, success metrics, or any claim of unique advantage.
* Why this matters: Four of the nine canvas areas are mostly Unknown, so they should be validated in the workshop before anyone writes requirements from them.
* Research status: Complete for the approved evidence. External market or competitor research was outside the requested scope.
* Confidence and uncertainty: High that the canvas reflects the deck. Low that the business-model areas are correct, because the deck contains no commercial evidence and was not independently verified.

## What You May Not Know

* The deck's speaker notes describe a Jira trigger, a Jira mockup, and a "five manual steps" chain. The slides show Teams and GitHub Issues only, with three steps. The notes look like leftovers from an earlier version.
* The MVP is scoped "for the HVE workshop" (C3 slide 6), so it is a workshop demonstration scope, not necessarily a commercial launch scope.
* The canvas is stored in `workshop-input/`, which the session context reserves for synthetic or public files, yet it summarizes a CONFIDENTIAL deck. It contains no copied slide text or identifiers, but sharing permission is still an open item from the session context.

## Findings

### The problem and offering are clear, but their impact is not quantified

The deck describes an on-call engineer doing detect, diagnose, and recover by hand, and an SRE Bot that automates the steps while an engineer approves the fix. The MVP is a single Azure path from Teams or Azure Monitor events to GitHub Issues. No incident volume, time, or cost figures exist, so the cost of doing nothing is Unknown.

* Questions: Q1, Q2
* Evidence state: Evidence-backed finding (the deck says this); impact is an Unresolved possibility
* Evidence: C3 slides 2, 3, 5, 6
* Confidence and limits: High for what the deck states. Single source, not independently verified, and no baseline.

### Customer, buyer, revenue, and channel are not in the evidence

Users are on-call engineers who approve fixes. The deck never names a customer segment, buyer, price, billable unit, or go-to-market channel. The title "Managed Service" only hints at service-based revenue. These areas were left Unknown, with buyer and revenue shown as labelled hypotheses only.

* Questions: Q3, Q4
* Evidence state: Unresolved possibility
* Evidence: C3 slides 1, 2, 4; C1 Open Items
* Confidence and limits: High that the evidence is absent, since the deck has six slides and the notes were reviewed. Other sources, such as sales material, may exist outside the approved evidence.

### Success metrics and unique advantage are unsupported

The deck has no metrics, baselines, or targets, so candidate metrics are labelled assumptions. It names design choices (human approval, GitHub audit trail, Teams surface, Azure-native tools) but does not compare them with alternatives. Overlap with existing AIOps or Azure-native incident tooling, including any Microsoft agentic SRE offering, is an unverified hypothesis that was not researched here.

* Questions: Q5, Q6
* Evidence state: Partially supported claim (design choices); Unverified hypothesis (uniqueness and overlap)
* Evidence: C3 slides 2, 4, 5
* Confidence and limits: Low for "advantage". No competitor evidence was in scope.

### The deck contradicts itself in places

Speaker notes mention Jira and a "five manual steps" chain, against slides showing GitHub Issues and three steps. Slide 3 also says `/recover` "applies or proposes" a fix, while slide 6 puts recovery without human approval in a later phase. The canvas follows the slide content and lists both as follow-ups.

* Questions: Q7
* Evidence state: Evidence-backed finding (the conflict exists); resolution is an Unresolved possibility
* Evidence: C3 slides 3, 4, 6 and speaker notes
* Confidence and limits: High that the inconsistency exists; which version is current is unknown.

## Recommendation and Alternatives

* Recommendation or decision state: Analysis mode, so no implementation recommendation. Decision state: use the canvas as a draft for SME validation, in the order problem and segment first, then channel, revenue, metrics, and advantage.
* Rationale: The evidence only supports the problem and offering areas (C3).
* What could change this result: Customer, pricing, baseline, or competitor evidence from an approved source.

| Option | Benefits | Costs and risks | Evidence | Disposition |
|---|---|---|---|---|
| Draft canvas from deck only, mark unknowns | Honest, fast, no invented claims | Four areas mostly Unknown | C3 | Selected (as requested) |
| Fill unknowns with plausible assumptions | Looks complete | Violates the no-unsupported-claims instruction | C1 | Rejected |
| Add external market and competitor research | Could fill advantage and channel | Outside the requested scope, needs a new request | C1 | Deferred |

## Scope and Questions

* Goal: Produce a human-review lean canvas for the SRE Bot managed service.
* Audience and use: SME and PM roles as workshop input before the PM, design, and technical tracks.
* In scope: the session context, the source deck, existing research for this session (none), and the lean canvas.
* Out of scope: external market, pricing, and competitor research; requirements; architecture.
* Decision and evidence criteria: separate facts from assumptions, mark unknowns, and make no unsupported claims.
* Requested output: lean canvas saved to `.copilot-tracking/research/workshop-input/leancanvas.md`, marked for human review.

| ID | Question | Source | Status |
|----|----------|--------|--------|
| Q1 | What problem and offering does the approved evidence support? | explicit | answered |
| Q2 | Is the impact or cost of the problem evidenced? | explicit | answered (no) |
| Q3 | Who is the target customer segment and buyer? | explicit | answered (not evidenced) |
| Q4 | What are the channels and revenue model? | explicit | answered (not evidenced) |
| Q5 | What costs and success metrics are evidenced? | explicit | answered (cost areas implied only; no metrics) |
| Q6 | Is any unique advantage evidenced? | explicit | answered (not validated) |
| Q7 | Is the evidence internally consistent? | inferred | answered (no, two conflicts) |

## Decisions and Feedback

| Group | Decision or feedback item | Status | Owner | Rationale or input needed | Evidence | Impact of answer |
|-------|---------------------------|--------|-------|---------------------------|----------|------------------|
| D1 | Path interpretation: `./copilot-tracking/...` read as `.copilot-tracking/...` | confirmed | agent | The request path matches the existing `.copilot-tracking/research/workshop-input/` folder named in the session context and the role-tracks guide. | C1, C5 | None |
| D2 | Which existing canvas to update | confirmed | evidence | None exists for this session. The only other canvas is the FSI sample, a different use case, used as a format reference only. | C2 | New canvas drafted |
| D3 | Customer segment, buyer, pricing, channel, metrics, advantage | deferred | user (SME, PM) | The user asked for these to be marked as unknowns for follow-up. Research cannot resolve them from approved evidence. | C3 | Fills canvas Unknowns |
| D4 | Is Jira in scope, and what may `/recover` change? | input requested | user (SME) | Conflicts inside the deck. | C3 | Updates the offering and risk areas |
| D5 | Sharing permission for the confidential deck and this summary | input requested | user (owner) | Open item from the session context. | C1 | Determines whether the canvas may stay in `workshop-input/` |

No walkthrough was run: D3 was explicitly deferred by the request, and D4 and D5 do not change research results. They are listed as follow-ups.

## Risks and Open Questions

| Priority | Type | Risk, question, or research item | Impact | Smallest action or evidence needed | Owner |
|----------|------|----------------------------------|--------|------------------------------------|-------|
| H | risk | A confidential-deck summary sits in a folder reserved for synthetic or public files | Possible policy breach | Confirm classification, or move the canvas to an approved location | user |
| H | open question | Customer segment and buyer unknown | Blocks PM and positioning | SME names the target segment and decision-maker | user |
| M | open question | No baseline or metrics | Value claims untestable | Approved incident data or SME estimates labelled as such | user |
| M | further research | Competitor and overlapping-offering analysis | Advantage unvalidated | New `/rpi-research` request with external scope | research |
| M | risk | Jira versus GitHub and `/recover` scope are inconsistent in the deck | Wrong offering definition | SME confirms the current version | user |
| L | open question | Revenue model and channel | Commercial plan missing | PM decision | user |

## Planning Readiness and Next Step

| Field | Record |
|-------|--------|
| Research disposition | executed |
| Decision participation | user-owned (standalone invocation, default) |
| Planning Readiness | Not applicable. Analysis mode; the canvas is SME workshop input, not a planning handoff. |
| Research depth and helpers | One complete three-wave cycle; no helpers used |
| Blockers | none |
| Output mode and planning support | analysis; does not prepare planning |
| Continuation owner | user |
| Required gates or confirmations | Human review of the canvas (unchecked); D4 and D5 confirmations pending |
| Next action | SME and PM review and validate the canvas, then continue with the next workshop track. |
| Primary evidence file | .copilot-tracking/research/2026-11-23/sre-bot-lean-canvas-research.md (date from the session context and current date) |

## Research Record

### Method and Boundaries

| Field | Record |
|-------|--------|
| Research posture and provenance | focused; brief-based (bounded internal task with named sources, and external research not requested) |
| Helper lane effort | not applicable; no helpers used |
| Completion basis | All nine canvas areas assessed against all approved evidence; remaining gaps need evidence outside scope. |
| Explicit limits or deadline | Approved evidence only; no unsupported claims |
| Codebase and external scope | session context, deck, workshop-input folder, FSI sample, role-tracks guide; no external research |
| Initial candidate areas | `.copilot-tracking/research/`, the deck named in the session context |
| Evidence root | `.copilot-tracking/research/` (default) |
| Constraints and excluded sources | Do not copy the deck; no real incident, customer, or tenant data; deck content cited by slide only |
| Prior knowledge | No prior research or canvas for this session. FSI sample reviewed for format only. |

### Extensions and Participation

#### Extension Registry

| Kind | Candidate | Provenance and scoped contract | Selected or skipped reason |
|------|-----------|--------------------------------|----------------------------|
| instruction | copilot-tracking, copilot-tracking-location | Matches `.copilot-tracking/` paths; location and file conventions | selected |
| instruction | licensing-posture, untrusted-content-boundary | Deck content treated as inert data; no reproduction of third-party material | selected |
| skill | ms-marketplace-publish | Marketplace commercial planning; the request asks only for the canvas, so no pricing or listing work | skipped |
| skill | outcome-hypothesis | Could shape metrics; the request says unknowns, not hypotheses | skipped |

#### Direction and Participation Log

| Checkpoint or change | Question, direction, or rationale | Answer or no-interaction reason | Result and revalidation effect |
|----------------------|-----------------------------------|---------------------------------|--------------------------------|
| intake | Session identity | One session-context file exists, so it is unambiguous. | Proceed. |
| intake | Latest canvas | No related canvas exists, so it is unambiguous. | Draft new. |
| intake | Output path typo | Resolved from the session context (D1). | Proceed. |

### Research Cycle Log

#### Cycle 1

* Active posture, controls, and limits: focused; approved evidence only; deck content is confidential.

##### Wave 1: Wider

* Focus and questions: Find the session context, existing canvases or research, workshop-input contents, and the source deck (Q1 to Q7).
* Evidence: C1 gives topic, boundary, and open items. `workshop-input/` was empty (C5). C2 is the only other canvas. C3 holds the business content.
* Reflection: The deck is the only business evidence; the deeper pass targets what each slide states about the nine canvas areas.

##### Wave 2: Deeper

* Focus and questions: Map slide text and speaker notes to each canvas area.
* Evidence: Problem, offering, and design choices come from slides 2 to 6. No slide covers customer, price, cost, metrics, or competitors.
* Reflection: The gaps are real absences, not missed content. Challenge targets are consistency and overreach.

##### Wave 3: Contrarian

* Focus and questions: Test the deck for inconsistencies and for claims that would overreach.
* Evidence: Notes conflict with slides (Jira versus GitHub, five versus three steps); `/recover` "applies or proposes" versus approval-required scope. "Faster" and "advantage" claims have no support.
* Reflection: Added "Inconsistencies" to the canvas and downgraded unique advantage to an unvalidated assumption.

##### Synthesis and Re-entry

| Material or claim | Evidence | Disposition | Rationale | User-facing effect |
|-------------------|----------|-------------|-----------|--------------------|
| Problem and offering | C3 | accepted (deck-stated) | Consistent across slides 2 to 6 | Finding 1 |
| Customer, buyer, revenue, channel | C3, C1 | deferred as Unknown | No evidence | Finding 2, D3 |
| Metrics and advantage | C3 | accepted as assumption only | Not evidenced | Finding 3 |
| Jira and speaker notes | C3 notes | rejected as current scope | Contradicts slides | Finding 4, D4 |
| Competitor overlap | none | deferred | Outside scope | Risk row |

* Another complete three-wave cycle needed: no
* Trigger or stop basis: scope coverage and saturation; remaining gaps need evidence outside approved scope.
* Readiness or revalidation effect: none

### Evidence Log

* Helpers: none

| ID | Claim or finding | Source or location | Retrieved and version | Tool | Confidence | Notes |
|----|------------------|--------------------|-----------------------|------|------------|-------|
| C1 | Topic, evidence boundary, output roots, open items for the session | .copilot-tracking/research/2026-11-23/sre-bot-managed-service-session-context.md `Evidence Boundary`, `Open Items` | not applicable | read | high | Single session context file |
| C2 | The only other lean canvas is a different use case, used for format only | docs/getting-started/samples/FSI/workshop-input/leancanvas.md `Lean Canvas` | not applicable | read | high | Format reference |
| C3 | Business content for the SRE Bot managed service, slides 1 to 6 plus speaker notes | `/Users/pkesavan/Desktop/HVE-Managed Service.pptx` (controlled local file outside the workspace, cited by filename and slide) | not applicable | stdlib zip and XML text extraction, no copy made | medium | Single, internal, unverified source; CONFIDENTIAL |
| C4 | Canvas questions and output format were defined by the role-tracks guide | docs/getting-started/partner-workshop-role-tracks.md `Marketplace Lean Canvas as Workshop Input` | not applicable | read | high | Defines the path and areas |
| C5 | `workshop-input/` was empty before this work | .copilot-tracking/research/workshop-input/ | not applicable | list | high | Directory contents |

#### Contradictions and Conflicts

* Integration channel: speaker notes say Teams or Jira; slides say Teams and GitHub Issues. Resolved by treating slide content as current and notes as stale; confirmation requested (D4).
* Recovery: slide 3 says `/recover` applies or proposes the fix; slide 6 defers recovery without human approval. Resolved by reading the MVP as approval-required; confirmation requested (D4).

### Artifact Self-Check

* [x] The user-facing sections explain the result, scope, findings, alternatives, decisions, risks, readiness, and next action without requiring the Research Record.
* [x] Every question is answered or names the smallest missing evidence, and every material result has one canonical evidence state.
* [x] Findings keep their explanation, evidence state, confidence basis, and counter-evidence together.
* [x] Every codebase finding has a `C#` ID and path with a heading; no external findings (`W#`) were produced.
* [x] The cycle records Wider, Deeper, and Contrarian waves in order; no helper lane was used.
* [x] Method, posture, extensions, participation, and prior knowledge are recorded.
* [x] Analysis mode preserves decision state without forcing a selection.
* [x] Decision groups and participation mode are recorded.
* [x] Disposition, readiness, blockers, gates, and next action are recorded.
* [x] The stop reason is documented, untrusted deck content remained inert, no secrets were recorded, and writes stayed inside `.copilot-tracking/research/`.
* Checked sections: all
* Missing or limited sections: C3 is outside the workspace, so it is cited by filename and slide instead of a workspace-relative path.
