---
name: agents-md-update
description: Create or update AGENTS.md and docs/agents/ using git diff to detect what changed since the last documented commit. Runs autonomously — no questions asked. Use when the user says "update AGENTS.md", "update agents docs", "document the codebase", or after significant code changes.
---

# agents-md-update

Create, restructure, or maintain agent context documentation (`AGENTS.md`, `docs/agents/`) using a context-saving index structure. Act **without asking the user any questions** — detect the situation and proceed:

- If `AGENTS.md` does **not** exist → **create mode**: build from scratch by scanning the repo.
- If `AGENTS.md` exists → **update mode**: refresh it using the git-diff workflow below.

Report what you created/updated when done.

## Output structure

```
AGENTS.md                          # root index only — no long details
docs/agents/
├── README.md
├── {category}/
│   ├── index.md                   # category index with jump table
│   └── {subtopic}.md
```

## Rules

1. `AGENTS.md` is **only** an index: one-line project overview, last documented commit, project structure table, "If you want to know X → go to Y" jump table, maintenance note.
2. Every `docs/agents/{category}/index.md`: short "when to read this" sentence, jump table to subfiles, link back to `AGENTS.md`.
3. Sub-detail files contain the actual content. Keep focused on one topic.
4. All links must be relative from the file that contains them.
5. Never paste long details directly into `AGENTS.md`.

## Workflow

### Create mode

1. Scan: `git ls-files`, top-level directories, build/config files, entry points.
2. Read a few key files to understand architecture — don't read everything.
3. Create `AGENTS.md`, `docs/agents/README.md`, and category index + subfiles per major area.
4. Write current commit: `git rev-parse HEAD`.
5. Run verification checklist.

### Update mode

1. Read `AGENTS.md` to find the `Last documented commit` line.
2. Run `git diff --name-only <current_commit>..HEAD`.
3. Read changed files to understand what needs documenting.
4. Skip trivial changes (comments, formatting, refactors that don't change behavior).
5. Update relevant docs in `docs/agents/`.
6. If a new sub-file is needed, create it and link from the category `index.md`.
7. Bump `Last documented commit` in `AGENTS.md` to `git rev-parse HEAD`.
8. Verify all relative links resolve.

## Verification checklist

- [ ] `AGENTS.md` has the project structure table.
- [ ] `AGENTS.md` has a jump table to all category indexes.
- [ ] `AGENTS.md` has an up-to-date `Last documented commit` line.
- [ ] Every `docs/agents/{category}/index.md` exists and has a jump table.
- [ ] All relative links resolve to existing files.
- [ ] Maintenance note exists in both `AGENTS.md` and `docs/agents/README.md`.
- [ ] No long details remain in `AGENTS.md`.
