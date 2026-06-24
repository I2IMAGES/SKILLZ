# Skill Trainer (free Claude Code skill)

An interactive tutor that teaches you how to build and deploy Claude Code skills correctly — through a structured tutorial, a 20-card flashcard deck, and a 10-question scored quiz.

Use it to onboard new team members, test your own understanding, or run a quick refresher before shipping a new skill.

## What you get

**Tutorial mode** — five modules covering:
1. What Claude skills are and why they exist
2. The folder and file structure
3. Writing effective frontmatter (especially the `description` field)
4. Writing clear, consistent skill bodies
5. Deploying, testing, and iterating

**Flashcard mode** — 20 cards, one at a time. You answer, it scores, it re-runs the ones you flagged for review.

**Quiz mode** — 10 questions (multiple choice, true/false, short answer, scenario). Scored at the end with a breakdown of what to revisit.

## Install

1. Make sure you have [Claude Code](https://www.anthropic.com/claude-code) installed.
2. Copy the `skill-trainer` folder into your Claude skills directory:
   ```
   ~/.claude/skills/skill-trainer/
   ```
   If `~/.claude/skills/` does not exist yet, create it.
3. Restart Claude Code.
4. Type `/skill-trainer` or "teach me how to build a Claude skill".

## Usage

When the skill starts, pick a mode:

- `tutorial` — new to skills or want a solid foundation
- `flashcards` — quick review, good before building something
- `quiz` — test yourself, get a score
- `all` — runs all three in sequence

You can switch modes mid-session or ask questions at any point.

## Files

- `SKILL.md` — the trainer skill Claude runs
- `README.md` — this file

## Who this is for

- Developers building their first Claude skill
- Team leads onboarding people to a skills-based workflow
- Anyone who has built skills before but wants to tighten their practice

---

Built to accompany the [SOP Builder skill](../sop-builder/). If you want to document your processes and then build skills to run them, start with SOP Builder, then use this to understand how the result gets deployed.
