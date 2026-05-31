# context-summary

A Claude.ai Personal Skill that generates a lean, paste-ready session handoff so work can continue in a new conversation without losing context.

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
- [Token Budget](#token-budget)
- [Related Skills](#related-skills)
- [License](#license)

---

## What It Does

`context-summary` compresses the active Claude.ai conversation into a structured, seven-block markdown document. That document is the minimum information a fresh Claude session needs to resume work immediately, at full fidelity, without re-reading the full chat history.

Think of it as a boarding pass for your next session. Not an archive. Not a transcript. A precise handoff.

---

## Why This Exists

Claude.ai conversations have a context window. Once a conversation grows long enough, earlier decisions, locked choices, and discovered gotchas fall out of active memory. You either pay to keep re-summarizing them in every prompt, start fresh and lose continuity, or manually copy-paste a wall of text that the next session still has to parse.

None of those are good options when you are mid-build on something that has real state: a codebase with live infrastructure, a project with locked architectural decisions, a workflow where the wrong assumption costs an hour of rework.

`context-summary` solves this by producing a deterministic, structured handoff that a new session can ingest in one paste. The format is fixed, so the new session always knows where to look for the branch state, the locked decisions, and the first action to take.

---

## Impact

**Before this skill:**
- Long sessions accumulate invisible debt. Decisions made in turn 5 are invisible by turn 50.
- Starting a new conversation means re-explaining the project from scratch or pasting a raw transcript and hoping Claude extracts the right things.
- Critical gotchas (a field that silently shadows another, a platform quirk that fails quietly) get rediscovered the hard way in the new session.

**After this skill:**
- One trigger phrase generates a complete handoff in under 30 seconds.
- The new session starts with locked decisions, live infrastructure state, active gotchas, and a paste-ready first prompt.
- You stop re-explaining and start building.

**Who benefits most:**
- Developers using Claude Code or Claude chat for multi-session builds.
- Technical users running long agentic workflows where state accumulates.
- Anyone whose sessions routinely exceed a few hours of work and involve infrastructure, decisions, or discovered constraints.

---

## How It Works

When you say one of the trigger phrases, Claude reads the current conversation directly (no copy-paste required from you) and produces a fixed seven-block markdown document. If a connected git repo is in context, Claude pulls live branch and commit state rather than guessing. If no repo is in context, it fills what it can from the conversation and marks anything unverifiable.

The output is markdown only. No preamble. No pleasantries. No meta-commentary. Just the seven blocks in order, ready to copy.

---

## The Seven-Block Output

| Block | Purpose |
|---|---|
| `TASK STATE` | 3-5 sentences on what the work is and exactly where it stands. |
| `CURRENT HEAD` | Git branch, commit SHA, working tree status, and infra state (Supabase, Vercel, env vars). |
| `DECISIONS LOCKED` | Do-not-reopen decisions grouped by domain (Product, Architecture, Tooling, Data, Security). |
| `ACTIVE GOTCHAS` | 5-12 traps that can bite the next session, each with its symptom and guard. |
| `OPEN QUESTIONS` | Unresolved items with a resolve-by marker for each. |
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

This skill is distinct from `session-summary` (a longer rescue package) and `kodetrain-session` (a structured app session log). If you name one of those explicitly, the correct skill fires instead.

---

## Installation

Claude.ai Personal Skills are installed through the Claude.ai settings interface. You need a Claude.ai account (Pro, Team, or Enterprise) that supports Personal Skills.

### Step 1: Copy the Skill File

Download or copy the raw content of `SKILL.md` from this repository.

Direct link to raw file:
```
https://raw.githubusercontent.com/[YOUR_GITHUB_USERNAME]/context-summary/main/SKILL.md
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

**Output is too short or missing blocks:** Confirm you installed the v2 file (token ceiling 5,000) rather than the original (ceiling 3,000). The version in this repo is v2.

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

### Chaining With Other Skills

`context-summary` pairs naturally with:

- `session-summary` or `kodetrain-session`: Run those first to generate an archive, then run `context-summary` to generate the lean handoff that points to it.
- Any build or dev workflow: Use at natural session boundaries (end of a story, before switching focus, before a long break).

---

## Token Budget

| Threshold | Meaning |
|---|---|
| Under 3,000 tokens | Target. Typical session handoff. |
| 3,000 to 5,000 tokens | Acceptable for high-density sessions (many decisions, complex infra). |
| Above 5,000 tokens | Archive material has leaked into the handoff. Cut it. |

The skill enforces this budget through its guardrails. If you find handoffs consistently hitting the ceiling, the session likely has content that belongs in the archive behind the ARCHIVE POINTER rather than in the handoff itself.

---

## Related Skills

These skills are referenced within `context-summary` as the archive layer:

- **session-summary**: Produces a full Context Rescue Package for long or complex sessions. Trigger: `Summarize Session`, `Session end`, `End of session`.
- **kodetrain-session**: Produces a structured two-section app session log for the KodeTrain companion app. Trigger: `Kode end`, `Kode Summary`, `End Kode Session`.

Those skills are maintained separately and are not included in this repository.

---

## License

MIT. Use freely, modify for your workflow, share with attribution.
