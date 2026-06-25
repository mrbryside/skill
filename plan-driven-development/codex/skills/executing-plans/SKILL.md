---
name: executing-plans
description: Execute a written implementation plan — in parallel or sequentially, via qwen-agent or yourself. Use when you have a plan to implement.
---

# Executing Plans

**Announce:** "I'm using the executing-plans skill to implement this plan."

## 1. Pick & load

List the plans in `docs/plans/` as a numbered menu and let the user choose which to run. Read that plan and note each task's **Depends on** markings (the plan's parallel-safe grouping). Review it critically; if anything is unclear, conflicting, or has critical gaps, raise it with the user before starting. Otherwise create a TodoWrite list of the tasks.

## 2. Ask how to schedule

Ask the user — **parallel or sequential?**

- Parallel — follow the plan's **Depends on** markings: dispatch every task whose dependencies are already done concurrently, keep dependent tasks ordered. (If the plan marks nothing parallel-safe, fall back to sequential.)
- Sequential — one task at a time, in order.

## 3. Ask who executes

Ask the user — **qwen-agent or current agent?**

- qwen-agent — load the `qwen-agent` skill and delegate each task (or each parallel group, one per qwen) to it; you orchestrate, qwen does the work.
- current agent — execute the tasks yourself; do not load qwen-agent.

## 4. Ask more verification

Ask the user — **do you need another verification** just like `browser use validation`, `script validation`? When you receive the answer you will use this verification with existing verification that you need to do.

## 5. Execute

IMPORTANT: forward below to delegated agents or yourself if you not delegate.
For each task: mark in_progress → follow its steps exactly → run the verifications → mark completed. Don't skip verifications.
**Update two artifacts as you go, not just one:**

- The TodoWrite list (in-memory progress).
- **The plan file itself** — tick the task's checkbox (`- [ ]` → `- [x]`) in `docs/plans/<plan>.md` immediately after its verifications pass. This is the durable record that survives the session.
  **In qwen-agent mode, you (the orchestrator) own the plan file.** The delegated qwen does the task and returns; it does **not** edit the plan. After each qwen task returns and its verifications pass, _you_ tick the checkbox. Never assume the delegate updated it.
  **Stop and ask** when blocked: missing dependency, repeated test failure, unclear instruction, or critical gap. Don't guess. Never start on main/master without explicit consent.

## 6. Finish

- Don't trust the result from delegated agent or yourself just review and if it's a small fix just fix by yourself but if it's a big issue just delegate to agent or do by yourself that selected by user in step 3.
- Confirm all plan checkboxes are `- [x]`. If any remain unticked, resolve them before archiving.
- Remove and Move the plan: `docs/plans/<plan>.md` → `docs/done/plans/<plan>.md`.
- Remove and Move its spec: `docs/specs/<spec>.md` → `docs/done/specs/<spec>.md`.
- If all finish, Present a short summary executed result for user.

## 7. Edit & Adjust

- If user ask to edit/adjust the finished work. you should edit by using section number 7 (who executes) that user selected before. but you should ask more about how to schedule (section number 6) again.
