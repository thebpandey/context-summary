# context-summary

A Claude.ai Personal Skill that generates a structured, seven-block session handoff so you can continue work in a new conversation without losing context, rediscovering decisions, or re-explaining your project state.

---

## Table of Contents

- [What It Does](#what-it-does)
- [Why This Exists](#why-this-exists)
- [Impact](#impact)
- [How It Works](#how-it-works)
- [The Seven-Block Output](#the-seven-block-output)
- [Trigger Phrases](#trigger-phrases)
- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
- [Token Budget](#token-budget)
- [Community](#community)
- [Author](#author)
- [License](#license)

---

## What It Does

`context-summary` compresses the active Claude.ai conversation into a structured, seven-block markdown document. That document is the minimum information a fresh Claude session needs to resume work immediately, at full fidelity, without re-reading the full chat history.

Think of it as a boarding pass for your next session. Not an archive. Not a transcript. A precise handoff.

One trigger phrase. Seven blocks. Copy, paste, keep building.

---

## Why This Exists

Claude.ai conversations have a context window. Once a conversation grows long enough, earlier decisions, locked choices, and discovered gotchas fall out of active memory. You either pay to keep re-summarizing them in every prompt, start fresh and lose continuity, or manually copy-paste a wall of text that the next session still has to parse.

None of those are good options when you are mid-build on something that has real state: a codebase with live infrastructure, a project with locked architectural decisions, a workflow where the wrong assumption costs an hour of rework.

`context-summary` solves this by producing a deterministic, structured handoff that a new session can ingest in one paste. The format is fixed, so the new session always knows where to look for the branch state, the locked decisions, and the first action to take.

This works for technical and non-technical sessions alike. Developers, writers, researchers, and planners all accumulate session debt the same way.

---

## Impact

**Before this skill:**
- Long sessions accumulate invisible debt. Decisions made in turn 5 are invisible by turn 50.
- Starting a new conversation means re-explaining the project from scratch or pasting a raw transcript and hoping Claude extracts the right things.
- Critical gotchas (a field that silently shadows another, a platform quirk that fails quietly, a constraint you already negotiated) get rediscovered the hard way in the next session.
- There is no consistent format, so every session restart feels different and something always gets missed.

**After this skill:**
- One trigger phrase generates a complete handoff in under 30 seconds.
- The new session starts with locked decisions, live infrastructure state, active gotchas, and a paste-ready first prompt.
- The format is fixed, so nothing is ever silently omitted.
- You stop re-explaining and start building.

**Who benefits most:**
- Developers using Claude Code or Claude chat for multi-session builds.
- Technical users running long agentic workflows where state accumulates.
- Writers, researchers, and planners who carry project context across multiple working sessions.
- Anyone whose sessions routinely exceed a few hours of work and involve decisions, constraints, or discovered edge cases.

---

## How It Works

When you say one of the trigger phrases, Claude reads the current conversation directly. No copy-paste required from you. It then produces a fixed seven-block markdown document.

If a connected git repo is in context, Claude pulls live branch and commit state rather than guessing. If no repo is in context, it fills what it can from the conversation and marks anything it cannot verify.

For non-code sessions (writing, research, planning), CURRENT HEAD adapts to describe the primary artifact and its state instead of git fields.

The output is markdown only. No preamble. No pleasantries. No meta-commentary. Just the seven blocks in order, ready to copy.

---

## The Seven-Block Output

| Block | Purpose |
|---|---|
| `TASK STATE` | 3-5 sentences: what the work is, where it stands, prior step done, next step queued. |
| `CURRENT HEAD` | Git branch, commit SHA, working tree status, and infra state. Adapts for non-code sessions. |
| `DECISIONS LOCKED` | Do-not-reopen decisions grouped by domain (Product, Architecture, Tooling, Data, Security). |
| `ACTIVE GOTCHAS` | 5-12 traps that can bite the next session, each with its symptom and guard. |
| `OPEN QUESTIONS` | Unresolved items, each with current assumption and a resolve-by marker. |
| `ARCHIVE POINTER` | Path to the full session log for anything not carried in the handoff. |
| `NEXT STEP` | A paste-ready prompt to drop into the new conversation to resume immediately. |

Every block is always present. If a block has no content, it reads `None this session.` so the new session never wonders if a block was accidentally omitted.

---

## Trigger Phrases

Any of the following (exact or close paraphrase) invokes the skill:

- `context summary`
- `context-summary`
- `context handoff`
- `handoff summary`
- `give me the handoff`
- `hand this off`
- `carry context forward`
- `prep the handoff`

**Note:** The signal for this skill is the intent to carry work forward into a new conversation, not to produce a full archive of the current one. If you want a complete record of everything that happened in a session, that is a different need; this skill produces a lean forward-looking handoff, not an archive.

---

## Installation

Claude.ai Personal Skills are installed through the Claude.ai settings interface. You need a Claude.ai account (Pro, Team, or Enterprise) that supports Personal Skills.

### Step 1: Copy the Skill File

Download or copy the raw content of `SKILL.md` from this repository.

Direct link to raw file:
```
https://raw.githubusercontent.com/thebpandey/context-summary/main/SKILL.md
```

### Step 2: Open Claude.ai Settings

1. Go to [claude.ai](https://claude.ai)
2. Click your profile icon (bottom-left of the sidebar)
3. Select **Settings**

### Step 3: Navigate to Skills

1. In Settings, find the **Skills** section (also labeled "Plugins" in some interface versions)
2. Select **Personal skills**
3. Click **Add skill** or the equivalent button

### Step 4: Paste the Skill

1. Paste the full contents of `SKILL.md` into the skill editor
2. The skill name will auto-populate from the frontmatter as `context-summary`
3. Save the skill

### Step 5: Verify Installation

Start a new conversation and type:

```
context summary
```

Claude should respond with the seven-block handoff structure immediately, without asking you to explain what it should do.

### Troubleshooting

**Skill does not trigger:** Make sure the frontmatter at the top of the skill file was included when you pasted. The `name:` and `description:` fields in the frontmatter are what Claude uses to match trigger phrases.

**Claude asks me to paste the conversation:** The skill is designed to read the current conversation directly. If Claude asks you to paste, the skill may not have loaded correctly. Re-check that the full skill file content was saved.

**Output is too short or missing blocks:** Confirm you installed the v2.1 file (token ceiling 5,000) rather than the original (ceiling 3,000). The version in this repo is v2.1.

---

## Usage

### Basic Usage

At any point during a long working session, type:

```
context summary
```

Claude generates the seven-block handoff. Copy the entire output and paste it into the new conversation as your first message.

### Adding External Context

If you have notes, a prior handoff, or state from another conversation to include, say:

```
I have context to add before you generate the handoff
```

Claude will pause for you to paste. After you paste, say `continue` and it will generate the handoff incorporating what you provided.

### Short Sessions

For sessions under roughly 10 turns with no accumulated decisions or gotchas, the skill produces a lean handoff: a brief TASK STATE and `None this session.` in the remaining blocks. This is correct behavior. Do not expect a padded output for a short session.

---

## Examples

### Example 1: Technical Session (Software Build)

This example shows a handoff for a multi-session Next.js API build with live infrastructure and locked architectural decisions.

```
### TASK STATE
Building a multi-tenant SaaS API in Next.js for a B2B workflow automation tool targeting small operations teams. Planning phase complete and pushed; Story 1.1 (Next.js scaffold, base layout, route structure) done and pushed. Story 1.2 (AES-256-GCM credential encryption helpers, server-only, no client exposure) is next in queue. Goal is a working MVP by end of sprint 3; Story 1.2 is on the critical path because all stored credentials depend on it.

### CURRENT HEAD
Branch: main (primary integration branch, no feature branches in use for MVP)
Commit: 4bc7e2a security and config fixes after Story 1.1 review
Working tree: clean
Pushed: yes
Supabase: project not yet created; needed at Story 1.3 for auth and row-level security setup
Vercel: Hobby plan, pointed at repo; no deploy until Story 1.3 env vars are wired
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
Continuing the B2B workflow automation API. Story 1.1 (scaffold) is done and pushed. Story 1.2 (AES-256-GCM credential encryption helpers, server-only) is next.

Key locked decision: encryption helpers are server-only, no client imports permitted.
Key gotcha: AES-256-GCM requires a fresh IV generated inside every encrypt call; IV reuse is silent and cryptographically broken.

Begin Story 1.2 with manual review gates enabled. This is foundational; all stored credentials depend on it. Full attention required before proceeding to Story 1.3.
```
```

---

### Example 2: Non-Technical Session (Research and Writing Project)

This example shows a handoff for a long-form research and writing project with no git repo. CURRENT HEAD adapts to describe the primary artifact state instead of git fields.

```
### TASK STATE
Writing a comprehensive guide on land exchange agreements for commercial real estate investors, targeting professionals who understand finance but are new to federal land transactions. Research phase is complete across five primary sources; Chapter 3 (valuation methodology) is drafted and reviewed. Chapter 4 (environmental compliance and title risk) is next. Deadline is two weeks out; Chapters 4 and 5 are the remaining critical path items.

### CURRENT HEAD
Artifact: "Land Exchange Agreements: A Practitioner's Guide" — Google Doc, shared with editor
State: Chapters 1-3 complete and reviewed; Chapter 4 outline drafted, not yet written
External dependencies: BLM appraisal standards document (saved in /references); editor review of Ch. 3 pending response
Tools: Google Docs (primary), Perplexity (research), Claude (drafting and structure)

### DECISIONS LOCKED
**Scope:** Guide covers federal land exchanges only; state-level and private exchanges explicitly excluded to keep scope manageable (do not expand, scope creep was flagged and rejected in session 1).
**Audience:** Written for investors with financial fluency but no BLM process knowledge; no assumption of prior federal land experience; all acronyms defined on first use.
**Structure:** Five chapters locked: Overview, Legal Framework, Valuation, Environmental and Title Risk, Deal Structuring. Order is non-negotiable; valuation must precede risk because risk quantification depends on valuation concepts established in Ch. 3.
**Voice:** Third-person professional throughout; no first-person anecdotes; no promotional language about any specific firm or service.

### ACTIVE GOTCHAS
- BLM appraisal standards changed in 2024; older sources (pre-2024) cite outdated equal-value requirements; symptom is incorrect valuation methodology in Ch. 4; guard is use only the saved /references/BLM-appraisal-2024.pdf, not web searches.
- Chapter 3 uses "equalization payment" as the preferred term; earlier drafts used "boot payment" interchangeably; symptom is inconsistent terminology that confuses readers; guard is find-and-replace "boot payment" before final review.
- Editor has not yet confirmed the Ch. 3 structural change (moving the comparable sales table to an appendix); symptom is writing Ch. 4 cross-references that point to a location that may move; guard is hold Ch. 4 cross-references until editor confirms.
- Environmental compliance section must reference NEPA, not state equivalents; symptom is inaccurate regulatory guidance; guard is verify every cited regulation against federal sources only.
- Word count target is 8,000-10,000 words for the full guide; Ch. 3 came in at 2,400 words; Ch. 4 and 5 must stay under 2,000 each or the guide exceeds scope.

### OPEN QUESTIONS
- Editor response on Ch. 3 structural change (appendix vs. inline table) not yet received; currently assumed the table stays inline until confirmed otherwise (resolve before writing Ch. 4 cross-references).
- BLM contact for factual review not yet identified; currently assumed the guide will carry a standard disclaimer rather than an expert endorsement (resolve before external publication).

### ARCHIVE POINTER
No archive committed. This handoff is the only record; save it somewhere durable if you need the detail later.
Reference documents: /references/BLM-appraisal-2024.pdf, /references/source-list.md

### NEXT STEP
```
Continuing the Land Exchange Agreements practitioner's guide. Chapters 1-3 are complete and reviewed. Chapter 4 (environmental compliance and title risk) is next.

Key locked decision: federal land exchanges only, no state or private exchanges; scope is fixed.
Key gotcha: all regulatory citations must reference federal sources (NEPA, BLM standards); state equivalents are incorrect for this guide.

Begin Chapter 4 using the approved outline. Hold cross-references to the Ch. 3 comparable sales table until editor confirms whether it moves to an appendix. Target under 2,000 words for this chapter.
```
```

---

## Token Budget

| Threshold | Meaning |
|---|---|
| Under 3,000 tokens | Target. Typical session handoff. |
| 3,000 to 5,000 tokens | Acceptable for high-density sessions (many decisions, complex infra, or large decision set). |
| Above 5,000 tokens | Archive material has leaked into the handoff. Cut it. |

The skill enforces this budget through its guardrails. If you find handoffs consistently hitting the ceiling, the session likely has content that belongs in the archive behind the ARCHIVE POINTER rather than in the handoff itself.

---

## Community

If you use this skill and it saves you time, open an issue or start a discussion on GitHub. Specific feedback on sections that consistently over-produce or under-produce is useful for future versions.

If you fork or adapt this skill for a workflow it does not currently cover (a different infrastructure stack, a domain-specific variant, a non-technical adaptation), open a PR or link your version in Discussions.

---

## Author

**Bhaskar Pandey**

Builder, technical strategist, and real estate professional. Not affiliated with Anthropic.

- GitHub: [github.com/thebpandey](https://github.com/thebpandey)
- LinkedIn: [linkedin.com/in/pandeybhaskar](https://www.linkedin.com/in/pandeybhaskar/)
- X: [@thebpandey](https://x.com/thebpandey)
- Email: [bhaskar.knp@gmail.com](mailto:bhaskar.knp@gmail.com)

If you find issues or want to propose changes, GitHub Issues and pull requests are preferred. For everything else, reach out on LinkedIn or X.

---

## License

MIT. Use freely, modify for your workflow, share with attribution.

---

## Claude Code

A Claude Code command version of this skill is available in the `claude-code/` directory of this repo.

### Installation

1. Create the Claude Code commands directory if it does not exist:

```bash
mkdir -p ~/.claude/commands
```

2. Copy the command file:

```bash
cp claude-code/context-summary.md ~/.claude/commands/context-summary.md
```

3. Restart any active Claude Code session. The command will appear in autocomplete when you type `/`.

### Usage
/context-summary

Claude Code will pull live git state, generate the handoff, write it to `_sessions/` in your current repo, and ask whether you want the output displayed in chat. Default is no display; press Enter to skip.

