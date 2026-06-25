---
name: writing-plans
description: Turn a resolved design doc into a task-by-task TDD implementation plan with short pseudocode. Use after grilling produces a design doc, before touching code.
---

writing plan with these step

- Writing plans
- After Writing Self Validation
- Ready to proceed

that you will found details below.

# Writing Plans

Input is a design doc from the grilling skill (`docs/specs/...`). If you weren't handed a specific one, list the docs in `docs/specs/` as a numbered menu and let the user choose. Write the plan assuming the engineer has zero context for our codebase and questionable taste. DRY, YAGNI, TDD, frequent commits.
Save the plan to `docs/plans/YYYY-MM-DD-<feature>.md`.
First read `AGENTS.md` for the architecture/structure and let it point you to the relevant folders, then map the files to create/modify — one responsibility each, following existing patterns. Read only the files you need; read more only if needed.
Break the work into bite-sized tasks, designed with clear file boundaries so independent tasks can run in parallel. Each task lists exact files (create/modify/test), a **Depends on:** line (task numbers, or "none" = parallel-safe), and `- [ ]` checkbox steps, each a 2-5 min action:

1. Write the failing test (sketch the assertion).
2. Run it, expect FAIL.
3. Implement minimally.
4. Run it, expect PASS.
   For code steps give **short pseudocode** — just enough to convey intent, key calls, and signatures — not full implementations; the writing agent fills in the real code. Never vague placeholders ("TBD", "handle edge cases", "similar to Task N"); always name the exact files, functions, and types, and keep names identical across tasks.

## After Writing Self Validation

- After writing, self-check against the design doc: every decision has a task, no placeholders, names consistent, and the dependency markings are correct. Fix gaps inline.
- Every task should have `- [ ]` checkbox steps, each a 2-5 min action:
- Depends on tasks provided.

## Ready to proceed?

After the self-check, present the plan and ask: **"Ready to proceed? I can load the executing-plans skill and implement — or, if you want changes first, tell me what to edit or add and I'll revise the plan."**

- This is a real fork, not a yes/no. Three valid responses:
  - **Proceed** — load the `executing-plans` skill and implement.
  - **Edit / add** — the user names changes; revise the plan file in place, re-run the self-check, then ask again. Loop until they're satisfied.
  - **Hold** — the user wants to stop here; leave the plan saved and end without implementing.
- Don't load executing-plans until the user explicitly chooses to proceed. An edit request is **not** consent to proceed — make the edits, then return to this gate.
