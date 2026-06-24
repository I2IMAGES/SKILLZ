# Learning Log: Claude Skill Development Session

**Session date:** 2026-06-24
**Scope:** Built, reviewed, critiqued, and improved two Claude Code skills (sop-builder, skill-trainer) from uploaded originals through three iterations

---

## What We Started With

Three files from the original author (Robin van Veen / Arcgent):
- `README2.md` — marketing and install documentation
- `SKILL.md` — the SOP Builder interview protocol
- `SOP_TEMPLATE.md` — the output template

The original skill was well-conceived: strong interview discipline (one question at a time, hunt for branches, use their words), a clean template, and a punchy README. The weaknesses were structural rather than conceptual.

---

## Iteration 1: Improve the Original SOP Builder

### What was wrong

| Issue | Root cause | Effect in use |
|-------|-----------|---------------|
| No opening move defined | Assumed Claude would infer the start | First question varies by session |
| Tools/access was Block 6 of 8 | Original ordering was arbitrary | Claude asked "how do you do it in [tool]?" before knowing what tools exist |
| "Confirm before you write" was Rule 7 but not enforced in the flow | Rule and flow were written independently | Confirm step got skipped in practice |
| No early-exit handling | Didn't anticipate users wanting to skip | Session broke if user said "just write the draft" |
| "Write the SOP using SOP_TEMPLATE.md" with no inline structure | Assumed the file was always in context | Output format undefined if file not loaded |
| No completion trigger | No explicit "interview is done" signal | Interview could continue indefinitely |
| Steps table had blank placeholder rows | Template assumed user would know to fill them | First-time users confused about what rows 2-3 were for |
| No Prerequisites section in template | Omission | Upstream dependencies never captured |
| No Version field or Revision History in template | Omission | No way to know if you're reading the current version |
| README said "reply to the newsletter" | No newsletter referenced | Confusing for anyone not from that newsletter |

### What was added

- **Block 0 (Open):** Explicit first question: "What process are we documenting today?" — anchors everything
- **Tools moved to Block 2:** Knowing the toolset early makes every subsequent question more specific
- **Block 3 (Prerequisites):** What must be true before the trigger fires
- **Rule 7 enforcement in flow:** Each block explicitly applies Rule 7 (confirm before advancing)
- **Rule 8 (Replay the happy path):** Made explicit as a named rule, not buried in Block 3 prose
- **Early-exit handling:** Produce draft with `[TO CONFIRM]` markers if user skips interview
- **Explicit completion trigger:** "When all blocks are confirmed, write the SOP immediately"
- **File-save instruction with filename convention:** `[process-name]-sop.md`
- **Prerequisites section in template**
- **Version field and Revision History table in template**
- **Example rows in steps table marked as examples** (not required content)
- **Related SOPs field in template header**
- **Escalation contact in exceptions** ("ask a human" → "which human")
- **README:** Full path spelled out; "works best for" guidance; removed newsletter reference

---

## Iteration 2: Build the Skill Trainer

### Design decisions

The request was "build an interactive quiz, flashcard, tutorial." The naive implementation would be: write some questions, write some cards, present them in sequence. That produces something that looks like learning but isn't.

The decision was to anchor the design in two adult learning frameworks instead:

**Andragogy (Knowles):** Adults learn best when the content connects to a real problem they are trying to solve, when they are treated as self-directed, when their prior experience is acknowledged, and when they know *why* each piece of content matters before they receive it.

**Heutagogy (Hase & Kenyon):** Goes further — self-determined learning where the learner identifies their own needs, reflects on their assumptions (not just the content), and produces something real (capability) rather than just answering questions (competency).

### What each design choice was for

| Design element | Framework it serves | What it prevents |
|----------------|--------------------|-----------------:|
| 3-question intake before any content | Andragogy (experience, readiness, problem-centered) | Generic session that wastes time on what they already know |
| Routing logic based on experience level | Andragogy (self-concept, self-directed) | Forcing a beginner tutorial on someone who just needs a refresher |
| "Why it matters" opener in each module | Andragogy (need to know) | Abstract content with no relevance hook |
| Reflection prompts at each module end | Heutagogy (reflection as core) | Knowledge that stays abstract and never connects to their situation |
| Double-loop question at tutorial close | Heutagogy (double-loop learning) | Unexamined assumptions that will cause problems at deploy time |
| Confidence rating before each flashcard | Heutagogy (metacognition) | Learners who feel confident about wrong things — the hardest gap to close |
| Topic-tagged quiz questions | Diagnostic design | Scores that can't tell you *what* to fix |
| Build Challenge capstone | Heutagogy (capability over competency, learner-generated content) | Learners who pass the quiz but can't write a real skill |
| Session close block | Completion psychology | Sessions that trail off with no takeaway named |

---

## Iteration 3: Audit and Correction

### What the audit found

**11 issues across 5 files.** Severity breakdown:

**Critical (would cause the skill to fail silently):**
1. Quiz fundamentals topic had zero questions tagged to it but appeared in the scoring diagnostic — dead branch that sends users on an improvement pathway for a topic they were never tested on
2. "Targeted mini-quiz" promised in the improvement pathway but no mini-quiz question banks defined anywhere — Claude would have to improvise or silently omit

**Significant (would produce incorrect or misleading output):**
3. Card 7 said "name two things" but the answer listed three — a complete answer would be marked wrong
4. Q4 distractor C ("folder name has a capital letter") is actually a real failure mode on Linux — presenting it as an obviously wrong answer teaches the wrong lesson
5. `triggers` field described in Module 3 but difference from `description` trigger phrases not explained — learners don't know when to use which

**Minor (friction, confusion, redundancy):**
6. No session close block — the skill just ended at Build Challenge Step 6
7. Block 2 still said "(ask this early)" even though it IS Block 2 now — leftover note from when it was Block 6
8. Body and Deployment topics each had one quiz question — the diagnostic table implied more resolution than a single question can provide; no caveat
9. Steps table example rows had no label marking them as examples
10. skill-trainer README had duplicate footer paragraph about companion skill
11. sop-builder README said "The file is saved as..." implying automatic behavior — it only happens if user accepts the offer

### What was corrected

All 11 fixed. The two critical issues required structural changes:
- Fundamentals improvement pathway now routes to Tutorial Module 1 + Flashcards 1, 16, 18 + Tutorial check questions (not quiz questions, since none exist at that topic level)
- "Targeted mini-quiz" language replaced with "retry the specific questions you missed" — honest about what's defined, no improvisation required

### What was not changed and why

- The quiz has only 1 body question and 1 deployment question. Adding questions to fix the imbalance was considered but rejected — 10 questions is the right length for a scored quiz; better to caveat the diagnostic than inflate the quiz. Caveat added.
- The skill-trainer does not hot-reload. This is a platform constraint, not a skill design choice. Documented in the SOP but not fixable here.

---

## Key Lessons Learned

### On skill design

1. **The `description` field is the most important thing in a skill and the most commonly done wrong.** It controls when the skill fires. A bad description is invisible — it doesn't error, it just misses or misfires. Every skill designer needs to test their description against: (a) the exact trigger phrase, (b) a paraphrase of the trigger, (c) something unrelated that might accidentally match.

2. **Output specs prevent the most common class of skill failures.** Without a specified output format, Claude improvises. The improvised format varies by session, by model version, by conversation history. An explicit spec — format, structure, filename, audience — is what makes a skill reliable rather than merely functional.

3. **Rules and flow must be written together.** In the original sop-builder, Rule 7 (confirm before advancing) existed in the Rules section but was not referenced in the Interview Flow blocks. The rule was functionally invisible — the flow didn't apply it. Any rule that matters needs to be referenced at the point in the flow where it applies.

4. **Supporting files must be in the skill folder.** Not a parent directory, not a sibling. The same folder. This is easy to get wrong and has no error message — Claude just can't find the file.

5. **Restart is not optional.** Skills load at session start. This is the single most common cause of "my change isn't working" issues. It should be step 1 in any troubleshooting checklist, and it was underemphasized in the original material.

### On adult learning design

6. **A quiz that returns a total score teaches nothing about what to study.** The score is the least useful output of a quiz. The topic breakdown, the specific missed questions, and the improvement pathway are the useful outputs. Design scoring around those, not around the number.

7. **Confidence tracking catches a different class of problem than correctness tracking.** A learner who gets something wrong and knows they don't know it will study it. A learner who gets something wrong and thought they knew it — that's the dangerous gap. Surfacing confidence mismatches is worth the additional friction of asking for a rating before each card.

8. **Reflection prompts only work if they are not rhetorical.** A prompt that says "Think about X" and immediately moves on is decoration. A prompt that says "Think about X — what comes up for you?" and then waits for an answer and responds to it is learning. The difference is whether Claude treats the prompt as a pause or a question.

9. **The double-loop question is the most valuable moment in a tutorial.** "What did you assume before that turned out to be wrong?" surfaces the hidden layer — not what the learner didn't know, but what they thought they knew incorrectly. These wrong assumptions are what cause problems at deploy time. No other question gets at them.

10. **Adults need a reason before content, not after.** The original tutorial delivered content and then (maybe) explained why it mattered. The revised version leads with why, then delivers content. The why creates a hook that the content hangs on. Without the hook, the content is just information.

### On the development process itself

11. **Writing rules and writing flow separately creates drift.** Rules in one section, flow in another, written at different times — they diverge. The fix is to reference rules explicitly by number at the points in the flow where they apply.

12. **Dead branches in scoring logic are invisible until you map the data.** The fundamentals topic in the diagnostic looked fine until I counted which quiz questions were actually tagged `[topic: fundamentals]` — zero. The diagnostic promised insight it couldn't deliver. This kind of error only surfaces when you trace every path end-to-end.

13. **"Offer to X" is not the same as "X happens."** The sop-builder README said the file is saved to disk. It is only saved if the user accepts the offer. The difference between "offer" and "do" matters for user expectations. Language should match the actual behavior.

---

## Files Changed in This Session

| File | Status | Summary of changes |
|------|--------|--------------------|
| `sop-builder/SKILL.md` | Improved from original | Added Block 0, moved tools to Block 2, added Block 3 Prerequisites, added Rules 7+8, added early-exit handling, added completion trigger, added file-save convention. Removed "(ask this early)" note in Block 2 (audit). |
| `sop-builder/SOP_TEMPLATE.md` | Improved from original | Added Prerequisites section, Version field, Related SOPs field, Revision History table, escalation contact. Marked example rows as examples (audit). |
| `sop-builder/README.md` | Improved from original | Full path in install step, "works best for" guidance, example output section, removed newsletter reference. Fixed "saved as" language (audit). |
| `skill-trainer/SKILL.md` | New file | Full tutorial (5 modules with reflection prompts + double-loop question), flashcard deck (20 cards with confidence tracking), quiz (10 questions with topic tags and diagnostic scoring), Build Challenge capstone, intake + routing, improvement pathway, session close block. Fixed Q4 distractor, Card 7 "two vs three", fundamentals dead branch, mini-quiz promise, triggers explanation (audit). |
| `skill-trainer/README.md` | New file | Mode table, what makes this different, andragogy/heutagogy principles mapped, install instructions, companion skill pointer. Removed duplicate footer (audit). |
| `docs/SOP-claude-skill-development.md` | New file | Full SOP for building and deploying a Claude Code skill, following the SOP_TEMPLATE format. |
| `docs/LEARNING-LOG.md` | New file | This document. |
