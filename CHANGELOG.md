# Changelog

All notable changes to this skill are documented here.

---

## [2.2.0] - 2026-07-06

### Changed
- **NEXT STEP is now a three-line pointer, not a self-contained briefing.** It names the project and unit of work, states the next action, and defers to the handoff as authoritative. The old form duplicated TASK STATE, a locked decision, and a gotcha into the pointer; that redundancy was re-paid on every turn of the new conversation. The handoff travels with the pointer, so the pointer no longer needs to survive alone.
- **Length budget replaces token budget.** Line counts are self-checkable during generation; token counts are not. Target is roughly 40 content lines, hard ceiling 80. The old 3,000/5,000 token figures survive in the README as rough equivalents for intuition.
- **Empty blocks are omitted, with a ledger line.** Instead of printing `None this session.` under empty headers, empty blocks are dropped and named in a single first line: `Blocks empty this session: <names>`. Deliberate omission stays distinguishable from oversight at lower carried cost.
- **CURRENT HEAD infra fields genericized.** `Supabase:` and `Vercel:` labels are now `Backend service:` and `Deploy target:` with Supabase, Vercel, and peers as examples, so the skill is not hardcoded to one stack.
- **Claude Code command filenames now carry the `code-` tool prefix** (`code-YYYY-MM-DD-HH-MM-context-summary.md`), matching the naming convention used across companion tooling in the same `_sessions/` directory.

### Fixed
- **Standalone violation in the Claude Code command.** The ARCHIVE POINTER fallback text referenced an external slash command; it now matches the skill's self-contained wording.
- **Nested code-fence bug in the SKILL.md worked example.** The example's outer fence now uses four backticks so the NEXT STEP inner fence renders correctly on GitHub.

---

## [2.1.0] - 2026-05-31

### Changed
- **Standalone skill.** Removed all references to other skills. `context-summary` now runs entirely on its own with no companion-skill dependencies. The ARCHIVE POINTER block no longer instructs the user to run any external skill; it either points to the user's own log or states that the handoff is the only record.
- **Trigger description.** Removed cross-skill disambiguation. The description now defines the skill purely by its own intent signal: carrying work forward into a new conversation versus producing a full archive.
- **Worked example.** Replaced the example project with a generic multi-tenant SaaS API build so the skill is not tied to any specific product. The example now fully conforms to the v2 output spec (3-5 sentence TASK STATE, five gotchas each with symptom and guard, OPEN QUESTIONS with current assumptions, NEXT STEP with the key locked decision and gotcha embedded inline).

### Fixed
- **Output-is-markdown-only guardrail.** Added an explicit guardrail with a binary test: the first character of the response must be the first character of the first block header. Prevents preamble or meta-commentary from leaking into the handoff.
- **Non-git session guidance.** CURRENT HEAD now explicitly handles writing, research, and planning sessions that have no git repo, by substituting equivalent artifact state in the same one-fact-per-line format.
- **Short-session handling.** Behavior on Invocation now states that a lean handoff for a short session (fewer than ~10 turns) is correct behavior, not a failure, and instructs against padding to meet token targets.
- **Other infra field.** Clarified that the `Other infra:` line is omit-when-empty and must not be written as `Other infra: None`.

---

## [2.0.0] - 2026-05-31

### Changed
- **Token budget:** Target raised from 1,500 to 3,000 tokens. Hard ceiling raised from 3,000 to 5,000 tokens.
- **TASK STATE:** Expanded from 2-3 sentences to 3-5. Now requires prior step completed, next step queued, and project goal or deadline if applicable.
- **CURRENT HEAD:** Added explicit fields for `Other infra` (DNS, external APIs, feature flags). Added requirement to note non-standard state with a one-sentence explanation. Expanded description of dirty working tree to include why it is uncommitted.
- **DECISIONS LOCKED:** Added `Data:` and `Security:` as named domain groups alongside existing Product, Architecture, Tooling. Now requires rationale clause on fragile decisions. Added completeness expectation: a decision omitted is a decision at risk.
- **ACTIVE GOTCHAS:** Expanded cap from 5-8 lines to 5-12. Each gotcha now requires the symptom it produces if ignored, not just the guard. Added dependency and integration constraints as a priority tier.
- **OPEN QUESTIONS:** Each item now requires the current known assumption alongside the question and resolve-by marker. Grouping by domain required when more than five items.
- **ARCHIVE POINTER:** Expanded from one line to one to three lines. Secondary lines for design docs, ADRs, and spec files produced or referenced during the session.
- **NEXT STEP:** Now requires the most important locked decision and most important active gotcha embedded inline, so the prompt works even if the handoff is not attached.
- **Guardrails:** Added explicit token range guidance (3,000-5,000 is acceptable, above 5,000 means archive leak). Added DECISIONS LOCKED completeness rule. Added symptom requirement to ACTIVE GOTCHAS guardrail. Added self-contained requirement to NEXT STEP guardrail.

---

## [1.0.0] - 2026-05-15

### Added
- Initial release.
- Seven-block fixed skeleton: TASK STATE, CURRENT HEAD, DECISIONS LOCKED, ACTIVE GOTCHAS, OPEN QUESTIONS, ARCHIVE POINTER, NEXT STEP.
- Trigger phrase set: context summary, context-summary, context handoff, handoff summary, give me the handoff, hand this off, carry context forward, prep the handoff.
- Live git state detection when repo is in context.
- Token budget: 1,500 target, 3,000 hard ceiling.
