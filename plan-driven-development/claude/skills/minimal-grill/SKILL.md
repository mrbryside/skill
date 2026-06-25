---
name: minimal-grill
description: For small tasks — grill the design, present three approaches, confirm there's nothing to add, plan inline, get approval, then execute. All in one pass with no spec or plan files written. Use when the user says "minimal grill", "quick grill", "small task", or for changes that don't warrant the full grill → writing-plans → executing-plans flow.
---

# Minimal Grill

**Announce:** "I'm using the minimal-grill skill."

One skill, in-session, no files written. Grill the design, pick an approach, hold the plan inline, get sign-off, then execute.

## 1. Grill

Interview the user **one question at a time** (wait for each answer) until every material decision is resolved. For each, give your recommended answer + one-line rationale. If the codebase can answer it, read `AGENTS.md` first for architecture/structure, jump to the relevant folder, and read only what you need. Stop when nothing material is left to guess.

## 2. Present three approaches

When every material decision is resolved, do **not** jump straight to the plan. First present **three distinct approaches**:

- Give each a short name + one-line summary.
- For each, lay out the trade-offs: what it optimizes for and what it sacrifices.
- **Recommend one** with a one-line rationale.
- **STOP and let the user choose.** If they pick a hybrid, restate the merged approach in one line and confirm before continuing.

## 3. Last call — anything to add?

**Ask: "Anything else to add or change before I lay out the plan?"** If they add something that shifts the design, re-confirm the chosen approach still holds. Proceed on "nothing else" / go-ahead.

## 4. Plan inline

Plan **around the chosen approach**. Do **not** write any file. Lay out the tasks as a list — bite-sized, with clear file boundaries. For each task note exact files, a **Depends on** (task numbers or "none"), and TDD steps as **short pseudocode**. No vague placeholders; name exact files/functions/types.

## 5. Approval gate

Present the inline plan and **STOP**. Ask: **"Ready to proceed? I can start implementing — or tell me what to edit or add and I'll revise the plan first."**

Three valid responses:
- **Proceed** — explicit go-ahead unlocks step 6.
- **Edit / add** — revise the plan, then return to this gate. An edit request is **not** consent to proceed.
- **Hold** — end without implementing (nothing written to disk).

## 6. Ask how to schedule

Ask the user — **parallel or sequential?**

## 7. Ask who executes

Ask the user — **subagent (cheap/haiku model) or current agent?**

- Subagent — use the Agent tool with `model: "haiku"`; you orchestrate.
- Current agent — execute yourself.

## 8. Ask more verification

Ask the user — **do you need additional verification such as browser validation or script validation?**

## 9. Execute

For each task: mark in_progress → follow steps exactly → run verifications → mark completed. Don't skip verifications. Stop and ask when blocked. Never start on main/master without explicit consent.

## 10. Finish

Review all results. Small fix: fix yourself. Big issue: delegate based on step 7 selection. Present a short summary.

## 11. Edit & Adjust

If the user asks to edit/adjust the finished work, use the executor from step 7, but ask about scheduling (step 6) again before starting.
