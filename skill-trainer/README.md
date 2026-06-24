# Skill Trainer (free Claude Code skill)

An interactive tutor that teaches you how to build and deploy Claude Code skills — through a structured tutorial, a 20-card flashcard deck, a 10-question diagnostic quiz, and a live build challenge where you write a real skill and get critique.

Designed for adult learners: it starts by asking what you are working on, connects every concept to your goal, and builds toward capability (can you apply it in a new situation?) not just recall (can you answer a question?).

## Modes

| Mode | Best for |
|------|----------|
| **Tutorial** | New to skills, want a solid foundation |
| **Flashcards** | Review before building something, reinforce weak spots |
| **Quiz** | Test yourself — get a topic-level diagnostic, not just a score |
| **Build Challenge** | Apply what you know by drafting a real skill and getting critique |
| **all** | Run all four in sequence |

## What makes this different from a standard quiz

**Intake first.** Before anything starts, the trainer asks what you are working on, what you already know, and what your biggest uncertainty is. Everything that follows is connected to those answers.

**Diagnostic scoring.** The quiz does not just give you X/10. It breaks your score down by topic (structure, frontmatter, body, deployment) and gives you a specific improvement pathway — which module to re-read, which flashcards to re-run, which questions to retry.

**Confidence tracking.** In flashcard mode, you rate your confidence before answering. Cards where you said "confident" but got it wrong are flagged separately — those are the highest-priority gaps.

**Reflection prompts.** Each tutorial module ends with a reflection question: not a test, but a prompt to connect the concept to your own situation. These are not skipped.

**Double-loop question.** At the end of the tutorial: "What did you assume about skills before today that turned out to be wrong?" This surfaces hidden assumptions and is where the deepest learning tends to happen.

**Build Challenge capstone.** You write a real `SKILL.md`. The trainer critiques your frontmatter and body against specific criteria, asks for a revision, and offers to save the finished file to the right path so you can deploy it immediately.

## Andragogy and Heutagogy principles applied

**Andragogy (Knowles):**
- Need to know — every module opens by explaining why it matters to what you said you are building
- Self-concept — learner picks their path; trainer supports, does not override
- Experience — intake asks what you already know; content adapts to that
- Problem-centered — anchored to the skill you want to build, not abstract concepts
- Motivation — connects to a real deliverable, not a test score

**Heutagogy (Hase & Kenyon):**
- Self-determined — learner routes their own session; can switch modes, skip, or go deep on one topic
- Double-loop learning — the double-loop question at tutorial end explicitly targets assumptions
- Capability not just competency — Build Challenge requires applying knowledge to a novel situation, not recalling it
- Learner-generated content — the capstone produces something real that the learner authored
- Reflection as core — reflection prompts are required at every module, not optional add-ons

## Install

1. Make sure you have [Claude Code](https://www.anthropic.com/claude-code) installed.
2. Copy the `skill-trainer` folder into your Claude skills directory:
   ```
   ~/.claude/skills/skill-trainer/
   ```
   If `~/.claude/skills/` does not exist yet, create it.
3. Restart Claude Code.
4. Type `/skill-trainer` or "teach me how to build a Claude skill".

## Files

- `SKILL.md` — the trainer skill Claude runs
- `README.md` — this file

## Companion skill

Use this alongside the [SOP Builder skill](../sop-builder/). SOP Builder documents the process; this skill teaches you how to turn that process into a deployable Claude skill.

---

Built to go with the SOP Builder. If you want to document processes and then build skills to run them, start with SOP Builder, then come here to learn the deployment side.
