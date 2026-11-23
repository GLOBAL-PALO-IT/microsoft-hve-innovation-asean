<!-- markdownlint-disable-file -->
<!-- markdown-table-prettify-ignore-start -->
# Handoff — SRE Bot Managed Service PRD

## Destination and purpose

* Platform: GitHub Issues.
* Repository: `GLOBAL-PALO-IT/microsoft-hve-innovation-asean`.
* Source of truth: `issues-plan.md`; supporting analysis: `issue-analysis.md`.
* Handoff status: **for user review only; not executable yet**.
* This is a planning-only handoff. No tracker item was created, updated, linked, commented on, closed, or published.

## Blocking scope reconciliation

The supporting RPI plan records prior user direction for permission-aware retrieval, human escalation, and confirmation before external actions. PRD DD-001 limits analysis to supplied alert/Teams content, and the confirmed MVP keeps recovery execution with the engineer. These directions are not reconciled in an approved PRD.

Keep **IS001, IS004, and IS005** marked `needs_review`; the entire handoff remains **non-executable** until a user/owner decision is reflected in an approved PRD. That decision must clarify whether retrieval is in scope and, if so, its permitted source/scope; what human escalation means; and how confirmation applies without changing the no-service-executed-recovery boundary. Do not invent retrieval scope or treat the prior direction as withdrawn. The existing `map-journey.md` remains the canonical experience artifact and is not replaced or duplicated.

## Execution gate

**Do not dispatch creates yet.** The read-only discovery surface did not provide `get_me`, `list_issue_types`, or `get_label`. Repository searches returned no issues, so parent/sub-issue support could not be confirmed from an existing example. No labels or issue types are asserted, and all candidate relationships are flattened and `needs_review`.

Before a separate execution pass:

* [ ] Validate GitHub identity and destination access using the read-only identity capability.
* [ ] Discover organization issue types and confirm whether `Feature` and `Task` are supported.
* [ ] Discover each label that would be applied; if no label can be validated, resolve the create-field requirement rather than guessing.
* [ ] Confirm sub-issue support and allowed parent/child configuration; otherwise retain a flat issue list.
* [ ] Complete human review of the BRD, PRD, and canonical UX journey; re-evaluate the PRD validation gate and complete the supporting RPI plan critique.
* [ ] Reconcile the RPI user direction and PRD DD-001 through a user/owner decision recorded in an approved PRD; keep IS001, IS004, and IS005 blocked until then.
* [ ] Resolve the blocking requirement decisions identified in `issue-analysis.md`; preserve any remaining unknowns in resulting issue text.
* [ ] Rebuild and review create payloads, including checked acceptance criteria and applicable field values, against the validated repository configuration.
* [ ] Obtain user approval before any tracker mutation.

## Planned operations

No operation is currently authorized or queued. Candidate order below is the proposed **Create** order only, if all gates pass. Candidate references are local planning identifiers and must not be sent to GitHub.

### Create — blocked candidates

* [ ] IS001 — `feat: establish the reviewed SRE Bot MVP contract` — `needs_review`.
* [ ] IS002 — `feat: accept and normalize the supported incident events` — `needs_review`.
* [ ] IS003 — `feat: identify incidents and reuse existing records` — `needs_review`.
* [ ] IS004 — `feat: produce grounded analysis and non-executing recovery proposals` — `needs_review`.
* [ ] IS005 — `feat: record engineer decisions and auditable actions` — `needs_review`.
* [ ] IS006 — `feat: report incident status and expose operational health` — `needs_review`.
* [ ] IS007 — `test: validate accessibility and the workshop incident workflow` — `needs_review`.
* [ ] IS008 — `docs: assess provisional Marketplace and Agent Store readiness` — `needs_review`; readiness assessment only, no publication.

No type, label, milestone, assignee, or API payload has been specified for these blocked candidates.

### Update

* None.

### Link — not queued

* Proposed logical grouping IS001 → IS002–IS008 is not a validated GitHub relationship. Keep flat unless read-only discovery confirms support and the user approves the hierarchy.

### Comment

* None.

### Close

* None.

### No Change

* No existing issues were found.

## Marketplace and Agent Store publication boundary

IS008 is only a provisional evidence-gathering assessment. Current Microsoft Marketplace/Agent Store guidance, Partner Center state, tenant feasibility, offer eligibility, identity/permissions, data handling, packaging, and review requirements have not been verified. A workshop-local demonstration is not publication approval. Do not publish, submit, or claim approval/readiness in this pass.

## Review record

* [ ] User reviewed the candidate scope, flattened structure, and all `needs_review` gates.
* [ ] User approved a revised executable handoff.
* [ ] Separate execution pass assigned to the GitHub Backlog Manager only after review.

## Planning files

* `planning-log.md`
* `issue-analysis.md`
* `issues-plan.md`
* `handoff.md`

<!-- markdown-table-prettify-ignore-end -->
