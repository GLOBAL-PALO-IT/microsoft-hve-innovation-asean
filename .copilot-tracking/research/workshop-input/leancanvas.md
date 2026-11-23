<!-- markdownlint-disable-file -->

# SRE Bot Managed Service Lean Canvas

| Field | Value |
|---|---|
| Date | 2026-11-23 |
| Session | sre-bot-managed-service |
| Status | DRAFT for human review |
| Evidence boundary | Source deck is marked CONFIDENTIAL (PALO IT). This canvas is a summary-level synthesis cited by slide number. No slide text, incident data, customer data, or identifiers are copied. |

- [ ] Reviewed and validated by a qualified human reviewer

> [!IMPORTANT]
> The only business evidence is one internal six-slide deck, which describes the
> product concept and MVP scope. It contains no customer, market, pricing, cost,
> baseline, or competitor evidence. Every statement tagged **Deck-stated** is the
> deck's own claim, not independently verified. Everything else is an
> **Assumption** or **Unknown** for workshop validation.
>
> This file sits in `workshop-input/`, which the session context reserves for
> synthetic or public material. Confirm the deck's classification and sharing
> permission before using this canvas beyond the team.

## Lean Canvas

| Canvas area | Draft workshop input | Evidence state |
|---|---|---|
| Problem | On-call engineers run every incident step by hand: respond to the alert, search logs and diagnose, then recover (C3 slide 3). The deck frames this as alert fatigue (C3 slide 3). Frequency, time lost, cost of downtime, and the cost of doing nothing are not stated. | Deck-stated (problem shape). Unknown (impact and cost of inaction) |
| Target customer segments | **Users:** on-call or SRE engineers who request actions in Teams and approve the final fix (C3 slides 2, 3, 4). **Customer organization, industry, size, and buyer or decision-maker:** not stated. A managed-service buyer such as an IT operations or platform leader running Azure workloads is a plausible hypothesis only. | Deck-stated (user role). Unknown (customer, buyer). Assumption (buyer hypothesis) |
| Value proposition | Faster, more consistent recovery with human oversight and an auditable system of record (C3 slide 2). "Faster" and "more consistent" have no baseline or target. | Deck-stated (claim). Unknown (magnitude) |
| Core offering | MVP runs one alert-to-issue path on Azure. Triggers are Teams requests and Azure Monitor alerts. Azure Functions and Service Bus ingest events. A Foundry agent with Azure OpenAI classifies the event and calls five GitHub Issues operations. Slash commands `/detect`, `/analysis`, `/recover` run detection, root-cause analysis, and a proposed or applied fix. An engineer approves the final fix, and recovery without approval is a later phase (C3 slides 3, 5, 6). Every request and bot action is written to a GitHub Issue (C3 slides 2, 4). | Deck-stated |
| Channels | **Product interaction surface:** Microsoft Teams via Azure Bot Service (C3 slides 4, 5, 6). **Go-to-market, sales, and delivery channel** (direct managed service, partner-led, Microsoft Marketplace): not stated. The deck scopes the MVP "for the HVE workshop", so a commercial launch plan is not implied. | Deck-stated (interaction surface). Unknown (go-to-market) |
| Revenue model | Not stated. The deck is titled "Managed Service" (C3 slide 1), which suggests service-based fees, but pricing, billable unit, contract term, and willingness to pay are unknown. | Unknown |
| Cost structure | The architecture implies cost areas for Azure Bot Service, Azure Functions, Service Bus, Foundry Agent Service and Azure OpenAI consumption, Application Insights, Key Vault, and GitHub (C3 slides 5, 6). People costs (build, operate, on-call support, onboarding) and compliance review are likely but unstated. No amounts are given. | Assumption (cost areas from architecture). Unknown (amounts) |
| Success metrics | The deck defines none. Candidates only: time from alert to issue created, time to root-cause analysis, time to recovery, share of incidents with a bot analysis, share of proposed fixes approved, engineer hours per incident, repeat-alert rate. Baselines and targets are unknown. | Assumption (candidates). Unknown (baselines, targets) |
| Unique advantage | Deck-stated design choices: human approval stays in control, a GitHub Issue audit trail for every action, Teams as the working surface, and an Azure-native governed tool path (C3 slides 2, 4, 5). Whether any of this is unique or durable is unvalidated, and no competitor or alternative is analysed. Overlap with existing AIOps or Azure-native incident tooling has not been checked. | Deck-stated (design choices). Assumption (as an advantage). Unknown (durability) |

## Evidence-backed facts (deck-stated, not independently verified)

* The concept automates detect, diagnose, and recover, with an engineer approving the final fix (C3 slides 2, 3).
* The MVP has two triggers (Teams requests, Azure Monitor alerts), one ingestion path, and five GitHub Issues operations (C3 slides 5, 6).
* Recovery without human approval, on-prem and SNMP alerts, scheduled jobs, AWS and GCP health events, file uploads, email and web intake, and non-Azure or local LLMs are later phases (C3 slides 5, 6).
* Every Teams request and bot action is written back to a GitHub Issue as the system of record (C3 slides 2, 4).

## Inconsistencies to resolve

* The deck's speaker notes refer to a Jira mockup, a Jira comment trigger, and a "five manual steps" as-is chain. The slides show Teams and GitHub Issues only, with three steps. The notes look carried over from an earlier version. This canvas follows the slide content. Confirm whether Jira is in or out of scope (C3 slides 3, 4, speaker notes).
* Slide 3 says `/recover` "applies or proposes" the fix. Confirm what the MVP may change in production and what approval gate applies (C3 slides 3, 6).

## Unknowns for follow-up

1. Which customer segment and which decision-maker or budget owner should the managed service serve first? (SME, PM)
2. What is the current incident volume, time to detect, diagnose, and recover, and the cost of downtime? (SME, with an approved data source)
3. Will real or synthetic incident evidence, runbooks, and SOPs be used, and where will they be stored? (SME)
4. Is this sold as a managed service, a Marketplace offer, or both, and what is the billable unit and price? (PM)
5. Which success metrics, baselines, and targets will prove the MVP works? (SME, PM)
6. Which alternatives exist today, including manual runbooks and current AIOps or Azure-native tooling, and where does this offer differ? (PM, requires research beyond the approved evidence)
7. What recovery actions may the bot take with approval, and which are never allowed? (SME, security)
8. Is Jira in scope, or is GitHub Issues the only system of record? (SME)
9. Can the source deck and this summary be shared beyond the team? (Owner)

## Workshop validation

The SME should confirm the problem impact and customer segment first. The PM should
not turn pricing, channels, metrics, or advantage claims into requirements until
evidence or an accountable owner confirms them.

## Source

* C1: .copilot-tracking/research/2026-11-23/sre-bot-managed-service-session-context.md
* C2: docs/getting-started/samples/FSI/workshop-input/leancanvas.md (format reference only, different use case; no content reused)
* C3: HVE-Managed Service.pptx, six slides plus speaker notes. Controlled local file outside the repository, cited by filename and slide, not copied.
* C4: .copilot-tracking/research/workshop-input/ contained no files before this canvas was written.
* Research record: .copilot-tracking/research/2026-11-23/sre-bot-lean-canvas-research.md
