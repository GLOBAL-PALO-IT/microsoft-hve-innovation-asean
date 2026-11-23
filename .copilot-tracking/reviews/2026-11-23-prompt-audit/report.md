# Prompt Audit Report: hve-paloit

Date: 2026-11-23. Produced by `/claude-api prompt-audit`. Companion artifact: `proposed.patch` in this directory (unified diff against the working tree, generated from a scratch copy; nothing in the repository was modified).

## 1. Assumptions (Step 0)

Scope. No file was named in the request, so the scope is the whole working directory's prompt surface: everything under `.github/` that reaches a model as text (agents, instructions, prompts, skills and their references, agentic-workflow prompt bodies) plus the root `copilot-instructions.md` and the plugin manifest that those files describe. Files outside the working directory (user-level Claude or Copilot configuration) were not read. `.mcp.json`, `.vscode/mcp.json`, and the generated `.github/workflows/*.lock.yml` files were not read. `docs/` is documentation for humans, not prompt surface, and was consulted only to verify facts the prompt files assert.

Target model. These files are read by a coding agent, not by application code, and the request named no model, so the default target is the model running this audit, Claude Fable 5.1. The primary harness documented by this repository is GitHub Copilot in VS Code (the workshop setup guide tells participants to select the Copilot Fusion model picker entry), and `.github/plugin.json` also packages the same files as a Claude Code plugin. Per-file overrides: three subagents pin `Claude Haiku 5.5 (copilot)` and were audited against Claude Haiku 5.5; six subagents pin GPT models (`GPT-6 Sol`, `GPT-6 Luna`, `GPT-5.6 Terra`) and were audited for Group 2 only, with no Claude-behavior claims and no proposal to switch them. Three prompt files pin an array `MAI-Code-1-Flash / Claude Haiku 4.5`; the pin itself is a finding.

Non-Anthropic provider markers. The agent and prompt frontmatter pins above, and the gh-aw lock files listing GPT and Gemini engines, are recorded here as context. No application code calls any LLM API.

Provenance. Git history is a single `Initial commit`, so `git blame` gives no provenance. Idiom-dating alone is Low confidence; Medium or High was granted only where a finding matches a documented row tied to target-model behavior, or where the repository itself contradicts the text.

Authoritative house standard. `.github/instructions/hve-core/hve-builder.instructions.md` is the repository's own authoring standard. It already bans all-caps emphasis, manual chain-of-thought requests, repeated directives, exhaustive edge-case lists, invented numeric limits, `#file:` directives, workspace-root `.github/` paths in distributable artifacts, and unsupported frontmatter keys. Where another file disagrees with it, the standard is treated as the authoritative side; because history cannot order the two, that direction is an assumption.

The repository is a trimmed copy. Only `.github/`, `.vscode/`, and `docs/` exist. Upstream HVE Core's `scripts/`, `extension/`, `.devcontainer/`, `logs/`, `evals/`, `docs/docusaurus/`, `docs/templates/`, and `docs/reference/` trees are absent, and 80 of the 119 scripts in `package.json` invoke the missing `scripts/` tree. The audit treats a prompt claim that depends on those trees as contradicted by the repository. Whether the right fix is to rewrite those claims (the direction this report proposes) or to restore the trees is the user's decision and is flagged as finding R-1.

## 2. Inventory (Step 1)

| Surface | Files | Notes |
|---|---|---|
| Root instruction file | 1 | `.github/copilot-instructions.md`, always loaded |
| Agents and subagents | 71 | `.github/agents/**/*.agent.md` (9 pin a model) |
| Instruction files | 68 | `.github/instructions/**/*.instructions.md` |
| Prompt files (commands) | 49 | `.github/prompts/**/*.prompt.md` |
| Skills | 105 | `SKILL.md`, plus 512 reference files sampled where `SKILL.md` claims depended on them |
| Agentic-workflow prompts | 8 | `.github/workflows/*.md` bodies (gh-aw, engine `copilot`) |
| Plugin manifest | 1 | `.github/plugin.json` (configuration the root file makes claims about) |
| Request-building code | 0 | No application calls a model; Group 4 request-config rows are not applicable |

About 42,000 lines in the main files. Every backticked repository path and `npm run` command in the surface was resolved mechanically: 122 references to repository paths that do not exist, 6 skill-reference files that do not exist, 4 undefined npm scripts, and 40 `docs/<x>/` output destinations that are placeholders for the consumer workspace (not findings).

## 3. Summary

The surface is well above average. Most agents follow a "skill owns the knowledge, agent routes and gates" shape, the safety and compliance files are written at normal volume with reasons attached, and 40 of 71 agents, 31 of 76 instruction and workflow files, 15 of 49 prompts, and about 50 of 105 skills came out clean or Low-only. The house standard has already removed most model-era pressure language; what remains is concentrated in a handful of files.

Three findings matter most:

1. The always-loaded root instruction file describes a different repository (R-1). Forty-two of its path claims, six template files, the validation guide, the docs site, the extension tree, and most of the npm commands it tells the agent to run do not exist here, and it says the plugin manifest declares no hooks while the manifest does. Every session starts with this context. The patch proposes a rewrite that describes this repository and names the handful of npm scripts that actually run.
2. Four prompt files carry full copies of same-named skills that have since been rewritten (P-H7, P-H8, P-H9, and the flagged P-H4). The skill copies are shorter, at normal volume, and in three cases carry safety gates (staging confirmation, Figma write confirmation, a synthetic-data preflight) that the prompt copies lack. The patch turns three of them into thin wrappers; the git-commit case touches a safety rule and is flagged for the user.
3. Repository self-contradictions that the model must reconcile at run time (Group 2, 95 findings). The most consequential: two agents name the same WorkIQ tools differently; three agents give three answers on where an ADR template comes from; a routing table disagrees with its own reference; the Mural skill's command table names four subcommands its parser does not define; twelve prompts and three instruction files use `#file:` directives the standard says the harness ignores; and the two VEX licensing tables disagree on what may be taken from the GitHub Advisory Database (flagged, since it is a licensing rule).

Only one pattern is a documented Claude Fable 5.1 behavioral hit across the whole surface: update suppressors ("do not narrate", "suppress low-level actions") in nine RPI and engagement-reporting files. Everything else in Group 1 is register and duplication.

Counts (a finding may carry a second tag; counted once by primary row):

| Group | Findings | Notes |
|---|---|---|
| 1a pressure language | 17 | one agent in full RFC-2119 register (A-H7); the rest scattered |
| 1b scaffolds and caps | 9 | planner question-count clamps, sentence caps in the ADR persona |
| 1c over-specification | 31 | mostly repetition as reinforcement; some pseudo-code and gold examples |
| 1d fossils | 22 | update suppressors, migration-relative phrasing, retired tool-name forms |
| 1e prohibition clusters | 3 | no-provenance "AI tells" phrase lists |
| 1f output choreography | 4 | numeric ceilings in workflow and community templates |
| 2 brittle configuration | 95 | stale paths, contradictions, history narratives, pinned models |
| 3 tool descriptions | 2 | tool names in prose; no tool definitions in scope |
| 4 request config | 4 | roster redundancy only; request-building rows not applicable |

Confidence totals: 45 High, about 90 Medium, about 60 Low (flag only).

## 4. Findings

IDs: R = root file and manifest; A = agents; I = instructions and workflow prompts; P = prompts; SZ = skills i-z; SA = skills a-h. Paths are relative to the repository root. Line numbers are from the current files.

### 4.1 High confidence

**R-1** `.github/copilot-instructions.md` (lines 17, 57, 67-73, 100-111, 146-147, 160, 173, 183, 187-190, 198, 212-297)
Evidence: "Scripts (`scripts/`) - Automation for linting...", "Extension (`extension/`)...", "`docs/templates/full-review-output-format.md`...", "Run `npm run plugin:sync`...", "Run `npm run validate:frontmatter`...", "PowerShell tests run exclusively through `npm run test:ps`. Never invoke Pester or test scripts directly.", "Treat the repository root and `docs/docusaurus` independently".
Pattern: Group 2, volatile specifics contradicted by the repository.
Why: none of `scripts/`, `extension/`, `.devcontainer/`, `logs/`, `docs/templates/`, `docs/reference/`, `docs/docusaurus/`, or `docs/contributing/validation.md` exists; `npm run validate:frontmatter` is undefined; `test:ps`, `plugin:sync`, `docs:generate`, `lint:ps`, `lint:models`, and `validate:local` all invoke missing files. The file is loaded into every session.
Action: rewrite (in patch). Flag for the user: choose between the consumer-copy direction the patch takes and restoring the upstream trees.

**R-2** `.github/copilot-instructions.md:143` versus `.github/plugin.json:263`
Evidence: "The manifest declares no hooks. Hooks support was removed with the telemetry hook, so a committed `hooks` field is reported as drift rather than preserved." versus `"hooks": "hooks/shared/telemetry.json"` (file absent).
Pattern: Group 2, instruction file contradicted by configuration it describes.
Action: remove the `hooks` entry from `plugin.json` (in patch, proposed only).

**R-3** `.github/plugin.json` membership drift
Evidence: 15 listed paths do not exist (`agents/data-science/data-workstream-coach.agent.md`, `agents/marketplace/*`, `agents/security/supply-chain-reviewer.agent.md`, `prompts/hve-core/pull-request.prompt.md`, `instructions/hve-core/pull-request.instructions.md`, seven `skills/data-science/*`); 33 package-scoped files on disk are not listed (for example `agents/data-science-engineering/data-science-engineering-coach.agent.md`, `prompts/data-science-engineering/synth-data-generate.prompt.md`, the engagement-reporting package).
Pattern: Group 2, volatile specifics; the sync script that owns the manifest is absent.
Action: flag. The patch does not rewrite membership; after accepting or rejecting the agent deletions and merges below (A-H2, A-M6, A-M7, A-M8), the manifest needs one manual pass.

**R-4** Distributable planner artifacts cite `scripts/linting/schemas/*-state.schema.json` as "the authoritative schema": `.github/agents/accessibility/accessibility-planner.agent.md:84`, `security/security-planner.agent.md:294`, `security/sssc-planner.agent.md:226,255`, `.github/instructions/shared/planner-identity-base.instructions.md:31,48`, `accessibility/accessibility-identity.instructions.md:28,109`, `rai-planning/rai-identity.instructions.md:129`, `security/sssc-planner.instructions.md:556`, and the twin `.github/skills/shared/planner-identity/SKILL.md:37,54`.
Pattern: Group 2, volatile specifics; also the standard's rule against workspace-root source-of-truth paths in distributable artifacts (hve-builder line 57).
Why: the files do not exist here, and each identity file already carries the full inline schema narrative.
Action: rewrite to make the identity file's State section authoritative (in patch).

**A-H1** `.github/agents/experimental/pptx.agent.md:5-6`, `hve-core/subagents/rpi-planner.agent.md:5`, `hve-core/subagents/rpi-review-builder.agent.md:5`. Evidence: `agents:` frontmatter key. Pattern: Group 2 contradiction with hve-builder line 51 ("The Copilot harness does not support `handoffs`, `agents`, or `argument-hint`"). Action: remove the key (in patch).

**A-H2** `.github/agents/coding-standards/subagents/code-review-pr.agent.md` (whole file; line 18 "This detailer replaces the former standalone PR Walkthrough agent."). Pattern: Group 4 redundant subagent plus history narrative. Why: same skill contract and output file as `code-review-orientation`; the orchestrator at `code-review.agent.md:47` says "There is no `pr` findings perspective" and never dispatches it; its only reference is the manifest. Action: delete (in patch); flag the manifest entry (R-3).

**A-H3** `.github/agents/engagement-reporting/engagement-report-generator.agent.md:57-58` "Do not narrate internal validation steps; present the report or the specific blocker returned by the skill". Pattern: 1d update suppressor. Why: documented for Claude Fable 5.1: with this present the model under-narrates and between-tool progress arrives only as thinking. Action: rewrite to the positive clause (in patch).

**A-H4** `.github/agents/project-planning/ux-ui-designer.agent.md:161` "...this agent is the only new consumer wired in this change." Pattern: Group 2 history narrative. Action: remove (in patch).

**A-H5** `.github/agents/project-planning/meeting-analyst.agent.md:66,75,76,80,168,174` `mcp_workiq_accept_eula`, `mcp_workiq_ask_work_iq` versus `engagement-report-generator.agent.md:9-13` `workiq/accept_eula`, `workiq/ask`. Pattern: Group 2 two names for one tool; Group 3 tool names in prose. Action: rewrite to `server/tool` form (in patch).

**A-H6** `.github/agents/project-planning/system-architecture-reviewer.agent.md:18,147,157` ADR template at `docs/templates/adr-template-solutions.md` and ADRs under `docs/decisions/`, versus `adr-creation.agent.md:20,71` (adr-author skill templates) and `code-review.agent.md:122` (folder holding `.adr-config.yml`). Pattern: Group 2 three files disagree; path absent. Action: rewrite to the adr-author skill and the `.adr-config.yml` rule (in patch).

**A-H7** `.github/agents/project-planning/network-isa95-planner.agent.md` (94 occurrences of MUST / MUST NOT / SHOULD / MAY; intake gate restated at 53, 99-109, 123-128, 172, 227, 304). Pattern: 1a pressure language; Group 2 contradiction with hve-builder line 72. Why: current models follow plain imperatives literally; a prompt where every sentence is MUST produces a rigid, hedging register and over-triggers the gates. The only file of 71 written this way. Action: rewrite in imperative voice, gate stated once (in patch).

**I-H1** `.github/instructions/shared/hve-core-location.instructions.md:20` "their content is referenced via `#file:` from agents and prompts". Pattern: Group 2 contradiction with hve-builder line 58 and `copilot-tracking.instructions.md:16`. Action: rewrite (in patch).

**I-H2** `.github/instructions/experimental/mural/mural-log-hygiene.instructions.md:43-46` four `#file:` bullets. Same pattern. Action: rewrite as Markdown links (in patch).

**I-H3** `.github/instructions/coding-standards/code-review/diff-computation.instructions.md:7` "this file is delivered via the explicit `#file:` import in code-review.agent.md ... must import it with `#file:`". Why: the agent loads it as the `code-review` skill's reference; `#file:` is banned. Action: rewrite; mirror in the skill twin (in patch).

**I-H4** `.github/instructions/project-planning/adr-handoff.instructions.md:255` "If the schema does not yet include `state.handoffs[]`, add it." Pattern: 1d migration-relative; `adr-identity.instructions.md:165-174` already defines it. Action: rewrite (in patch).

**I-H5** `adr-handoff.instructions.md:140` "When an `ADR Planning` section is present in that shared file, use it; otherwise use the generic AI-assistance note". Why: `disclaimer-language.instructions.md:51-54` has that section. Action: rewrite line 140 (in patch); the three embedded generic notes at 180, 227, 293 are flagged because replacing them changes posted text.

**I-H6** `.github/instructions/project-planning/adr-byo-template.instructions.md:78,84` legacy template under `docs/templates/` "remains available as a workspace-level fallback". Why: path absent; hve-builder line 37 says not to preserve obsolete compatibility behavior. Action: remove sentence and row (in patch).

**I-H7** `.github/instructions/docusaurus-edits.instructions.md:126-136` category list naming `Security` and `Templates`, omitting `customization` and `plugins`; lines 52 and 140 name `docs/reference/` and `docs/docusaurus/static/img/`. Why: contradicted by the `docs/` tree. Action: rewrite to defer to `_category_.json` (in patch).

**I-H8** `.github/instructions/hve-core/commit-message.instructions.md:50` "chore: cleaned up and moved code from A to B" under a rule requiring imperative mood. Pattern: 1c gold example contradicting the rule. Action: rewrite (in patch).

**I-H9** `.github/instructions/project-planning/community-interaction.instructions.md:179,190,235` hardcoded "74 days", "21 days", "14 days" where line 66 defines `{{time_period}}`. Pattern: Group 2 volatile specifics. Action: rewrite (in patch).

**I-H10** `.github/workflows/pr-review.md:246-248` "five or more critical findings (security vulnerabilities, empty PR description, no linked issue, and fundamental misalignment...)" lists four categories. Pattern: 1f numeric ceiling, unreachable as written. Action: rewrite (in patch).

**I-H11** `mural-destinations.instructions.md:65` ("`handoffs` frontmatter") and `mural-human-record.instructions.md:20` ("argument-hint"). Pattern: Group 2 contradiction with hve-builder line 51. Action: rewrite; mirror twins (in patch).

**I-H12** `.github/instructions/workflows.instructions.md:411-422` Enforcement Statement list versus Validation Requirements at 296-340: each names scripts the other omits. Pattern: Group 2 duplicated info that drifted. Action: remove the second list (in patch). Note: every script both lists name lives under the absent `scripts/` tree (flag I-L8).

**I-H13** `vex-generation.instructions.md:55` permits "Identifiers, aliases, CVSS, severity, and affected ranges" from the GitHub Advisory DB; `vex-standards.instructions.md:38` permits "Reference URLs and identifiers only." Pattern: Group 2 contradiction on a licensing rule. Action: flag. The user decides which rule stands; the looser passage adds permissions, so no edit is proposed.

**P-H1** `#file:` directives in twelve prompts: `hve-core/git-commit.prompt.md:11`, `git-commit-message.prompt.md:11`, `git-merge.prompt.md:8`, `security/security-capture.prompt.md:10`, `security-plan-from-prd.prompt.md:10`, `sssc-capture.prompt.md:12`, `sssc-from-brd.prompt.md:12`, `sssc-from-prd.prompt.md:12`, `sssc-from-security-plan.prompt.md:12`, `rai-planning/rai-capture.prompt.md:14,16`, `rai-plan-from-prd.prompt.md:14,16`, `rai-plan-from-security-plan.prompt.md:16,18`. Pattern: Group 2 contradiction with hve-builder line 58. Action: rewrite to by-name references (in patch).

**P-H2** `design-thinking/dt-method-next.prompt.md:39,52`, `dt-canonical-deck.prompt.md:40-41,63-64,85,93`, `accessibility/accessibility-coverage-matrix.prompt.md:33,37` workspace-root `.github/skills/...` paths for reads and execution. Pattern: Group 2 contradiction with hve-builder line 57. Action: rewrite to skill-name references (in patch).

**P-H3** `hve-core/git-setup.prompt.md:4-6`, `git-commit.prompt.md:4-6`, `git-commit-message.prompt.md:4-6` `model:` array `MAI-Code-1-Flash (copilot)` / `Claude Haiku 4.5 (copilot)`. Pattern: Group 2 pinned, prior-generation model names; array form that `docs/contributing/ai-artifacts-common.md:113` says older Copilot CLI versions reject. Action: remove (in patch).

**P-H4** `hve-core/git-commit.prompt.md:15-19,33,42` versus `.github/skills/hve-core/git-commit/SKILL.md:43,69,91`. Prompt: "proceed with `git add -u` ... Default to `git add -u` if user does not respond", "Never wait for confirmation for any step". Skill: "Never use an unscoped `git add -A` or `git add -u` path", "Never infer either decision from silence." Pattern: Group 2 contradiction on a safety rule; the prompt also contradicts itself (line 17 asks, line 42 forbids waiting). Action: flag. Recommended resolution is the same thin wrapper used for P-H7; not applied because it touches a confirmation rule.

**P-H5** `git-commit.prompt.md:60-64` example bullets use `*` while line 40 and the instructions file require `-`. Pattern: 1c example contradicting the rule. Action: rewrite (in patch).

**P-H6** `git-commit.prompt.md:36-41` restates commit-message rules with drift ("under 4 specific change points" versus "LESS THAN 100 bytes"). Pattern: Group 2 duplicated info that drifted. Action: remove (in patch).

**P-H7** `hve-core/git-setup.prompt.md:9-215` duplicates `.github/skills/hve-core/git-setup/SKILL.md` at shouting volume ("You WILL", "You MUST never", "## MUST NOT", "Emoji Table MUST include"). Pattern: Group 2 prompt body duplicating a skill; 1a. Action: rewrite to a wrapper (in patch).

**P-H8** `design-thinking/dt-figma-export.prompt.md:37-472` duplicates the `dt-figma-export` skill and its `references/exercise-templates.md`; drifted on MCP config file (`.vscode/mcp.json` versus `.mcp.json`), carries a pricing prediction ("will eventually become a usage-based paid feature"), and lacks the skill's write-confirmation step. Pattern: Group 2 duplicates that drifted; time-sensitive content. Action: rewrite to a wrapper (in patch); the missing write gate is noted for the user.

**P-H9** `data-science-engineering/synth-data-generate.prompt.md:17-355` duplicates the `synth-data-generate` skill and lacks its `SYNTHETIC_DATA_OPERATION_V1` preflight gate and its fairness-labeling rule. Pattern: Group 2 duplicate with drift. Action: rewrite to a wrapper (in patch).

**P-H10** `design-thinking/dt-handoff-implementation-space.prompt.md:79,81` look for `handoff-summary-solution-space.md` and `rpi-handoff-solution-space.md`; the sibling prompt writes `handoff-solution-space.md` and `.copilot-tracking/research/{project-slug}-research-topic.md`; both prompts write that same research path, so the later exit overwrites the earlier. Pattern: Group 2 volatile specifics. Action: rewrite 79 and 81 (in patch); the shared output path is flagged.

**SZ-H1** `.github/skills/session-log-canvas-update/SKILL.md:16` "The canvas it maintains lives at `.github/extensions/hve-core/session-log/`." Why: `.github/extensions/` does not exist; the skill says it belongs to the HVE Core repository only. Action: delete the skill directory (in patch).

**SZ-H2** `.github/skills/rpi/rpi/SKILL.md:188` and copies at `rpi-plan/SKILL.md:26`, `rpi-implement/SKILL.md:22`, `rpi-walkthrough/SKILL.md:26`, `rpi-research/references/research.md:200`, `rpi-review/references/review.md:47`, `rpi-plan-critique/references/lens-assessment.md:26`: "default an optional subagent to `Claude Haiku 5.5 (copilot)` at `max` reasoning effort ... retry it once with `reasoning_effort` set to `medium`." Pattern: Group 2 pinned model name in body prose (the subagent frontmatter already pins it), recency-trap retry rule, repeated seven times. Action: rewrite once, point the copies at the session defaults (in patch).

**SZ-H3** `project-planning/adr-author/SKILL.md:99,136,172` and `requirements-author/SKILL.md:428,456` "MUST be updated", "MUST warn", "MUST load ... MUST append", "MUST NOT change". Pattern: 1a; Group 2 contradiction with hve-builder line 72. Action: lowercase (in patch).

**SZ-H4** `project-planning/functional-planner/SKILL.md:46,115` `mcp_github_list_issue_types`, `mcp_github_get_label` versus `backlog-management/SKILL.md:64,75` `github/*`. Pattern: 1d retired tool-name form; Group 2. Action: rewrite (in patch).

**SZ-H5** `project-planning/outcome-hypothesis/SKILL.md:51` hard-coded `../../shared/disclaimer-language/SKILL.md` path. Pattern: Group 2 contradiction with hve-builder line 56. Action: rewrite to a by-name reference (in patch).

**SZ-H6** `root-cause-analysis/root-cause-analysis/SKILL.md:46-47` "switch to the debugging model `gpt-5.6-luna` when ... cheaper". Pattern: Group 2 pinned model and pricing claim. Action: rewrite (in patch).

**SA-H1** `hve-core/vally-tests/SKILL.md:130` agent target `evals/agent-behavior/eval.yaml` versus `references/eval-suite-routing.md:16` `evals/agent-behavior/stimuli/<slug>.yml`. Pattern: Group 2 table contradicts the reference it says it mirrors. Action: rewrite (in patch).

**SA-H2** `experimental/mural/SKILL.md:417-445` intro says the table is "not exposed by the current CLI" while 13 rows say "Shipped as ..."; four rows name subcommands the parser does not define (`widget create-image`, `mural find`, `workspace summary`, `widget tag merge`). Pattern: Group 2 shipping log contradicted by `scripts/mural/_parser.py` and by the drift-guarded commands table at 252-344. Action: rewrite (in patch).

**SA-H3** `experimental/customer-card-render/SKILL.md:130-138` "split across 3 opinionated slides" then "across 4 slides"; tree at 56-58 lists three files; `templates/` has `use-case-slide4.content.yaml` and lines 72-86 and `references/mapping-spec.md` implement four. Pattern: Group 2 stale fact. Action: remove the stale block, add the fourth file to the tree (in patch).

**SA-H4** `data-science-engineering/dataops/SKILL.md:63-66` "No repository artifact currently owns the last two rows" versus `data-catalog/SKILL.md:52`, `feasibility/SKILL.md`. Pattern: Group 2 contradiction. Action: rewrite owners (in patch).

**SA-H5** `copilot-agent-store-publish/SKILL.md:4-18` top-level `authors`, `ms.date`, `keywords` frontmatter, versus `hve-artifact-authoring/references/frontmatter-schemas.md:59-70`. Pattern: Group 2 contradiction with the skill frontmatter contract; `keywords` is trigger enumeration outside `description`. Action: rewrite frontmatter (in patch).

**SA-H6** `engagement-reporting/engagement-reporting/SKILL.md:139-140` "do not narrate internal linting, section scans, or fallback checks". Pattern: 1d update suppressor (documented Claude Fable 5.1 behavior). Action: rewrite (in patch).

**SA-H7** `references/slide-content.md` is named as a file to read at `experimental/hve-demo-material/SKILL.md:25,265`, `experimental/hve-demo-material.agent.md:113,256`, `experimental/customer-card-render/SKILL.md:112`, and `.github/workflows/demo-material-author.md:213`, but does not exist anywhere in the repository (the skill's references are character-animation, curriculum, house-style, output-contract). Pattern: Group 2 volatile specific. Action: rewrite to the file that documents the YAML schema, or to the `hve-slides` skill (in patch; see the diff-author notes for what was found).

### 4.2 Medium confidence

Agents (`.github/agents/...`):

| ID | Location | Evidence (short) | Pattern / why | Action |
|---|---|---|---|---|
| A-M1 | `design-thinking/dt-coach.agent.md:321-337`, `:245`, `:276`, `:12`, `:45` | "## Canonical Deck ... (MANDATORY)", "you MUST load", "Non-Negotiable Protocol", three "Never ..." | 1a, 1d patch accretion: lines 275-276 already say the same at normal volume | remove block, fold the one new rule into 275 (patch) |
| A-M2 | `dt-coach.agent.md:19` | "2-3 sentences, not walls of text" | 1b/1f numeric cap | rewrite qualitative (patch) |
| A-M3 | `dt-coach.agent.md:179` | "**Remember**: Hats should always..." | 1c repetition of 183 | remove (patch) |
| A-M4 | `dt-coach.agent.md:100,223` | same path given as "state directory" and "artifacts directory"; "which remains in" | 1d migration-relative | rewrite (patch) |
| A-M5 | `coding-standards/code-review.agent.md:94-96`; "Do not re-read the diff for any reason." in six subagents | "Do not re-read files partially ... issue verification reads", three enumerated exceptions | 1d tool-use discouragement; patch accretion | rewrite with the reason (context budget) and one principle (patch) |
| A-M6 | `security/subagents/supply-chain-skill-assessor.agent.md` vs `skill-assessor.agent.md` | same tools, enums, formats; delta is one index filename | Group 4 redundant subagent | delete, fold index rule into skill-assessor, repoint sssc-reviewer (patch) |
| A-M7 | `project-planning/subagents/brd-quality-reviewer.agent.md` vs `prd-quality-reviewer.agent.md`; both `:33` "Per the consolidated design..." | 103-line bodies differing only by document kind | Group 4; Group 2 history narrative | merge into `Requirements Quality Reviewer` with `document_kind` input; update two dispatch sites (patch) |
| A-M8 | `coding-standards/subagents/code-review-{functional,security,accessibility}.agent.md` | identical contract, differ in Lane Preset; orchestrator already passes `perspective` | Group 4 | merge into `Code Review Perspective`; update orchestrator table (patch) |
| A-M9 | `security/subagents/finding-deep-verifier.agent.md:46-77,176-177,190-191` | "Evidence Search Strategy" Phases 1-4 restate Required Steps; two NOTEs say the same | 1c repetition | remove the duplicate procedure and one NOTE (patch) |
| A-M10 | `security/sssc-reviewer.agent.md:25,40,132,192` | "does not delegate to Report Generator" four times | 1c | remove 40 and 192 (patch) |
| A-M11 | `project-planning/backlog-manager.agent.md:52,59,80,86-87,165` | read-only boundary stated five times | 1c | remove 165, trim 80 (patch) |
| A-M12 | `project-planning/prd-builder.agent.md:230-235,280-288,465-475,493-501,577-599,601-630` | pseudo-code "algorithms", duplicate recovery example, generic Best Practices | 1c choreography and padding | remove; keep the one unique rule (patch) |
| A-M13 | `prd-builder.agent.md:477-486` | "Tool Selection Guidelines" naming tools in prose | Group 3 | remove (patch) |
| A-M14 | `prd-builder.agent.md:189` | "State written before this contract may carry `schemaVersion` instead of `kind`" | 1d compatibility shim; hve-builder line 37 | flag (may protect real state files) |
| A-M15 | `accessibility/accessibility-planner.agent.md:196` | "Never edit the `disclaimer-language` skill to add an accessibility variant" (the variant exists) | Group 2 recency trap; prohibition | flag |
| A-M16 | `accessibility-planner.agent.md:18,24,48,115,196` | "(L7 disclaimer lever)", "(L1 identity-inheritance lever...)" | Group 2 design-doc labels | remove parentheticals (patch) |
| A-M17 | `agentic-workflows.agent.md:35-48,164-173`, eight "Use cases" lists | README prose and 24 example queries in a dispatcher | Group 2 trigger enumeration; verbose | remove (patch) |
| A-M18 | `experimental/subagents/pptx-subagent.agent.md:116` | "(default: `claude-haiku-4.5`)" | Group 2 pinned prior-generation model | rewrite (patch) |
| A-M19 | `experimental/pptx.agent.md:113,17` | bold "must always run ... Even when slides appear correct"; "**Important**:" | 1a | rewrite (patch) |
| A-M20 | `system-architecture-reviewer.agent.md:120-139` | database and deployment selection heuristics | Group 2 general knowledge; strategy coaching | remove (patch) |
| A-M21 | `experimental/hve-demo-material.agent.md:305-308` | "one measured level ran 361 seconds without the option and 284 seconds with it" | Group 2 history narrative | rewrite (patch) |
| A-M22 | `engagement-report-generator.agent.md:55-56` | "Do not run Markdown linters, shell searches, or repository diagnostics" | 1d tool-use discouragement; agent has no `execute` tool | remove (patch) |
| A-M23 | `network-isa95-planner.agent.md:314-320` | six fixed flow IDs pinned into the diagram rule | 1c gold output | rewrite as illustrative (patch, inside A-H7) |
| A-M24 | `security/subagents/report-generator.agent.md:153-154` (GPT-pinned) | "for compatibility with prior report structure" | Group 2 history narrative | rewrite (patch) |
| A-M25 | `security/security-reviewer.agent.md:279` | "identical to the original workflow" | 1d migration-relative | rewrite (patch) |
| A-S3 | `dependency-reviewer.agent.md:30,33,37,51` | `.devcontainer/`, `scripts/security/` | Group 2 absent paths | rewrite (patch) |

Instructions and workflow prompts (`.github/instructions/...`, `.github/workflows/*.md`):

| ID | Location | Evidence (short) | Pattern / why | Action |
|---|---|---|---|---|
| I-M1 | `shared/planner-identity-base.instructions.md:105,113` and echoes in accessibility, privacy, security, sssc (`:99,113,279,315`), rai (`:117,123,277`) identity files; twin `skills/shared/planner-identity/SKILL.md` | "Ask 3-5 questions per turn", "Never ask more than 5 questions", "batches of 5-7", "up to 7" | 1b/1f numeric clamps; hve-builder line 72 | rewrite base and the two non-inheriting overrides; mirror twin (patch) |
| I-M2 | `project-planning/adr-identity.instructions.md:35-40,16,91-92`; twin in adr-author references | "two to three sentences", "No bullet lists unless...", "Never surface step labels" | 1f sentence caps; 1d anti-formatting; fossil for a model that leaked labels | rewrite (patch) |
| I-M3 | `adr-identity.instructions.md:228-230` | "MUST `read_file` ..." three times | 1a | lowercase (patch) |
| I-M4 | `adr-byo-template:22,88,100,127`, `adr-standards:167,191`, `adr-identity:124` | "(GP-13)", "(GP-08)"... | Group 2 plan IDs undefined in repo | remove (patch) |
| I-M5 | `privacy/privacy-identity.instructions.md:72,215`; twin | "PRD phase mapping ... (FR-003, DD-003)"; "the fifth `backlog-templates` caller" | Group 2 history narrative | remove / rewrite (patch) |
| I-M6 | `hve-core/commit-message.instructions.md:12,16,45,59-60,64` | "MUST be in imperative mood", "LESS THAN 100 bytes", "(0-5 items AT MOST)" | 1a; byte limits stay | lowercase; drop the item cap (patch) |
| I-M7 | `workflows/dependency-pr-review.md:101,108`, `doc-update-check.md:123,128`, `issue-implement.md:70,72`, `issue-triage.md:126,132`, `vex-draft.md:144,149` | "**You MUST call `noop` and stop immediately...**" + "**Failure to call `noop` ... will cause workflow failure.**" | 1a; the second sentence is the load-bearing reason | rewrite into one plain line with the reason; conditions unchanged (patch) |
| I-M8 | `workflows.instructions.md:162` | "MUST follow these structural expectations" over "Prefer..." bullets | 1a, no enforcing check | rewrite (patch) |
| I-M9 | `hve-core/markdown.instructions.md:273-285,51-52,140,253,278` | Miscellaneous bullets restate eight rules; "Try to always use `*`" | 1c repetition; 1a hedge on a requirement | remove duplicates, rewrite 140, fix two stray indents (patch) |
| I-M10 | `markdown.instructions.md:60` vs three instruction files without `applyTo` | table says instruction files require `description`, `applyTo` | Group 2; schema absent so history cannot order | flag |
| I-M11 | `hve-core/writing-style.instructions.md:66,75-89` (`applyTo: **/*.md`) vs em dashes in 23 instruction files and bolded-prefix lists in 8 | the rule file and the files it governs disagree | Group 2 | flag: narrow `applyTo` to `docs/**`, or bring the shared planner files into line |
| I-M12 | `ci-owned-validation.instructions.md:27` | "Do not restore removed `eval:*`, `lint:all`, ... compatibility aliases" | 1d migration-relative | rewrite (patch) |
| I-M13 | `adr-handoff.instructions.md:93,62,169,214` | "When in doubt, fire the handoff"; "{no more than two sentences}" | 1a default-to steering; 1f caps | remove / rewrite (patch) |
| I-M14 | `community-interaction.instructions.md:29,37-49` | "target 2-4 sentences per response"; Response Length column | 1f | rewrite; drop column (patch) |
| I-M15 | `engagement-reporting/style-guide.instructions.md:33-34` | "AI patterns such as 'I'm happy to help'..." | 1e no-provenance tic list | rewrite positively (patch) |
| I-M16 | `coding-standards/uv-projects.instructions.md:8,41,85-95,123,31-39,43-68` | "You are a Python environment specialist..."; commands listed three times; mandatory `ipykernel`, CUDA branch for every `.py` | 1d identity stub; 1c; Group 2 wrong degrees of freedom | remove persona and duplicates (patch); notebook/ML defaults flagged |
| I-M17 | `coding-standards/python-tests.instructions.md:50-143,190` | four "Before — unittest.mock / After — pytest-mock" pairs | 1d migration content; general knowledge | keep the After halves (patch) |
| I-M18 | `coding-standards/rust/rust.instructions.md:517-526`, `rust-tests.instructions.md:198-203` | "Patterns to Avoid" and "Test Conventions" recaps duplicating earlier sections | 1c | remove; relocate two unique items (patch) |
| I-M19 | `bash.instructions.md:44-48,157-167,333-345`, `python-script.instructions.md:95-112,114-140,142-174`, `csharp.instructions.md:70-83`, `terraform.instructions.md:131-167`, `powershell.instructions.md:168-174`, `pester.instructions.md:230-248` | `-e`/`-u`/`-o pipefail` explained; pathlib method list; `coalesce()` tutorial; `Should -*` table | Group 2 general knowledge | reduce each to its convention line (patch) |
| I-M20 | `bash:64-65,74-75`, `powershell:15-16,267-268`, `pester:42-43,276-277`, `python-script:239` | copyright header shown with and without "2026 ... All rights reserved." | Group 2 example/rule mismatch; year rots | unify on the year-less form (patch; confirm against the absent validation script) |
| I-M21 | `csharp.instructions.md:94` vs `csharp-tests.instructions.md:8` | field naming row shows `_logger` and `isActive`; tests file cites an underscore rule the main file lacks | Group 2 | split the row (patch; protected-field case is an assumption) |
| I-M22 | `csharp-tests.instructions.md:26-30` | format `GivenContext_WhenAction_ExpectedResult` with examples that lack both parts | 1c example over-indexing | rewrite (patch) |
| I-M23 | `workflows/issue-implement.md:81-100` | "1. Read ... 2. Search ... 3. Outline ... 4. Make ... 5. Verify ... 6. Create a PR" | 1b plan-first choreography | rewrite as outcomes and contract (patch) |
| I-M24 | `workflows/issue-triage.md:134-139` | "Only proceed with triage when..." restating the activation guard | 1c | remove (patch) |
| I-M25 | `mural-destinations.instructions.md:68-70,23,36` | "v1 retro coverage"; "(Decision D5)", "(Decision D4 + Pattern I)" | Group 2 roadmap and design IDs | remove / rewrite (patch) |
| I-S3 | `coding-standards/powershell/powershell.instructions.md:202-218` | "CI Integration" section documenting `scripts/lib/Modules/CIHelpers.psm1` | Group 2 absent module | remove (patch) |

Prompts (`.github/prompts/...`):

| ID | Location | Evidence (short) | Pattern / why | Action |
|---|---|---|---|---|
| P-M1 | `agent: dt-coach` in eight dt-method prompts (line 3); `agent: security-planner` in `security-plan-from-prd:3` | agent `name` values are `DT Coach`, `Security Planner`; 31 of 33 references use the display name | Group 2 inconsistency | rewrite (patch) |
| P-M2 | `hve-core/ado-get-build-info.prompt.md:8` | "**MANDATORY**: Activate the skill" | 1a | drop the marker (patch) |
| P-M3 | `git-setup.prompt.md` register and repetition | superseded by P-H7 | | skipped (file becomes a wrapper) |
| P-M4 | six security prompts (`:12`/`:14`) | "Display both the disclaimer and the attribution before any questions or analysis." after a sentence ending the same way | 1c | remove duplicate (patch) |
| P-M5 | four sssc prompts `:18` | "...; do not restate those tables in this prompt." | 1d note to the author left in runtime text | rewrite (patch) |
| P-M6 | `security-capture:10`, `security-plan-from-prd:10` vs `sssc-*:12` | disclaimer "every new conversation" vs "every new project" | Group 2 | flag |
| P-M7 | `security/risk-register.prompt.md:16,89,112-114` | "clear, simple, professional language" four times | 1c generic virtues | remove repeats (patch) |
| P-M8 | `security/incident-response.prompt.md:70-78,159-167` | query-development steps; Five Whys script | Group 2 general knowledge | remove / condense (patch) |
| P-M9, P-M10, P-M11 | `synth-data-generate.prompt.md`, `dt-figma-export.prompt.md` | update suppressors, plan-first, inline AUC rubric, shouting | fallbacks only if P-H8/P-H9 are declined | skipped (bodies become wrappers) |
| P-M12 | `dt-canonical-deck.prompt.md:59-65,84-97` | "verify the actual command parameters by reading the skill documentation ... If skill interfaces have changed, update commands"; PowerShell branch in a bash fence with `\` continuations | Group 2 command inventory copied from skills | remove; reference the skills (patch) |
| P-M13 | `dt-method-next.prompt.md:63` | "3+ times in the last 6 transition_log entries ... 'I notice you've returned to Method [N]...'" | 1c scripted utterance; invented threshold | rewrite (patch) |
| P-M14 | `accessibility-coverage-matrix.prompt.md:45-51` | "that separation is intended rather than unfinished..." | Group 2 design narrative | condense (patch) |
| P-M15 | `experimental/graph-research.prompt.md:10,25,104,112` | no-build rule stated four times | 1c | remove one, trim one (patch) |

Skills (`.github/skills/...`):

| ID | Location | Evidence (short) | Pattern / why | Action |
|---|---|---|---|---|
| SZ-M1 | `rpi/rpi/SKILL.md:31`, `rpi-challenger:53`, `rpi-quick:64`, `rpi-walkthrough:51`, `rpi-research:110`, `rpi-plan:86`, `rpi-plan-critique:63` | "Do not narrate low-level actions."; "Suppress low-level actions, routine tool calls..."; "suppress continual updates" | 1d update suppressors; six are paired with a good positive clause | remove the suppressor limb; rewrite the bare one (patch) |
| SZ-M2 | `rpi-walkthrough/SKILL.md:70`, `references/walkthrough.md:84-93` | "avoid em dashes, filler, ... 'delve', 'tapestry' ... excessive bolding" | 1e tic list; 1d anti-formatting | rewrite positively; keep "Write it like this instead" (patch) |
| SZ-M3 | `rpi-walkthrough/SKILL.md:28,52,61,72` | one-or-two-question cadence stated four times | 1c | keep 72 only (patch) |
| SZ-M4 | `project-planning/proposal-response/SKILL.md:20,129-131,239-242` | exact recovery procedure for a dot-prefixed relative-path read failure, repeated | Group 2 recency trap; 1c | rewrite once (patch) |
| SZ-M5 | `project-planning/outcome-hypothesis/SKILL.md:51,148,159,178,182 / 64,138,156,171,184` | two gates each restated five times | 1c | keep two statements each (patch) |
| SZ-M6 | `project-planning/privacy-by-design/SKILL.md:150` | "dedicated agent wiring is outside this skill change" | Group 2 PR narrative | rewrite (patch) |
| SZ-M7 | `security/vex/SKILL.md:40-42` | "The forthcoming tested gate module and tests will live in this skill" | Group 2 roadmap | remove (patch) |
| SZ-M8 | `project-planning/adr-author/SKILL.md:82,130` | "(GP-17)", "GP-13 (...)" | Group 2 plan IDs | remove (patch) |
| SZ-M9 | `security/risk-register/SKILL.md:19,118,139-141` | "clear, concise, simple" four times | 1c | keep one (patch) |
| SZ-M10 | `security/incident-response/SKILL.md:154-169` | "Be blameless..."; numbered Five Whys | Group 2 general knowledge | condense (patch) |
| SZ-M11 | `project-planning/gitlab/SKILL.md:22,42,89-91` | "This first hve-core implementation is Python-only"; "until a protected OAuth backend is available" | Group 2 roadmap phrasing | rewrite (patch) |
| SA-M1 | `hve-core/prompt-analyze:38`, `prompt-refactor:37`, `prompt-builder:37` | "Do not dispatch retired named lifecycle workers." | 1d phantom alternative | remove (patch) |
| SA-M2 | `experimental/powerpoint/SKILL.md:159,219,229-232` | "This is a behavior change..."; "microsoft/hve-core#1020" | 1d; Group 2 issue archaeology | remove (patch) |
| SA-M3 | `experimental/hve-demo-material/SKILL.md:138-142` | 361-second versus 284-second anecdote | Group 2 history narrative | rewrite (patch) |
| SA-M4 | `accessibility/accessibility/SKILL.md:78-85,301,364-372,392-394,444` | maintainer design notes, test-harness internals, `docs/docusaurus` on `127.0.0.1:3001`, "**now** carries" | Group 2 verbose; 1d | move notes to a reference; rewrite two lines (patch) |
| SA-M5 | `hve-core/vally-tests/SKILL.md:87-92`; `references/grader-catalog.md:107` | "Delivery: Phase 2 / Phase 3" column; `model: gpt-4o-mini` | Group 2 project phases; pinned retired model | remove column; placeholder model (patch) |
| SA-M6 | `data-science-engineering/synth-data-generate/SKILL.md:70,208-212` | five "no code in chat" items; "without pausing or triggering continuation requests" | 1c; 1a do-not-stop booster | one item (patch) |
| SA-M7 | `engagement-reporting/engagement-report-conventions/SKILL.md:43-44` | "AI patterns such as 'I'm happy to help'..." | 1e | remove (patch) |
| SA-M8 | `hve-core/architecture-diagrams/SKILL.md:209-213` | ASCII gold example with misaligned borders | 1c example the model will copy | re-align (patch) |
| SA-M9 | `design-thinking/demo-setup/SKILL.md:150` | "does not natively recognize yet ... could be formalized ... if a future use case requires it" | Group 2 roadmap | rewrite (patch) |
| SA-M10 | `design-thinking/dt-figma-export/SKILL.md:77,81-88` | "up to six tool calls per month", "Tier 1" | Group 2 vendor numbers | rewrite (patch) |
| SA-M11 | `experimental/mural/SKILL.md:67-68,242-246,447-456` | "no longer permits"; Python internals; "Planning log WI-01", "PD-7.1" | 1d; Group 2 | rewrite / move / remove (patch) |
| SA-M12 | `experimental/video-to-gif/SKILL.md:30-64,299-308`, `experimental/demo-video/SKILL.md:125-145` | per-platform FFmpeg install walkthroughs | Group 2 general knowledge | one line (patch) |
| SA-M13 | `engagement-report-council-critique/SKILL.md:30` | "the legacy `critic-run-id` input" (still the live name elsewhere) | 1d | rewrite (patch) |
| SA-M14 | `experimental/tts-voiceover/SKILL.md:24,252` | "Free tier provides 500K characters per month"; "the updated `embed_audio.py`" | Group 2 volatile; 1d | rewrite (patch) |
| SA-M15 | `coding-standards/python-foundational/SKILL.md:18` | "This content is a skill rather than an instructions file for three reasons" | Group 2 narrative | remove (patch) |
| SA-M16 | `design-thinking/ux-artifacts/SKILL.md:97`; `hve-slides/SKILL.md:110` | "It replaces, rather than renames..."; "does not require Impeccable, Monaco, PowerPoint..." | 1d phantom alternatives | remove (patch) |

### 4.3 Low confidence (flag only, not in the patch)

Agents: pinned `model:` on nine subagents, two with an in-body rationale (A-L1); maintainer notes such as "do not restate it here" inside prompts (A-L2); agents referenced by slug rather than `name` in six places (A-L3); `vally-test-author.agent.md` depends on an absent `evals/` tree (A-L4); `ms.date`, `keywords`, `estimated_session_time` frontmatter on the two marketplace coaches (A-L5); "approximately 30 queries before throttling" and "3-5 sentence" summaries in meeting-analyst and report-generator (A-L6); "Limit to 10 inline comments" in dependency-reviewer (A-L7); scattered repetition in eight files (A-L8); `v0.86.2` pinned eleven times in agentic-workflows (A-L9); residual caps in otherwise clean files (A-L10); three different signing invocations for the same script (A-L11); two sources of truth named in sssc-planner (A-L12); the two framework assessors share most of their bodies with skill-assessor (A-L13); accessibility-planner declares about 150 inline MCP tool ids (A-L14); interview cadences and response-format rules in functional-planner and brd-builder (A-L15); prd-builder's backlog handoff omits GitHub (A-L16); subagent lists by `.github/agents/**` glob (A-L17).

Instructions: Think/Speak/Empower "This reasoning stays internal" scaffold (I-L1); undefined design labels "Sequential Model A", "Pattern J", "Layer B", "Slot 2" (I-L2); ADR identity's authority sentence narrower than the shared untrusted-content rule (I-L3, safety rule); scattered caps with adjacent reasons (I-L4); diff-size thresholds 20/50/2000/500 (I-L5); VEX maturity promotion criteria in an instruction file (I-L6); Moq version pin encoding the 2023 incident (I-L7); the `scripts/` tree references in sssc-planner's signing contract, workflows.instructions' enforcement list, and powershell CI section (I-L8; the last is in the patch as I-S3); dangling "workshop document" reference and a duplicated bullet (I-L9); SLSA version stated two ways (I-L10); generic virtues in three workflow prompts (I-L11).

Prompts: allowed-git-command list restated four times in git-commit (P-L1); "≥80% reduction" target in cspell-config (P-L2); numeric question counts in capture prompts (P-L3); `tools` lists narrower than the steps in two dt-handoff prompts (P-L4); a three-space-indented YAML example (P-L5); evals-import misattributes the column contract (P-L6).

Skills: thoroughness boosters in rpi-research and root-cause-analysis (SZ-L1); "3-5 questions per turn" in planner-identity and batch sizes in backlog-templates (SZ-L2); installer status-line emoji choreography and a duplicated phase name (SZ-L3); `schemaVersion` compatibility path in requirements-author (SZ-L4); `brd-author#discover` constants under a renamed skill (SZ-L5); "the most common failure" coaching in backlog-plan (SZ-L6); `-OutputFormat Json` stated three times in gh-code-scanning (SZ-L7); root-cause-analysis at 760 lines (SZ-L8); two "canonical" autonomy vocabularies (SZ-L9); OTel general knowledge and a semantic-conventions version pin (SZ-L10); dated caveats in security-planning and rai-standards (SZ-L11); dated provenance line in the hve-builder requirements catalog (SA-L1); agreeing repetition in hve-demo-material (SA-L2); recency-trap tool prohibitions in engagement-reporting (SA-L3); a retraction anecdote in copilot-otel-metrics (SA-L4); buzzword list and RFC caps in powerpoint (SA-L5); Contributing and test sections inside runtime skills (SA-L6); roadmap sentence in feasibility (SA-L7); 80-percent target in cspell-config skill (SA-L8); "no `~/.mural.env` legacy fallback" phantom (SA-L9); generic virtues and a near-duplicate list in synth-data-generate (SA-L10); agreeing duplicate between the two accessibility skills (SA-L11).

### 4.4 Not flagged (keep list)

The `workflows.instructions.md` MUST rules for SHA pinning, permissions, credentials, runners, and the PR gate (compliance rules with reasons or enforcing checks); the installer's 663-line prescriptive flow (fragile operation); the `caveman` skill's terse style (product intent); the narration word budget in hve-demo-material (a media-duration contract); the positive "announce the current workflow stage" lines in architecture-diagrams and the RPI skills; byte-identical instruction and skill twins (working redundancy); the three backlog executors (genuinely different binding protocols); `[!CAUTION]` disclaimer blocks (compliance language aimed at the reader).

## 5. Proposed diff (Step 6)

`proposed.patch` in this directory is a `diff -ruN` between an untouched copy of `.github/` and a copy with the High and Medium actions above applied. Apply from the repository root with `patch -p1 --dry-run < .copilot-tracking/reviews/2026-11-23-prompt-audit/proposed.patch` first, then without `--dry-run`. One finding per hunk where the file layout allowed it; whole-file rewrites (R-1, A-H7, P-H7, P-H8, P-H9, the three merges) are single hunks by nature.

Excluded from the patch: every `flag` and Low item; P-H4 (git-commit staging rule; safety); I-H13 (VEX licensing); I-H5's three embedded disclaimer templates; I-M10 and I-M11 (schema and style-rule direction); A-M14 and A-M15; R-3 (manifest membership, which depends on which deletions and merges are accepted).

Removal completeness: deleting `code-review-pr`, `supply-chain-skill-assessor`, the two quality reviewers, the three code-review lanes, and the `session-log-canvas-update` skill leaves entries in `.github/plugin.json` (R-3) and may leave rows in `docs/agents/*`; grep for the old `name` values after applying.

## 6. Verification (Step 7)

Stale-fact and conflict findings were checked against the repository (path existence by file tools, parser subcommands by reading `_parser.py`, templates by listing directories). No behavioral probe was run: the repository has no eval harness in this copy (`evals/` and the `vally` scripts are absent). Before relying on the Group 1 removals, run the affected agents on a representative session each: the network ISA-95 planner (A-H7), the DT Coach (A-M1), the RPI stage-boundary updates (SZ-M1), and one workflow `noop` guard in a dry run (I-M7). If any cut regresses, re-add the instruction in its minimal form rather than restoring the original.
