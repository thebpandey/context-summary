# Changelog

All notable changes to this skill are documented here.

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
- Distinction from session-summary and kodetrain-session skills.
- Token budget: 1,500 target, 3,000 hard ceiling.
