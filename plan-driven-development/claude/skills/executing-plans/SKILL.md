---
name: executing-plans
description: Execute a written implementation plan — in parallel or sequentially, via a cheap subagent or yourself. Use when the user says "execute the plan", "implement the plan", "run the plan", "start building", or when a plan file exists in docs/plans/ and the user wants to implement it.
---

# Executing Plans

**Announce:** "I'm using the executing-plans skill to implement this plan."

## 1. Pick & load

List the plans in `docs/plans/` as a numbered menu and let the user choose which to run. Read that plan and note each task's **Depends on** markings (the plan's parallel-safe grouping). Review it critically; if anything is unclear, conflicting, or has critical gaps, raise it with the user before starting. Otherwise create a task list of the tasks.

## 2. Ask how to schedule

Ask the user — **parallel or sequential?**

- Parallel — follow the plan's **Depends on** markings: dispatch every task whose dependencies are already done concurrently, keep dependent tasks ordered.
- Sequential — one task at a time, in order.

## 3. Ask who executes

Ask the user — **subagent `qwen-local` or current agent?**

- Subagent — spawn `qwen-local` subagent and delegate each task (or each parallel group, one per agent); you orchestrate.
- Current agent — execute the tasks yourself.

## 4. Ask more verification

Ask the user — **do you need additional verification such as browser validation or script validation?**
When you receive the answer, use this verification alongside existing verification steps.

## 5. Execute

For each task: mark in_progress → follow its steps exactly → run the verifications → mark completed. Don't skip verifications.

**Update two artifacts as you go:**

- The task list (in-memory progress).
- **The plan file itself** — tick the task's checkbox (`- [ ]` → `- [x]`) in `docs/plans/<plan>.md` immediately after its verifications pass.

**In subagent mode, you (the orchestrator) own the plan file.** The delegated subagent does the task and returns; it does **not** edit the plan. After each subagent task returns and verifications pass, _you_ tick the checkbox.

**Stop and ask** when blocked: missing dependency, repeated test failure, unclear instruction, or critical gap. Don't guess. Never start on main/master without explicit consent.

## 6. Finish

- Review all results. Small fix: fix yourself. Big issue: delegate based on step 3 selection.
- Confirm all plan checkboxes are `- [x]`. If any remain unticked, resolve them before archiving.
- Move the plan: `docs/plans/<plan>.md` → `docs/done/plans/<plan>.md`.
- Move its spec: `docs/specs/<spec>.md` → `docs/done/specs/<spec>.md`.
- Present a short summary of the executed result for the user.

## 7. Edit & Adjust

If the user asks to edit/adjust the finished work, use the executor selected in step 3, but ask again about scheduling (step 2) before starting.
