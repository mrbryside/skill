---
name: minimal-grill
description: For small tasks — grill the design, present three approaches with trade-offs and a recommendation, confirm there's nothing to add, plan inline, get approval, then execute, all in one pass with no spec or plan files written. Execution begins only after the user approves the inline plan. Use for quick changes that don't warrant the full grilling → writing-plans → executing-plans flow.
---

# Minimal Grill

One skill, in-session, no files written. Grill the design, pick an approach, hold the plan inline, get sign-off, then execute.
**Announce:** "I'm using the minimal-grill skill."

minimal-gril with these step

- Grill
- Present three approaches
- Last call — anything to add?
- Plan inline
- Approval gate — plan before implementation
- Ask how to schedule
- Ask who executes
- Ask more verification
- Execute
- Finish
- prepare yourself to `Edit & Adjust` section if user want to edit & adjust

that you will found details below.

## 1. Grill

Interview the user **one question at a time** (wait for each answer) until every material decision is resolved. For each, give your recommended answer + one-line rationale. If the codebase can answer it, read `AGENTS.md` first for architecture/structure, jump to the relevant folder, and read only what you need. Stop when nothing material is left to guess. (if you decide to delegate to find the context by task just forward about how to read the file in this section too.)

## 2. Present three approaches

When every material decision is resolved, do **not** jump straight to the plan. First present **three distinct approaches**:

- Give each a short name + one-line summary of the solution's shape.
- For each, lay out the trade-offs: what it optimizes for and what it sacrifices (complexity, performance, time-to-build, flexibility, operational cost, blast radius).
- **Recommend one** with a one-line rationale tied to the constraints surfaced in the interview.
- **STOP and let the user choose.** Don't proceed until they pick — recommendation, another, or a hybrid. If they pick a hybrid, restate the merged approach in one line and confirm before continuing.

## 3. Last call — anything to add?

Once the approach is chosen, **ask: "Anything else to add or change before I lay out the plan?"** This is the cheap moment to fold in late context before planning takes shape. If they add something that shifts the design, re-confirm the chosen approach still holds. Proceed on "nothing else" / go-ahead.

## 4. Plan inline

Plan **around the chosen approach**. Do **not** write any file. Lay out the tasks as a TodoWrite list — bite-sized, with clear file boundaries so independent tasks can run in parallel. For each task note exact files, a **Depends on** (task numbers or "none" = parallel-safe), and TDD steps (failing test → implement minimally → pass → commit) as **short pseudocode** — enough to convey intent, not full code. No vague placeholders; name exact files/functions/types.

## 5. Approval gate — plan before implementation

Present the inline plan and **STOP**. Do not write or edit any implementation code until the user explicitly chooses to proceed. Ask: **"Ready to proceed? I can start implementing — or tell me what to edit or add and I'll revise the plan first."** Surface any open risks or assumptions alongside.
This is a real fork, not a yes/no. Three valid responses:

- **Proceed** — an explicit go-ahead unlocks step 6.
- **Edit / add** — the user names changes; revise the inline plan, re-check it for placeholder-free, consistent naming, then return to this gate and ask again. Loop until they're satisfied. An edit request is **not** consent to proceed.
- **Hold** — the user wants to stop here; since nothing is written to disk, just end without implementing and note the plan was inline-only.
  Up to this point, no implementation has happened — only the interview, the approach choice, and the inline plan.

## 6. Ask how to schedule

Ask the user — **parallel or sequential?**

- Parallel — follow the plan's **Depends on** markings: dispatch every task whose dependencies are already done concurrently, keep dependent tasks ordered. (If the plan marks nothing parallel-safe, fall back to sequential.)
- Sequential — one task at a time, in order.

## 7. Ask who executes

Ask the user — **qwen-agent or current agent?**

- qwen-agent — load the `qwen-agent` skill and delegate each task (or each parallel group, one per qwen) to it; you orchestrate, qwen does the work.
- current agent — delegate each task (or each parallel group, one per yourself).

## 8. Ask more verification

Ask the user - \*\*do you need another verification just like `browser use validation` , `script validation`
and when you receive answer you will using this verification with existing verification that you need to do.

## 9. Execute

IMPORTANT: forward below to delegated agents or yourself if you not delegate.
For each task: mark in_progress → follow its steps exactly → run the verifications → mark completed. Don't skip verifications.
**Update two artifacts as you go, not just one:**

- The TodoWrite list (in-memory progress).
- **The plan file itself** — tick the task's checkbox (`- [ ]` → `- [x]`) in `docs/plans/<plan>.md` immediately after its verifications pass. This is the durable record that survives the session.
  **In qwen-agent mode, you (the orchestrator) own the plan file.** The delegated qwen does the task and returns; it does **not** edit the plan. After each qwen task returns and its verifications pass, _you_ tick the checkbox. Never assume the delegate updated it.
  **Stop and ask** when blocked: missing dependency, repeated test failure, unclear instruction, or critical gap. Don't guess. Never start on main/master without explicit consent.

## 10. Finish

- Don't trust the result from delegated agent or yourself just review and if it's a small fix just fix by yourself but if it's a big issue just delegate to agent or do by yourself that selected by user in step 3.
- If all finish, Present a short summary executed result for user.

## 11. Edit & Adjust

- If user ask to edit/adjust the finished work. you should edit by using section number 7 (who executes) that user selected before. but you should ask more about how to schedule (section number 6) again.
