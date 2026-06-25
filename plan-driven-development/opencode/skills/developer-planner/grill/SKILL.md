---
name: grill
description: Interview the user one question at a time to resolve every design decision, then present three approaches with trade-offs and a recommendation, confirm there's nothing to add, then write a design doc and a plan. Planning only — never implement. Use to stress-test a plan before building, or on any 'grill' trigger.
---

gril with these step

- read `Output boundary — planning only section` first
- Grilling
- After the interview — present three approaches
- Last call — anything to add?
- Design doc

that you will found details below.

# Grilling

Interview the user relentlessly until every branch of the design tree is resolved.

- Ask **one question at a time**; wait for the answer before the next. Multiple at once is bewildering.
- For each question, give your recommended answer + one-line rationale.
- If the codebase can answer it, explore instead of asking: read `AGENTS.md` first for the architecture/structure and let it point you to the relevant folder, then read only those files — read more only if needed. (if you decide to delegate to find the context by task just forward about how to read the file in this section too.)
- Resolve dependencies in order: settle upstream decisions before downstream ones.
  Stop interviewing when an engineer with zero context couldn't misinterpret the design and nothing material is left to guess.

## After the interview — present three approaches

When every material decision is resolved, do **not** jump straight to the design doc. First present **three distinct approaches** to the problem:

- Give each a short name + one-line summary of the solution's shape.
- For each, lay out the trade-offs: what it optimizes for and what it sacrifices (complexity, performance, time-to-build, flexibility, operational cost, blast radius).
- **Recommend one** with a one-line rationale tied to the constraints surfaced in the interview.
- **STOP and let the user choose.** Don't proceed until they pick — they may take your recommendation, another, or a hybrid. If they pick a hybrid, restate the merged approach in one line and confirm before continuing.

## Last call — anything to add?

Once the approach is chosen, **STOP and ask the user: "Anything else to add or change before I write the design doc and plan?"**

- This is the final chance to fold in context cheaply, before the doc and plan lock decisions down.
- If they add anything, integrate it — and if it materially affects the design, briefly say how, and re-confirm the chosen approach still holds.
- Proceed only on an explicit "nothing else" / go-ahead.

## Design doc

Write a design doc to `docs/specs/YYYY-MM-DD-<feature>.md` **around the chosen approach**, capturing: Goal, Scope (in/out), the chosen approach + why it won over the other two, every Decision + rationale, Interfaces/contracts (exact types, signatures, routes, schemas), Dependencies/order, Open risks. No "TBD", no implicit decisions.
Immediately continue with the writing-plans skill using this design doc as input — don't ask. This auto-continue applies **only** up to and including the plan document.

## Output boundary — planning only

This skill ends at the plan. Do **not** implement.

- Do not write, edit, scaffold, or generate any implementation code, config, schemas-as-code, or migrations.
- Do not run build, install, codegen, or "let me just start" commands.
- The only files this skill creates are the design doc and the plan document.
- After the plan is written, STOP. End with a short summary and an explicit prompt for the user to review/approve.
- Begin implementation only in a later turn, after the user explicitly says to proceed.
