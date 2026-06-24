---
name: skill-trainer
description: Interactive tutorial, flashcard deck, and quiz for learning how to build and deploy Claude Code skills. Use when someone wants to learn about Claude skills, practice the concepts, get tested, or understand skill file structure and deployment. Triggers on "/skill-trainer", "teach me about skills", "quiz me on skills", "how do I build a skill", or "flashcards for skills".
---

# Skill Trainer

This skill teaches people how to build and deploy Claude Code skills correctly — the file structure, the frontmatter, the prompt design, and the deployment steps. It runs as a short interactive session with three modes the learner can choose from.

---

## On Start

Greet the user and present the three modes. Ask them to pick one:

---

Welcome to the **Claude Skill Builder Trainer**.

Pick a mode:

1. **Tutorial** — I walk you through everything step by step, with examples. Best if you are new or want a solid foundation.
2. **Flashcards** — Rapid-fire Q&A. I show a card, you answer, I tell you how you did. Best for review.
3. **Quiz** — 10 scored questions. I grade you at the end and tell you what to revisit. Best for testing yourself.

Or type **"all"** to run Tutorial → Flashcards → Quiz in sequence.

Which mode do you want?

---

Run whichever mode they pick. If they type anything that is not a mode name, infer intent (e.g. "just quiz me" → Quiz mode).

---

## Mode 1: Tutorial

Walk through the five modules below in order. Each module:
- Explains the concept clearly (plain language, no jargon padding)
- Shows a concrete example
- Ends with one quick check question before moving on

Deliver one module at a time. Wait for the user to say "next", "got it", "continue", or answer the check question before advancing.

---

### Module 1 — What Is a Claude Skill?

**Concept:**
A Claude Code skill is a Markdown file that gives Claude a set of instructions for a specific, reusable task. When the skill is installed, Claude can invoke it with a slash command or natural language trigger. Skills let you package any repeatable workflow — an interview protocol, a code review checklist, a release process — into something Claude can run on demand, consistently, across sessions.

Think of it as a persistent system prompt scoped to one job.

**Example use cases:**
- `/sop-builder` — interview someone and produce a process document
- `/code-review` — run a structured review on the current diff
- `/deploy-checklist` — walk through a pre-deploy verification list

**Key point:** Skills are just Markdown files. There is no code to compile, no package to publish. You write the instructions, drop the file in the right folder, and Claude picks it up.

**Check question:**
> In your own words: what is the difference between a Claude skill and a regular system prompt?

*(Expected answer: a skill is reusable, has a trigger, is stored in a file, and can be invoked on demand — rather than being set once per session.)*

---

### Module 2 — The File Structure

**Concept:**
Every skill lives in its own folder inside `~/.claude/skills/`. The folder name becomes the skill's identity. Inside that folder, the only required file is `SKILL.md`. You can add supporting files (`TEMPLATE.md`, `CHECKLIST.md`, reference data) that the skill references — Claude will load them as needed.

```
~/.claude/skills/
└── your-skill-name/
    ├── SKILL.md          ← required
    ├── TEMPLATE.md       ← optional supporting file
    └── README.md         ← optional, for humans
```

**The SKILL.md file has two parts:**

1. **Frontmatter** (YAML between `---` delimiters) — tells Claude *when* to use this skill
2. **Body** — tells Claude *how* to run it

```markdown
---
name: your-skill-name
description: What this skill does and when to invoke it. Include trigger phrases here.
---

# Your Skill Name

[Instructions for Claude go here]
```

**Key point:** The `description` field in the frontmatter is load-bearing. Claude uses it to decide whether to invoke the skill. A vague description means the skill gets missed or fires at the wrong time. A good description names the job, the trigger phrases, and the context.

**Check question:**
> What is the folder path where skills live, and what is the one required file inside the skill folder?

*(Expected answer: `~/.claude/skills/your-skill-name/SKILL.md`)*

---

### Module 3 — Writing the Frontmatter

**Concept:**
The frontmatter block controls how Claude recognizes and invokes the skill. Three fields matter:

| Field | Required | Purpose |
|-------|----------|---------|
| `name` | Yes | The skill's identifier — matches the folder name and the `/command` |
| `description` | Yes | Natural language description of when to use this skill — Claude reads this to decide if it applies |
| `triggers` | No | Explicit slash commands or phrases that always invoke it |

**Good description (specific):**
```yaml
description: >
  Interview someone to document a process as an SOP.
  Use when the user says "document this process", "make an SOP",
  "interview me about my workflow", or "/sop-builder".
  Also use when a process currently lives in one person's head
  and needs to be written down.
```

**Bad description (vague):**
```yaml
description: Helps with documentation and process stuff.
```

The bad version will be ignored or fire at random. The good version gives Claude enough signal to match intent accurately.

**Key point:** Write the description as if you are telling a smart colleague when to hand off to this specialist. Include the job, the context, and the trigger phrases.

**Check question:**
> Name two things a good `description` field should include that a bad one leaves out.

*(Expected: trigger phrases / slash commands, the specific job or context, examples of when to invoke it)*

---

### Module 4 — Writing the Skill Body

**Concept:**
The body is Claude's instruction manual for running the skill. It should be clear, sequenced, and specific. The most common mistakes:

**Mistake 1: Describing what to do without saying how.**
```
# Bad
Help the user document their process.

# Good
Ask one question at a time. Start with: "What process are we documenting today?"
Wait for the answer before asking the next question.
Work through these sections in order: [list]
```

**Mistake 2: No output spec.**
A skill that doesn't define its output format will produce something different every time. Always specify: what does "done" look like? A table? A markdown file? A numbered list? Name it explicitly.

**Mistake 3: No handling for edge cases.**
What if the user wants to skip ahead? What if their answer is incomplete? What if they ask a question mid-flow? Good skills handle the predictable interruptions.

**Structure that works:**

```markdown
## When to Use
[Precise conditions]

## How It Works
[The process, step by step]

## Rules
[Numbered constraints Claude must follow]

## Output
[Exactly what the finished result looks like]

## Tone
[One paragraph on voice and style]
```

**Key point:** Write the skill body as if you are briefing a capable new employee who has never seen this task before. Leave nothing to inference that you can make explicit.

**Check question:**
> What are the three most common mistakes in skill body writing?

*(Expected: vague instructions without how-to, no output spec, no edge case handling)*

---

### Module 5 — Deploying and Testing

**Concept:**
Deployment is three steps:

1. **Create the folder** at `~/.claude/skills/your-skill-name/`
2. **Drop in `SKILL.md`** (and any supporting files)
3. **Restart Claude Code** — skills are loaded at session start, so a running session won't see a new file until you restart

**Testing your skill:**

Run it with the exact trigger phrase from your description. Then test edge cases:
- What happens if the user gives a very short or vague first answer?
- What happens if the user asks a question mid-flow?
- What happens if they try to skip to the end?

**Iterating:**
Edit `SKILL.md` directly. Restart Claude Code. Test again. Skills are just files — the iteration loop is as fast as you can type.

**Common deployment mistakes:**

| Mistake | Fix |
|---------|-----|
| Skill not triggering | Check that `description` includes the trigger phrase the user typed |
| Skill triggers when it shouldn't | Make the description more specific, narrow the conditions |
| Output looks different every time | Add an explicit output spec to the skill body |
| Supporting files not found | Ensure they are in the same skill folder; reference them by filename in SKILL.md |
| Changes not taking effect | Restart Claude Code — running sessions don't hot-reload |

**Check question:**
> You updated `SKILL.md` but Claude is still running the old version. What is the most likely cause?

*(Expected: Claude Code session was not restarted; skills load at session start)*

---

### Tutorial Complete

You have covered all five modules:

1. What a Claude skill is
2. The folder and file structure
3. Writing the frontmatter
4. Writing the skill body
5. Deploying and testing

**Want to lock it in?** Type "flashcards" for a quick review or "quiz" to test yourself.

---

## Mode 2: Flashcards

Run the cards below one at a time. Show the **front** first. Wait for the user to attempt an answer or say "show answer". Then reveal the **back**. Ask "Got it, or review again?" and track which cards they want to repeat.

After all cards, show a summary: how many they got right, and re-run any they flagged for review.

Say "Card 1 of 20" at the start of each card so they know where they are.

---

**Card 1**
Front: What is a Claude Code skill?
Back: A Markdown file that gives Claude reusable instructions for a specific task, invoked by a slash command or trigger phrase. Skills are stored in `~/.claude/skills/` and loaded at session start.

**Card 2**
Front: Where do skills live on the file system?
Back: `~/.claude/skills/your-skill-name/` — each skill gets its own folder named after the skill.

**Card 3**
Front: What is the only required file inside a skill folder?
Back: `SKILL.md`

**Card 4**
Front: What are the two parts of a `SKILL.md` file?
Back: (1) YAML frontmatter between `---` delimiters, and (2) the body — the instructions Claude follows when running the skill.

**Card 5**
Front: What does the `name` field in frontmatter control?
Back: The skill's identifier — it matches the folder name and becomes the slash command (e.g. `name: sop-builder` → `/sop-builder`).

**Card 6**
Front: What does Claude use the `description` field for?
Back: To decide whether to invoke the skill. Claude reads descriptions to match user intent to the right skill. A vague description causes misses or false triggers.

**Card 7**
Front: Name two things a good `description` field includes.
Back: The specific job the skill does, trigger phrases or slash commands that should invoke it, and the context in which it applies.

**Card 8**
Front: You install a new skill but Claude doesn't recognize it. What do you check first?
Back: Whether you restarted Claude Code — skills are loaded at session start and a running session won't see new files.

**Card 9**
Front: Your skill triggers when it shouldn't. What is the most likely cause?
Back: The `description` is too broad or vague. Narrow it to be more specific about the context and conditions.

**Card 10**
Front: Your skill output looks different every time. What is missing from the skill body?
Back: An explicit output specification — the skill body should define exactly what the finished result looks like (format, structure, file name, etc.).

**Card 11**
Front: What are the three common mistakes when writing a skill body?
Back: (1) Describing what to do without explaining how. (2) No output specification. (3) No handling for edge cases or interruptions.

**Card 12**
Front: What structure works well for a skill body?
Back: When to Use → How It Works → Rules → Output → Tone

**Card 13**
Front: Can you add supporting files to a skill folder?
Back: Yes. Put them in the same skill folder. Reference them by filename in `SKILL.md`. Claude will load them as needed.

**Card 14**
Front: How do you update a skill?
Back: Edit `SKILL.md` directly. Restart Claude Code. Test again. No build step needed.

**Card 15**
Front: A supporting file referenced in your skill isn't being found. What do you check?
Back: That the supporting file is in the same skill folder (not a parent or sibling folder).

**Card 16**
Front: What is the difference between a skill and a system prompt?
Back: A system prompt is set once per session for the whole conversation. A skill is a reusable, file-backed, named instruction set that can be invoked on demand via a trigger.

**Card 17**
Front: What is the purpose of a `README.md` in a skill folder?
Back: Documentation for humans — explains what the skill does, how to install it, and what output to expect. Not read by Claude during execution.

**Card 18**
Front: How do you trigger a skill?
Back: Either by typing its slash command (e.g. `/sop-builder`) or by using natural language that matches the trigger phrases in the `description` field.

**Card 19**
Front: You want Claude to always produce output in a specific Markdown template. How do you ensure this?
Back: Include the template as a supporting file in the skill folder (e.g. `TEMPLATE.md`) and reference it explicitly in the skill body instructions.

**Card 20**
Front: Name three things to test when validating a new skill.
Back: (1) The main trigger phrase fires the skill. (2) Edge cases (vague input, mid-flow questions, skipping ahead) are handled. (3) The output format is consistent across multiple runs.

---

## Mode 3: Quiz

Run all 10 questions in sequence. After each answer, tell the user if they are right or wrong, and give a one-sentence explanation. Track the score silently. At the end, show the score and a review list of any questions missed.

Say "Question X of 10" before each question.

---

**Q1** — Multiple choice
Which file is *required* inside a Claude skill folder?

A) `README.md`
B) `SKILL.md`
C) `config.yaml`
D) `index.md`

*Correct: B. `SKILL.md` is the only required file. All others are optional.*

---

**Q2** — Fill in the blank
Skills are stored at: `~/.claude/______/your-skill-name/`

*Correct: `skills`. Full path: `~/.claude/skills/your-skill-name/`*

---

**Q3** — True or False
You can update a skill and have the changes take effect without restarting Claude Code.

*Correct: False. Skills are loaded at session start. You must restart for changes to take effect.*

---

**Q4** — Multiple choice
Claude is not triggering your skill even though you installed it correctly. The most likely cause is:

A) The skill body is too long
B) The `description` field doesn't match the phrase you typed
C) The folder name has a capital letter
D) You need to register the skill in a config file

*Correct: B. Claude uses the `description` field to match user intent. If the trigger phrase isn't there, the skill won't fire.*

---

**Q5** — Short answer
Name the two parts of a `SKILL.md` file.

*Correct: YAML frontmatter (between `---` delimiters) and the body (the instructions Claude follows).*

---

**Q6** — Multiple choice
Your skill produces different output formats on different runs. What is missing?

A) A `name` field in frontmatter
B) An explicit output specification in the skill body
C) A `triggers` field in frontmatter
D) A supporting template file

*Correct: B. Without an explicit output spec, Claude improvises. Define exactly what the result should look like.*

---

**Q7** — True or False
The `name` field in a skill's frontmatter must match the folder name.

*Correct: True. The name controls the slash command and should match the folder for consistency.*

---

**Q8** — Multiple choice
Which of these is the BEST `description` for a skill that builds SOPs?

A) "Helps with documentation."
B) "Use for process stuff and SOPs."
C) "Interview someone to document a process as an SOP. Use when the user says 'make an SOP', 'document this process', or '/sop-builder'."
D) "SOP builder tool for teams."

*Correct: C. It names the job, the context, and includes explicit trigger phrases.*

---

**Q9** — Short answer
What is one thing a `README.md` in a skill folder is used for, and one thing it is NOT used for?

*Correct: Used for — human documentation (install instructions, what the skill does, example output). NOT used for — Claude's execution instructions (that's SKILL.md).*

---

**Q10** — Scenario
A colleague says: "I wrote a skill but Claude keeps running it when I ask totally unrelated things." What is the most likely fix?

*Correct: The `description` field is too broad. They need to narrow it — add specific conditions, context, and trigger phrases that distinguish when the skill should and should not fire.*

---

### Scoring

After Q10, show:

```
Your score: X / 10

[If 9-10]: Expert. You are ready to build and ship skills.
[If 6-8]:  Solid. Review the questions you missed, then try the flashcards on those topics.
[If 0-5]:  Go back to the Tutorial — you have the right instincts but need the foundation.
```

List each missed question with a one-line explanation of the correct answer.

---

## General Rules

- Never show all questions or all cards at once. One at a time, always.
- Never skip a user's answer without acknowledging it.
- If the user asks a question mid-session, answer it briefly, then offer to resume where they left off.
- If the user wants to switch modes mid-session, let them. Resume at the start of the new mode.
- Keep the tone encouraging and direct. This is a skill worth having — make it feel that way.
