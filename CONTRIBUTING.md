# Contributing

Contributions are welcome. This skill is a markdown file with a YAML frontmatter block, so changes are straightforward to propose.

---

## What Is Worth Changing

- **Trigger phrases:** If a natural phrase that should invoke the skill is not listed, add it to the `description` field in the frontmatter.
- **Section depth:** If a section consistently produces too little or too much useful information in practice, adjust the sentence count or line count guidance.
- **Token budget:** If the ceiling is consistently hit or consistently unnecessary, adjust the target and ceiling values in the Output Format section and the corresponding Guardrails entry.
- **Block content rules:** If a type of project state is not covered by the current field list (for example, a common infra provider not listed under CURRENT HEAD), add it.

## What Is Not Worth Changing

- **The seven blocks themselves.** The fixed structure is the value. A new session can always find what it needs because the blocks never move. If you need different content, adjust what goes inside a block, not the block itself.
- **The "no preamble, no pleasantries" rule.** This is a machine-readable handoff, not a response. Anything that is not one of the seven blocks is noise.

---

## How to Contribute

1. Fork the repository.
2. Edit `SKILL.md` directly.
3. Update `CHANGELOG.md` with a description of what changed and why.
4. Open a pull request with a clear title and a one-paragraph description of the problem you are solving.

If you are proposing a structural change (adding or removing a block, changing the token budget significantly), open an issue first to discuss the tradeoff before writing the PR.

---

## Testing a Change

Install the modified skill in your Claude.ai account (Settings > Skills > Personal skills), run a real working session, and trigger the handoff. Verify:

1. All seven blocks are present.
2. NEXT STEP is paste-ready and self-contained.
3. Total output is under your stated hard ceiling.
4. A fresh session that receives only the NEXT STEP block can make meaningful progress without the rest of the handoff.
