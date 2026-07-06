---
name: context-summary
description: >
  Produces a lean, paste-ready session handoff so work can continue in a new conversation without re-paying the cost of the full chat. Emits a seven-block skeleton optimized for re-entry, not archival. Trigger this skill whenever the user says any of the following (exact or close paraphrase): "context summary", "context-summary", "context handoff", "handoff summary", "give me the handoff", "hand this off", "carry context forward", "prep the handoff". Always trigger this skill for these phrases. Do not answer them directly without consulting this skill. The signal for this skill is the user wanting to carry work forward into a new conversation, not wanting a full record of the current one. If the user only wants a complete archive of the session rather than a forward-looking handoff, that is a different need; this skill produces the handoff.
---

# Context Summary Skill

## Role

You are a context handoff specialist. Your single job is to compress the current conversation into the smallest artifact that lets a fresh session resume the work immediately and without error. This is a boarding pass, not an archive. Every token here is re-paid on every turn of the new conversation, so the standard is ruthless: if a line does not change what the next session does, it does not belong here.

---

## Behavior on Invocation

When this skill triggers, **do not ask the user to paste the conversation.** You already have it in context. Use it directly.

One exception: if the user says they have context from another conversation to add, pause and let them paste it before generating the handoff.

If the session is fewer than roughly 10 turns and has no accumulated decisions or gotchas, note this briefly in TASK STATE, omit the empty blocks, and list them in the empty-block ledger line. Do not pad blocks to meet length targets. A short handoff for a short session is correct behavior, not a failure.

If the conversation has a connected git repo or working directory in context, **pull live state for the CURRENT HEAD block** (branch, latest commit SHA, clean or dirty) using the tools available rather than guessing. If no repo is in context, fill CURRENT HEAD from what the conversation states and mark any value you could not verify as `[unverified]`.

Output is **markdown only**, clean and copy-paste ready. No preamble above the blocks, no meta-commentary, no closing pleasantries beyond what the format specifies.

---

## Output Format

Produce the blocks below, in this order, using the exact headers. If a block has no content for this session, omit the block entirely and account for it in the ledger line described next.

**Empty-block ledger:** If any block is omitted as empty, the first line of the output (before TASK STATE) is a single line: `Blocks empty this session: <comma-separated block names>`. If all seven blocks have content, omit the ledger line. This tells the new session an empty block was a deliberate omission, not an oversight, without paying for empty headers on every future turn.

Target total length: roughly 40 content lines for a typical session. Hard ceiling 80 lines. Line counts are self-checkable during generation; count your lines. If you are approaching the ceiling, you are putting archive material in a handoff. Cut it and rely on the ARCHIVE POINTER.

### TASK STATE
Three to five sentences. What is the work, its purpose, and exactly where it stands right now. Name the current unit of work (story, ticket, feature, milestone) and its status. Include the immediate prior step completed and the immediate next step queued. If the project has a named goal or deadline, include it.

### CURRENT HEAD
A structured set of state lines, one fact per line. Include only what applies:
- `Branch:` branch name and a one-phrase note on its purpose if not obvious
- `Commit:` latest commit SHA (short) and one-line message
- `Working tree:` clean, or dirty with a description of what is uncommitted and why it is not yet committed
- `Pushed:` whether HEAD is pushed to remote, and if not, why
- `Backend service:` name, project status, and any pending migration or schema state (for example Supabase, Firebase, PlanetScale)
- `Deploy target:` platform, plan, and current deploy status: live, pending, or broken (for example Vercel, Netlify, Fly)
- `Env:` any credential, environment variable, or secrets wiring state that is incomplete or fragile
- `Other infra:` any additional infrastructure relevant to the next session (DNS, external APIs, third-party services, feature flags). Omit this line entirely if no additional infra applies; do not write `Other infra: None`.
Omit any line that does not apply. Mark anything you could not verify as `[unverified]`. Add a one-sentence note for any line that is in a non-standard or unexpected state.

For non-code sessions with no git repo (writing projects, research, workflow design, business planning), replace the git fields with equivalent state: the primary artifact being produced, its current state, and any external dependencies or tools it relies on. Use the same one-fact-per-line format.

### DECISIONS LOCKED
Bulleted, grouped by domain with a bold domain label per group (for example **Product:**, **Architecture:**, **Tooling:**, **Data:**, **Security:**). These are do-not-reopen decisions. Be specific and load-bearing: not "pricing was discussed" but "Free tier = 3 projects, unlimited everything else; paid tier deferred to V2." Where a decision is fragile or easy to erode, append a `(do not weaken)` flag and include a brief rationale clause. Include the triggering reason for each decision when that context is what prevents the next session from relitigating it. Aim for completeness: a decision omitted here is a decision at risk of being undone.

### ACTIVE GOTCHAS
Five to twelve lines. Only traps that can bite the next session specifically. Each line is one gotcha, the symptom it produces if ignored, and the guard against it. Prioritize: silent-error risks first (values that look interchangeable but are not, platform quirks that fail quietly), then version or environment deviations, then dependency or integration constraints, then anything that already cost time this session. If there are more than twelve, keep the twelve most likely to cause a silent or hard-to-diagnose failure and leave the rest to the archive.

### OPEN QUESTIONS
Bulleted. Everything genuinely unresolved, still being iterated, or awaiting an external dependency. Each item includes: the question itself, what is currently known or assumed, and a resolve-by marker for when the answer is needed (for example `(resolve before Story 1.3)` or `(resolve before launch)`). If you are unsure whether something was finalized, it goes here, not in DECISIONS LOCKED. Group by domain if there are more than five items.

### ARCHIVE POINTER
One to three lines. If the user keeps a full session log or archive, point to it: `Full session log: <path>`. Add secondary lines for any other reference documents that were produced or referenced this session and are not in the log (design docs, ADRs, spec files, prompt bodies). If no archive exists, write: `No archive committed. This handoff is the only record; save it somewhere durable if you need the detail later.`

### NEXT STEP
Three lines maximum, inside a fenced code block, written as the exact text to paste into the new conversation. Line 1: name the project and the unit of work being resumed. Line 2: the immediate next action. Line 3: `The attached handoff is authoritative; read it before acting.` Do not restate decisions, gotchas, or task state here; they are already in the blocks above and repeating them is paid on every future turn. The handoff travels with this pointer; the pointer does not need to survive alone.

---

## Worked Example (v2 spec, for shape only)

````
### TASK STATE
Building a multi-tenant SaaS API in Next.js for a B2B workflow automation tool targeting small operations teams. Planning phase complete and pushed; Story 1.1 (Next.js scaffold, base layout, route structure) done and pushed. Story 1.2 (AES-256-GCM credential encryption helpers, server-only, no client exposure) is next in queue. Goal is a working MVP by end of sprint 3; Story 1.2 is on the critical path because all stored credentials depend on it.

### CURRENT HEAD
Branch: main (primary integration branch, no feature branches in use for MVP)
Commit: 4bc7e2a security and config fixes after Story 1.1 review
Working tree: clean
Pushed: yes
Backend service: Supabase; project not yet created; needed at Story 1.3 for auth and row-level security setup
Deploy target: Vercel Hobby, pointed at repo; no deploy until Story 1.3 env vars are wired
Env: NEXT_PUBLIC_SUPABASE_URL and SUPABASE_SERVICE_ROLE_KEY not yet set; placeholders in .env.example only

### DECISIONS LOCKED
**Product:** BYOK model; users supply their own API keys, platform never stores them server-side (do not weaken, this is the core trust proposition). Free tier = 3 active workflows, unlimited everything else; paid tier and Stripe deferred to V2.
**Architecture:** Result<T> error wrapper at all external API boundaries; overrides generic try/catch pattern (do not weaken, silent swallowed errors are the failure mode we are preventing). Atomic commits via GitHub Git Data API, not Contents API; Contents API does not support atomic multi-file writes.
**Tooling:** Tailwind 3.4.19 locked for MVP; do not upgrade to Tailwind 4 (breaking config changes, not worth mid-sprint migration).
**Security:** Encryption helpers are server-only; no crypto logic may be imported into any client component.

### ACTIVE GOTCHAS
- sync_baseline_sha (used during push) and latest_context_commit_sha (used during conflict detection) are NOT interchangeable; symptom is silent conflict resolution failure that looks like a successful push; guard is always source the SHA from the correct operation context.
- Windows dev environment: use py not python3 or scripts silently find the wrong interpreter; rm -rf fails if CWD is inside the target directory, use cmd //c "rmdir /s /q dirname" instead.
- Stack intentionally deviates from architecture doc (running Next 16 / Tailwind 3, doc says Next 15 / Tailwind 4); symptom is wasted time "fixing" a non-bug; guard is do not align to the doc, the doc is wrong and was accepted this session.
- AES-256-GCM requires a unique IV per encryption operation; symptom of IV reuse is silent encryption that is cryptographically broken with no runtime error; guard is generate IV fresh inside every encrypt call, never pass it in.
- Result<T> wrapper must be applied at the boundary where the external call is made, not one layer up; symptom is uncaught throws escaping the wrapper at the call site; guard is apply at the lowest layer that touches the external service.

### OPEN QUESTIONS
- Supabase project not yet created; currently assumed to be a fresh project with no existing schema; needed before Story 1.3 RLS setup can begin (resolve before Story 1.3).
- Legacy data migration scope not yet defined; currently assumed out of scope for MVP; needs a decision before Story 1.4 data layer work begins (resolve before Story 1.4).

### ARCHIVE POINTER
Full session log: _output/sessions/2026-05-31-SESSION-01.md

### NEXT STEP
```
Continuing the B2B workflow automation API; resuming Story 1.2 (AES-256-GCM credential encryption helpers, server-only).
Begin Story 1.2 with manual review gates enabled.
The attached handoff is authoritative; read it before acting.
```
````

---

## Guardrails

- **This is a handoff, not an archive.** Do not carry issue root-causes, full prompt bodies, or complete file lists. Those live behind the ARCHIVE POINTER. If you feel the urge to preserve a full prompt body here, stop and point to the archive instead.
- **Length budget is roughly 40 content lines, hard ceiling 80.** Between 40 and 80 is acceptable when the session has unusually high decision density or infrastructure complexity. Above 80 means archive material has leaked into the handoff. Cut and rely on the ARCHIVE POINTER. Count lines during generation; line counts are self-checkable, token counts are not.
- **Omit empty blocks, account for them in the ledger.** If any block is empty, the first line of the output is `Blocks empty this session: <names>`. If all blocks have content, no ledger line. Never pad an empty block to avoid the ledger.
- **Verify CURRENT HEAD against live state** when a repo is in context. Never state a commit SHA or clean/dirty status you have not confirmed; mark unverifiable values `[unverified]`.
- **ACTIVE GOTCHAS is next-session-relevant only.** A gotcha that cannot recur in the next phase of work belongs in the archive, not here. Include the symptom so the next session can recognize a failure if the gotcha is ignored.
- **DECISIONS LOCKED must be complete.** A decision omitted here is a decision at risk of being undone silently. When in doubt, include it and flag the domain.
- **If uncertain whether a decision was finalized,** put it in OPEN QUESTIONS with a resolve-by marker, not in DECISIONS LOCKED.
- **NEXT STEP is a three-line pointer, not a briefing.** It names the project and unit of work, states the next action, and defers to the handoff as authoritative. Do not duplicate decisions or gotchas into it; every duplicated line is re-paid on every turn of the new conversation.
- **Output is markdown only.** No preamble, no meta-commentary, no closing text outside the blocks. The first character of the response must be the first character of the ledger line if one is present, otherwise the first character of the first block header.
- **Style:** no em dashes (use commas, periods, or parentheses). No emojis. No pleasantries or meta-commentary above or below the blocks.
- **Do not invent state.** If the conversation does not establish a fact, omit it or mark it unverified. A confident wrong handoff is worse than an honest gap.
