# SOP Builder (free Claude Code skill)

Most processes do not live in a document. They live in someone's head, and the only way to run them is to "ask Sarah".

This skill fixes that. It interviews the person who actually does the work, one question at a time, and turns their answers into a clean SOP that a teammate, a new hire, or an AI agent can follow without guessing.

A good SOP is the thing that has to exist BEFORE you automate. Automate a vague process and you just get a faster way to be wrong, at scale, with your logo on it.

## What you get

- A structured interview that pulls the process out of someone's head
- Active hunting for the "we just know" parts that normally get lost
- A finished SOP in a clean template, with an automation-ready breakdown at the end (which steps an agent can own, which need a human in the loop)

Works best for processes with at least 5 steps and at least one decision point. Short, fully mechanical tasks don't need an interview — just write the steps. This tool earns its keep on the messy, judgment-heavy ones.

## Install

1. Make sure you have [Claude Code](https://www.anthropic.com/claude-code) installed.
2. Copy the `sop-builder` folder into your Claude skills directory:
   ```
   ~/.claude/skills/sop-builder/
   ```
   If `~/.claude/skills/` does not exist yet, create it.
3. Start Claude Code and type `/sop-builder` (or just say "interview me to document my process").

That is it. Pick a process that currently lives in someone's head and let it interview you.

## Files

- `SKILL.md` — the interview protocol Claude follows
- `SOP_TEMPLATE.md` — the output format
- `README.md` — this file

## Example output

After the interview, you get a filled SOP covering:
- Trigger and prerequisites
- Step-by-step happy path (in a table)
- Decision rules, exceptions, and escalation contacts
- Tools, access, and data locations
- Definition of done and quality bar
- Handoffs in and out
- Automation candidates and open questions

If you choose to save, the file is written as `[process-name]-sop.md` in your working directory.

---

Built by [Robin van Veen](https://arcgent.ai) at Arcgent. We build AI agents that automate real business processes. If this skill surfaced a process worth automating, that is exactly what we do next.
