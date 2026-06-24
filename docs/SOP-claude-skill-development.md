# SOP: Building and Deploying a Claude Code Skill

**Owner:** Skill developer / team lead
**Version:** 1.1
**Last updated:** 2026-06-24
**Frequency:** Per skill — once per new skill, then on each iteration
**Average time:** 2–4 hours for a first skill; 30–60 minutes for subsequent skills
**Why it matters:** A skill built on a vague description misfires or gets ignored. A skill with no output spec produces inconsistent results. A skill with no edge case handling breaks on the first unusual input. Getting the structure right once prevents all three failure modes from being repeated across every skill you ship.
**Related SOPs:** SOP-process-documentation.md (run this first if the process isn't documented yet)

---

## Prerequisites

- Claude Code installed and running (`claude --version` to verify)
- The process or workflow to be packaged is already documented — either in your head clearly enough to specify, or as a completed SOP from the SOP Builder skill
- A text editor capable of saving `.md` files
- Access to `~/.claude/skills/` on the target machine (create the directory if it does not exist)

---

## Trigger

What starts this process and how you know it is time to begin.

- **Signal:** You have a repeatable task that Claude currently handles inconsistently, or that requires re-explaining every session
- **Input arrives via:** Internal need, team request, or completed SOP identifying an automation candidate
- **Input looks like:** A documented process (steps, decision rules, output spec) or a clear enough mental model to write one

---

## Steps (the happy path)

| # | Step | Tool / system | What to look at | Output of this step |
|---|------|---------------|-----------------|---------------------|
| 1 | Create the skill folder | Terminal / file explorer | Folder name is lowercase, hyphen-separated, no spaces | `~/.claude/skills/your-skill-name/` exists |
| 2 | Create `SKILL.md` with frontmatter block | Text editor | `name` matches folder name exactly; `description` includes job, context, and at least two trigger phrases | Valid frontmatter block saved |
| 3 | Write the skill body | Text editor | Specifies *how* not just *what*; has explicit output spec; handles at least 3 predictable edge cases; uses When to Use / How It Works / Rules / Output / Tone structure | Complete skill body saved |
| 4 | Add supporting files if needed | Text editor | Files are in the same skill folder; referenced by filename in skill body | Supporting files in place |
| 5 | Restart Claude Code | Terminal | Session fully closed and reopened — not just a new conversation tab | Fresh session running |
| 6 | Test the primary trigger | Claude Code | Type the exact slash command or trigger phrase from the description | Skill invokes correctly |
| 7 | Test edge cases | Claude Code | Run each scenario from the edge case list below | Skill handles all three without breaking |
| 8 | Iterate if needed | Text editor → Terminal | Edit SKILL.md, restart Claude Code, retest | Clean pass on all test scenarios |

---

## Decision Points

**Decision: Does the skill need supporting files?**
- **Yes when:** The output must follow a specific template; the skill references a checklist, data set, or reference document that is too long to embed in the skill body
- **No when:** All instructions and output format can be specified inline in SKILL.md without it becoming unwieldy (>300 lines)
- **Info needed:** The output spec and whether it is stable enough to template

**Decision: Should I use `triggers` field or just `description`?**
- **Use `description` only when:** The slash command phrase is included in the description text — this handles both natural language and slash command invocation
- **Add `triggers` when:** You need guaranteed invocation on an exact slash command even if the natural language matching would not fire (rare)
- **Default:** Start with description only. Add triggers only after testing shows a gap.

**Decision: Is the skill ready to ship or does it need another iteration?**
- **Ship when:** Primary trigger fires correctly on first attempt; all three edge case tests pass; output format is consistent across two independent runs
- **Iterate when:** Any test fails; output differs meaningfully between runs; the skill fires when it should not (false trigger)

---

## Exceptions and Edge Cases

**When the skill does not trigger after install:**
Check in this order: (1) Was Claude Code restarted after installing? (2) Does the `name` field exactly match the folder name? (3) Does the `description` field contain the phrase you used to trigger it? (4) Is the frontmatter block valid YAML with `---` delimiters on both sides?

**When the skill triggers on unrelated requests (false positive):**
The `description` is too broad. Narrow it by adding specific context ("only when the user is asking to document a *business process*") and removing phrases that could match general requests.

**When output is inconsistent across runs:**
An explicit output specification is missing or insufficient. Go back to the skill body and add: the exact format (table / numbered list / Markdown file), the exact filename if saved to disk, and the specific sections that must always appear.

**When a supporting file is not found:**
The file is not in the skill folder. Move it to `~/.claude/skills/your-skill-name/`. Verify the reference in the skill body uses the filename only, not a path.

**When the skill was updated but Claude is running the old version:**
Claude Code was not restarted after the edit. Skills load at session start. Restart the session.

**Stop and escalate when:** The skill is producing outputs that could be sent to external parties (clients, customers) and the output is wrong in a way that is not obviously caught before sending — pause, involve a second reviewer, and revise the output spec before continuing.

---

## Tools, Access, and Data

- **Tools used:** Text editor (any), Terminal, Claude Code CLI
- **Access required:** Write access to `~/.claude/skills/` on the machine where Claude Code runs; no external credentials or API keys needed
- **Where the data lives:**
  - Before: Process documentation in the developer's head or in an SOP document
  - During: `SKILL.md` and supporting files in the skill folder (local filesystem)
  - After: Deployed skill in `~/.claude/skills/your-skill-name/`; optionally committed to a shared repository for team distribution

---

## Definition of Done

- **Finished when:** Primary trigger fires on the first attempt; three edge case scenarios pass; output format is consistent across two independent runs from a cold session
- **A good output looks like:** Claude runs the skill without any prompting beyond the trigger; produces output in the format specified; handles interruptions gracefully; does not require correction on a standard run
- **Checked by:** Skill developer self-tests, then optionally a second person unfamiliar with the process runs the trigger cold — if they need to prompt Claude beyond the trigger, the skill body is under-specified

---

## Handoffs

- **Receives from:** A completed process document or SOP (output of the SOP Builder skill), or a clear enough mental model to write one directly
- **Passes to:** The team or individual who will use the skill — distribute via shared repository or direct file copy to `~/.claude/skills/` on their machine
- **Most common reason for rework:** The `description` field was written too broadly (causes false triggers) or the output spec was omitted (causes inconsistent results)

---

## Notes for Automation

- **Pure mechanics (agent can own):** Creating the folder, writing boilerplate frontmatter, running the restart-and-test loop for known trigger phrases
- **Needs human judgment (keep in the loop):** Writing the `description` field (requires knowing what *not* to match); specifying edge cases (requires knowing the ways the process actually breaks); deciding when output quality is sufficient to ship
- **Open questions to resolve before automating:** How to validate that a skill does not false-trigger without exhaustive manual testing; how to distribute skills to a team without manual file copying

---

## Revision History

| Version | Date | Changed by | What changed |
|---------|------|------------|--------------|
| 1.0 | 2026-06-24 | Claude (session audit) | Initial version |
| 1.1 | 2026-06-24 | Claude (session audit) | Added triggers vs description decision point; clarified escalation condition; added cold-session test to definition of done |
