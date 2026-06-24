# SOP: [Process Name]

**Owner:** [role / person]
**Version:** 1.0
**Last updated:** [date]
**Frequency:** [how often it runs]
**Average time:** [duration]
**Why it matters:** [what breaks downstream if done wrong or skipped]
**Related SOPs:** [links or names of upstream / downstream SOPs, if any]

---

## Prerequisites

What must be true or done before this process can start.

- [condition or upstream dependency]
- [access or resource that must be provisioned]

---

## Trigger

What starts this process and how you know it is time to begin.

- **Signal:** [the event or condition that kicks it off]
- **Input arrives via:** [email / form / system / person]
- **Input looks like:** [format and what it contains]

---

## Steps (the happy path)

| # | Step | Tool / system | What to look at | Output of this step |
|---|------|---------------|-----------------|---------------------|
| 1 | *(example) Open the incoming request form* | *(example) Helpdesk portal* | *(example) All required fields are filled* | *(example) Request confirmed ready to action* |
| 2 | [your step] | [tool] | [what to check] | [result] |
| 3 | [your step] | [tool] | [what to check] | [result] |

---

## Decision Points

For each judgment call, the explicit rule so anyone can make the same choice.

- **Decision:** [what is being decided]
  - **Choose A when:** [condition]
  - **Choose B when:** [condition]
  - **Info needed to decide:** [what must be in front of you]

---

## Exceptions and Edge Cases

The cases that do not fit the normal flow, and what to do.

- **When [situation]:** [what to do]
- **Most common failure:** [what goes wrong] → [the fix]
- **Stop and escalate when:** [the trigger] — escalate to [name / role]

---

## Tools, Access, and Data

- **Tools used:** [list]
- **Access required:** [logins / permissions / licenses]
- **Where the data lives:** [before → during → after]

---

## Definition of Done

- **Finished when:** [clear completion criteria]
- **A good output looks like:** [quality bar]
- **Checked by:** [who reviews, if anyone] — sends it back if [reason]

---

## Handoffs

- **Receives from:** [who / what provides the input]
- **Passes to:** [who] who expects [what, in what format]
- **Most common reason for rework:** [reason]

---

## Notes for Automation

- **Pure mechanics (agent can own):** [steps that are deterministic and tool-based]
- **Needs human judgment (keep in the loop):** [steps with real decision-making or relationship context]
- **Open questions to resolve before automating:** [anything unclear or unconfirmed during the interview]

---

## Revision History

| Version | Date | Changed by | What changed |
|---------|------|------------|--------------|
| 1.0 | [date] | [name] | Initial version |
