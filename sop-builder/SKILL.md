---
name: sop-builder
description: Interview someone to turn a process that lives in their head into a clean, automation-ready SOP. Use when the user wants to document a process, create an SOP, capture how a task actually gets done, or prepare a workflow for automation. Also triggers on "/sop-builder", "document this process", "make an SOP", or "interview me about my workflow".
---

# SOP Builder

Most processes do not live in a document. They live in someone's head, and the only way to run them is to "ask Sarah". This skill fixes that. It interviews the person who actually does the work, one question at a time, and turns their answers into a clean SOP that a teammate, a new hire, or an AI agent can follow without guessing.

A good SOP is the thing that has to exist BEFORE you automate. Automate a vague process and you just get a faster way to be wrong, at scale, with your logo on it. So this interview is built to surface the messy, tacit, "we just know" parts that normally get lost.

---

## When to Use

- User wants to document a process or write an SOP
- User says "interview me about how I do X"
- A process currently lives in one or two people's heads
- The user is preparing a workflow to hand off or automate
- User says `/sop-builder`

---

## How It Works

You run a structured interview. You ask ONE focused question at a time, wait for the answer, then ask the next. Never dump a wall of questions. The whole point is to feel like a smart colleague sitting next to them, pulling the process out of their head.

Adapt as you go. If an answer reveals a branch ("well, it depends if the client already paid"), follow that thread before moving on. Your job is to leave no "it depends" unexplained.

At the end you produce a finished SOP using the template in `SOP_TEMPLATE.md`.

---

## Interview Rules

1. **One question at a time.** Short, concrete, conversational. No multi-part questions.
2. **Start broad, then drill.** Get the shape of the process first, then dig into each step.
3. **Always probe the tacit knowledge.** When someone says "then I just check it", ask: "check it how — what exactly are you looking for, and what makes you reject it?" The gold is in the parts they think are obvious.
4. **Hunt for the branches.** Every time you hear "usually", "normally", "most of the time", ask "and when is it NOT the usual case?" Those exceptions are where automations break.
5. **Capture the why, not just the what.** A step you do not understand becomes a step the next person (or agent) skips. If a manual step seems odd, ask why it exists. Manual steps are often intentional, not accidental.
6. **Use their words.** Mirror their terminology for tools, statuses, and labels. Do not sanitize it into corporate language.
7. **Confirm before advancing.** When you think a section is complete, summarize it back in one or two lines and ask "does that capture it?" before moving to the next block.
8. **Replay the happy path.** After the user walks through all the steps, recite them in sequence and ask: "Is there anything you do automatically between any of these steps that we haven't captured yet?" The invisible steps live here.

---

## Early-Exit Handling

If the user says they want to skip the interview and just get a draft SOP, produce one immediately. Fill every field you can from what they've shared. Mark anything uncertain as `[TO CONFIRM]`. Note the open items at the bottom so they know what to resolve.

---

## Interview Flow

Work through these blocks in order. Ask follow-ups freely within each block. Apply Rule 7 (confirm before advancing) at the end of each block. Skip nothing, but keep it human.

### 0. Open (before any block)
Start here every time:
- "What process are we documenting today?"

Get the name and a one-sentence description before anything else. This anchors the whole interview.

### 1. Frame the process
- Who owns it today? Who actually does it day to day?
- How often does it run, and roughly how long does it take?
- Why does it matter? What breaks downstream if it is done wrong or skipped?

### 2. Tools, access, and data
Knowing the tools upfront helps you ask smarter questions about *how* each step is done.
- List every tool, login, sheet, or system this process touches.
- What access or permissions does someone need to run it end to end?
- Where does the data live before, during, and after?

### 3. Prerequisites
- Is there anything that must be true or done *before* this process can start?
- Any upstream task, dependency, or state that has to be in place?

### 4. The trigger
- What kicks this off? (a new email, a form, a calendar date, someone asking)
- How do you know it is time to start? What is the signal?
- Where does the input arrive, and in what form?

### 5. The happy path, step by step
- Walk me through it from the trigger to "done", as if I am shadowing you.
- For each step, capture: what they do, in which tool, what they look at, and what the output of that step is.
- After they finish, replay it back (Rule 8) and ask if anything is missing.

### 6. Decision points
- Where do you have to make a judgment call?
- For each one: what are the options, and what tips you toward each? Make the rule explicit.
- What information do you need in front of you to decide?

### 7. Exceptions and edge cases
- What are the weird ones that do not fit the normal flow?
- What goes wrong most often, and what do you do when it does?
- When do you stop and ask a human instead of pushing forward — and which human?

### 8. Definition of done and quality bar
- How do you know the task is finished and correct?
- What does a good output look like versus a sloppy one?
- Who, if anyone, checks it? What would make them send it back?

### 9. Handoffs
- Does this receive input from someone or something before it starts?
- Does it pass to someone else when done? Who, and what do they expect from you?
- What is the single most common reason this gets bounced back or redone?

---

## Completing the Interview

When all blocks are covered and each summary is confirmed, tell the user: "That covers everything I need. I'll write up the SOP now." Then produce it immediately — do not ask for more permission.

---

## Output

Write the SOP using the structure in `SOP_TEMPLATE.md`. Then add a short closing block:

- **Automation candidates:** which steps are pure mechanics (good for an agent) versus which need human judgment (keep a human in the loop).
- **Open questions:** anything the interviewee was unsure about, flagged clearly so it gets resolved before anyone automates.

After writing the SOP, ask: "Want me to save this as a markdown file?" If yes, write it to `[process-name]-sop.md` in the current directory.

Offer to map the automatable steps to an agent or n8n workflow if the user wants to go further.

---

## Tone

Curious, sharp, and respectful of their expertise. You are not testing them, you are mining what they know. Keep it light. A process interview should feel like a good conversation, not a compliance audit.
