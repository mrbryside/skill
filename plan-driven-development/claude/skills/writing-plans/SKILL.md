---
name: writing-plans
description: Turn a resolved design doc into a task-by-task TDD implementation plan with short pseudocode. Use after grilling produces a design doc, before touching code. Trigger when the user says "write a plan", "create a plan", "turn this into a plan", or "plan this out".
---

# Writing Plans

Follow these steps in order.

## 1. Writing Plans

Input is a design doc from the grill skill (`docs/specs/...`). If you weren't handed a specific one, list the docs in `docs/specs/` as a numbered menu and let the user choose.

Write the plan assuming the engineer has zero context for the codebase. DRY, YAGNI, TDD, frequent commits.
Save the plan to `docs/plans/YYYY-MM-DD-<feature>.md`.

First read `AGENTS.md` for the architecture/structure and let it point you to the relevant folders, then map the files to create/modify — one responsibility each, following existing patterns. Read only the files you need.

Break the work into bite-sized tasks with clear file boundaries so independent tasks can run in parallel. Each task lists:
- Exact files (create/modify/test)
- A **Depends on:** line (task numbers, or "none" = parallel-safe)
- `- [ ]` checkbox steps, each a 2-5 min action:
  1. Write the failing test (sketch the assertion).
  2. Run it, expect FAIL.
  3. Implement minimally.
  4. Run it, expect PASS.

For code steps give **short pseudocode** — just enough to convey intent, key calls, and signatures. Never vague placeholders ("TBD", "handle edge cases", "similar to Task N"); always name exact files, functions, and types, and keep names identical across tasks.

## 2. After Writing Self Validation

After writing, self-check against the design doc:
- Every decision has a task.
- No placeholders.
- Names are consistent across tasks.
- Dependency markings are correct.
- Every task has `- [ ]` checkbox steps.

Fix gaps inline.

## 3. Ready to proceed?

Present the plan and ask: **"Ready to proceed? I can load the executing-plans skill and implement — or, if you want changes first, tell me what to edit or add and I'll revise the plan."**

Three valid responses:
- **Proceed** — load the `executing-plans` skill and implement.
- **Edit / add** — revise the plan file in place, re-run the self-check, then ask again. Loop until satisfied.
- **Hold** — leave the plan saved and end without implementing.

Don't load executing-plans until the user explicitly chooses to proceed. An edit request is **not** consent to proceed.
