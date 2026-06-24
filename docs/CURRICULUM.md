# AI Trajectory Curriculum: From Process to Deployed Skill

**Designed for:** Professionals and teams using Claude Code who want to move from "asking Claude things" to running repeatable, reliable AI workflows
**Format:** Three sessions, each 60–90 minutes, spaced 3–7 days apart
**Tools required:** Claude Code CLI installed and running
**Skills required:** `sop-builder` and `skill-trainer` installed at `~/.claude/skills/`

---

## The Arc

Most teams use Claude the same way they use a search engine — a question in, an answer out, starting from scratch every time. This curriculum breaks that pattern. By the end of three sessions, the client has a documented process, a deployed Claude skill that runs it, and the mental model to repeat both without help.

```
Session 1          Session 2            Session 3
Process in         Process becomes      Skill goes live
someone's head  →  a documented SOP  →  on the team
                       ↓
                   SOP becomes
                   a deployed skill
```

---

## Session 1 — Document the Process

**Goal:** Turn a process that lives in someone's head into a clean, structured SOP
**Tool:** `/sop-builder`
**Time:** 60–90 minutes
**Output:** A completed `[process-name]-sop.md` file

### Before the session

Ask the client to come prepared with one process in mind. Good criteria:

- It runs at least weekly
- It currently lives in one or two people's heads
- It has at least 5 steps and one judgment call
- Something goes wrong downstream when it is done inconsistently

If they cannot name one in 30 seconds, ask: "What is the thing your team has to ask you specifically because no one else knows how to do it?" That is the process.

### Running the session

Open Claude Code. Type `/sop-builder`. Let the skill run the interview. Your role as facilitator is to:

- Keep the client from answering in generalities ("we just handle it") — push for specifics
- Watch for "it depends" answers and flag them: "that is a decision point, let's capture the rule"
- Note anything the client says is obvious — those are the tacit knowledge gaps the SOP needs most

The interview will work through: process framing, tools and access, prerequisites, the trigger, the happy path step by step, decision points, exceptions, definition of done, and handoffs.

### After the session

Review the generated SOP together. Ask:

- "Is there anything here that would confuse someone running this for the first time?"
- "Which step is most likely to be done wrong?"
- "Which decision point is the hardest to get right?"

Save the file. This is the input to Session 2.

### What the SOP quality tells you

| SOP characteristic | What it means for Session 2 |
|--------------------|----------------------------|
| Clear steps, explicit decision rules | Client is ready — Session 2 can move fast |
| Vague steps, lots of `[TO CONFIRM]` | Spend 15 minutes at the start of Session 2 tightening the SOP before building the skill |
| Very short (under 10 steps, no decisions) | The process may be too simple to skill — find a more complex one, or combine two processes |
| Very long (30+ steps, many branches) | Break it into two skills before Session 2 |

---

## Session 2 — Build the Skill

**Goal:** Turn the Session 1 SOP into a deployed Claude skill the client understands and can maintain
**Tool:** `/skill-trainer`
**Time:** 90 minutes
**Output:** A deployed `~/.claude/skills/[process-name]/SKILL.md` and a client who can explain why every section exists

### Before the session

Client should have:
- Their completed SOP from Session 1
- Claude Code running
- `skill-trainer` installed

### Running the session

Open Claude Code. Type `/skill-trainer`.

The intake will ask what they are working on — they should describe the process from their Session 1 SOP. This anchors the entire session to their real work.

**Recommended path for most clients:** Tutorial → Build Challenge. Skip Flashcards and Quiz on the first pass — those are for reinforcement, not for a first-time build.

If the client says they have some prior experience with Claude skills: Quiz first to find gaps, then targeted review, then Build Challenge.

**During the Build Challenge:** The skill they draft should be built directly from their SOP. The SOP's steps become the happy path. The SOP's decision points become the decision rules in the skill body. The SOP's exceptions become the edge case handling. The SOP's definition of done becomes the output spec.

This is not coincidental — it is the design. The SOP Builder and the Skill Trainer are built as a pair.

### What the Build Challenge output tells you

The quality of the skill draft is your readiness signal for Session 3. Evaluate on four criteria:

| Criterion | Strong signal | Weak signal |
|-----------|--------------|-------------|
| Frontmatter description | Specific job + context + 2+ trigger phrases, no false-trigger risk | Vague, or only the slash command listed |
| Body specificity | Tells Claude *how*, not just *what* | Describes outcomes without specifying steps |
| Output specification | Format, structure, and filename named explicitly | "Produce a summary" with no format |
| Edge case handling | At least 3 predictable interruptions handled | No edge cases, or "handle gracefully" without definition |

**If all four are strong:** Client is ready for Session 3. Deploy the skill now and close the session.

**If one or two are weak:** Work through the specific gaps before ending the session. The skill-trainer's improvement pathway will point to the right flashcards and tutorial module.

**If three or four are weak:** The client needs more time with the tutorial. Schedule a 30-minute review session before Session 3. Do not rush to deploy a skill that will produce inconsistent output — it will undermine trust in the whole approach.

### Deploying at the end of Session 2

Once the skill is solid:

1. Create `~/.claude/skills/[process-name]/` on the client's machine
2. Save the drafted `SKILL.md` there (the skill-trainer's Build Challenge offers to do this)
3. Restart Claude Code
4. Test the primary trigger together — type the slash command, run through a standard case
5. Test one edge case from their SOP

The client should run the trigger themselves, not watch you do it. Hands on keyboard matters.

---

## Session 3 — Deploy and Scale

**Goal:** The skill is live and used; the client can build the next one without help; a plan exists for scaling to the team
**Tool:** The deployed skill from Session 2 + direct Claude Code
**Time:** 60–90 minutes
**Output:** Skill in active use; two or three next processes identified; a team distribution plan

### Part 1 — Test against real inputs (20 minutes)

Run the skill against real cases from the client's actual work, not hypotheticals.

- Run the happy path with a real recent example
- Run the two or three edge cases from the SOP with real examples
- Have a team member who was not in Sessions 1 or 2 trigger the skill cold — if they need to prompt Claude beyond the trigger phrase, the skill body is under-specified

Note any gaps. Edit `SKILL.md` on the spot if needed. Restart. Retest.

### Part 2 — Identify the next processes (20 minutes)

Ask: "Now that you have seen how this works, what are the next two or three processes you would want to skill?"

Use the same criteria as Session 1 — repeatable, lives in someone's head, has decision points. Rank them by:

1. **Impact of inconsistency** — which process, when done wrong, causes the most downstream pain?
2. **Frequency** — which runs most often?
3. **Complexity** — which would benefit most from being explicit rather than implicit?

The top two become the roadmap. The client can run Sessions 1 and 2 on those independently, or you can facilitate.

### Part 3 — Team distribution (20–30 minutes)

A skill that lives on one machine is not a team capability. Discuss:

**Option A — Manual distribution**
Each team member copies the skill folder to their `~/.claude/skills/`. Works for small teams. Requires someone to push updates manually when the skill changes.

**Option B — Shared repository**
Skills live in a Git repository. Team members clone it and symlink or copy to `~/.claude/skills/`. Updates are pulled. Version history is built in. This is the right answer for teams of 3 or more.

**Option C — Onboarding script**
A shell script that clones the skill repository and installs all skills to the correct path. New team members run one command. Best for teams with turnover or frequent onboarders.

For most clients starting out: Option B. Set up a private repository, commit the skill folder, write a one-paragraph install README. That is a 20-minute task that pays off immediately.

### Closing the session

End with three questions:

1. "What would need to be true for you to build the next skill without my help?"
2. "What is the single most valuable process you could skill in the next 30 days?"
3. "Who on your team should be the internal owner of skill development going forward?"

The answers tell you whether the engagement is complete or whether there is a natural extension — either more facilitated sessions, a team workshop, or a retainer for ongoing skill development and review.

---

## Progression Signals at a Glance

| Signal | What it means |
|--------|---------------|
| Client can name a process immediately in Session 1 | High self-awareness — session will move fast |
| SOP has many `[TO CONFIRM]` fields | Process is under-documented; expect Session 2 to start with cleanup |
| Build Challenge skill is strong on first draft | Client has strong systems thinking; can likely self-serve from here |
| Team member triggers the skill cold without confusion | The skill is genuinely complete |
| Client asks "how do I update it when the process changes?" | They are thinking like a skill maintainer — good sign |
| Client asks "can we connect this to our other tools?" | They are ready for agent workflow conversations |

---

## The Natural Extension Conversation

When a client asks "can we connect this to our other tools?" or "can this run without me triggering it?" — that is the transition from skills to agents. Skills are Claude following instructions on demand. Agents are Claude acting autonomously, connected to systems, triggering on events rather than slash commands.

That conversation is the beginning of a different engagement. The curriculum above is the prerequisite for it — you cannot build a reliable agent on top of a vague process. The SOP Builder and Skill Trainer ensure the process is solid before anything is automated at scale.

---

## Files in This Repository

```
~/.claude/skills/
├── sop-builder/
│   ├── SKILL.md          ← the interview protocol
│   ├── SOP_TEMPLATE.md   ← the output format
│   └── README.md
└── skill-trainer/
    ├── SKILL.md          ← tutorial, flashcards, quiz, build challenge
    └── README.md

docs/
├── CURRICULUM.md                    ← this file
├── SOP-claude-skill-development.md  ← SOP for building skills
└── LEARNING-LOG.md                  ← session learning log
```

---

## Quick Reference: Session Checklist

**Session 1**
- [ ] Client has identified a process (repeatable, 5+ steps, has judgment calls)
- [ ] `/sop-builder` run to completion
- [ ] SOP reviewed and saved as `[process-name]-sop.md`
- [ ] Gaps and `[TO CONFIRM]` items noted for Session 2

**Session 2**
- [ ] `skill-trainer` installed and running
- [ ] Intake completed with client's real process named
- [ ] Tutorial completed (or skipped based on experience level)
- [ ] Build Challenge completed; skill evaluated on 4 criteria
- [ ] Skill deployed to `~/.claude/skills/[process-name]/`
- [ ] Primary trigger tested; one edge case tested
- [ ] Client ran the trigger themselves

**Session 3**
- [ ] Skill tested against real inputs (not hypotheticals)
- [ ] Cold-trigger test with a team member not in Sessions 1 or 2
- [ ] Next two or three processes identified and ranked
- [ ] Team distribution method chosen and set up
- [ ] Three closing questions asked and answered
- [ ] Next engagement or handoff defined
