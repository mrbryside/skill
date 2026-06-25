---
name: agents-markdown-creator
description: "Load this skill to create or update AGENTS.md autonomously — no questions asked — using a context-saving index structure for LLMs."
---

# agents-markdown-creator

Create, restructure, or maintain agent context documentation (`AGENTS.md`, `docs/agents/`) using a context-saving index structure.

## Autonomous operation

When this skill loads, act **without asking the user any questions**. Detect the situation and proceed:

- If `AGENTS.md` does **not** exist → **create mode**: build `AGENTS.md` and `docs/agents/` from scratch by scanning the repo structure.
- If `AGENTS.md` exists → **update mode**: refresh it using the git-diff workflow below.

Infer the project overview, structure table, categories, and content yourself from the codebase. Only stop if a git or file-write command fails. Report what you created/updated when done.

## Output structure

```
AGENTS.md                          # root index only — no long details
docs/agents/
├── README.md                      # how to maintain this folder
├── {category}/
│   ├── index.md                   # category index with jump table
│   └── {subtopic}.md              # detailed subfiles
```

## Rules

1. `AGENTS.md` is **only** an index. It must include:
   - One-line project overview.
   - Last documented commit line (right after the overview).
   - Project structure table (every relevant level, with descriptions).
   - "If you want to know X → go to Y" table pointing to each `docs/agents/{category}/index.md`.
   - Maintenance note (see template below).

2. Every `docs/agents/{category}/index.md` is also an index. It must include:
   - Short "when to read this" sentence.
   - "If you want to know X → go to Y" table pointing to subfiles.
   - Link back to `AGENTS.md`.

3. Sub-detail files (`{subtopic}.md`) contain the actual content. Keep them focused on one topic.

4. All links must be relative from the file that contains them.

5. Never paste long details directly into `AGENTS.md`.

6. When adding a new top-level category:
   - Create `docs/agents/{category}/index.md`.
   - Add a row to the table in `AGENTS.md`.
   - Add a row to the table in `docs/agents/README.md`.

7. When adding a sub-topic:
   - Create `docs/agents/{category}/{subtopic}.md`.
   - Link it from `docs/agents/{category}/index.md`.

## Templates

### AGENTS.md

```markdown
# AGENTS.md — {project-name}

One-line overview.

`Last documented commit: {sha}`

## Project structure

| Path      | Purpose     |
| --------- | ----------- |
| `{path}/` | description |

Only open the sections below when they are relevant to the current task.

| If you want to know... | Go to                                                              |
| ---------------------- | ------------------------------------------------------------------ |
| topic description      | [docs/agents/{category}/index.md](docs/agents/{category}/index.md) |

## Maintenance note

When adding new context:

1. Put detail in the relevant `docs/agents/{category}/` subfile.
2. If the category does not exist, create the folder and an `index.md`.
3. Link the new subfile from the category `index.md`.
4. If it is a new top-level category, add a row to the table above.
5. Never paste long details directly into `AGENTS.md`.
6. Any new document under `docs/agents/` must follow the same index style as `AGENTS.md`: start with a short "when to read this" description, use an "If you want to know X → go to file Y" table when it covers multiple subtopics, and keep long details in linked subfiles.
```

### docs/agents/README.md

```markdown
# Agent documentation

This folder holds context documents for agents. Treat every `index.md` as a jump point — only open the linked files that are relevant to the current task.

| If you want to know... | Go to                                                  |
| ---------------------- | ------------------------------------------------------ |
| Root index             | [../AGENTS.md](../AGENTS.md)                           |
| topic                  | [docs/agents/{category}/index.md]({category}/index.md) |

## Maintenance note

(same as AGENTS.md maintenance note)
```

### docs/agents/{category}/index.md

```markdown
# Category name

Short "when to read this" sentence.

| If you want to know... | Go to                      |
| ---------------------- | -------------------------- |
| subtopic               | [subtopic.md](subtopic.md) |

Back to [AGENTS.md](../AGENTS.md)
```

### docs/agents/{category}/{subtopic}.md

```markdown
# Subtopic title

Actual content here. Keep focused on one topic.

Back to [{category}/index.md](index.md)
```

## Workflow

### Create mode (no `AGENTS.md` yet)

1. Scan the repo to learn the structure: `git ls-files`, top-level directories, build/config files, entry points.
2. Read a few key files (entry points, configs, main packages) to understand architecture, how to run, and how to test — don't read everything.
3. Create `AGENTS.md` (root index), `docs/agents/README.md`, and one `docs/agents/{category}/index.md` plus subfiles per major area, following the Templates.
4. Write the current commit into `AGENTS.md` after the overview: `git rev-parse HEAD`.
5. Run the verification checklist.

### Update mode (`AGENTS.md` exists)

1. Read `AGENTS.md` to find the current commit (the `Last documented commit` line after the overview).
2. Run `git diff --name-only <current_commit>..HEAD` to see what changed since the last doc update.
3. Read changed files first to understand what needs documenting.
4. Read other files only if you need context the diff doesn't provide — don't read everything.
5. Identify what changed and decide what to doc: codebase structure, architecture, flow, how to run, how to test, new components. Skip trivial changes (docs, comments, formatting, refactors that don't change behavior, bug fix).
6. Update the relevant docs in `docs/agents/`.
7. If a new sub-file is needed, create it and link from the category `index.md`.
8. Bump the `Last documented commit` line in `AGENTS.md` to `git rev-parse HEAD`.
9. Verify all relative links resolve.

## Verification checklist

- [ ] `AGENTS.md` has the project structure table.
- [ ] `AGENTS.md` has a jump table to all category indexes.
- [ ] `AGENTS.md` has an up-to-date `Last documented commit` line.
- [ ] Every `docs/agents/{category}/index.md` exists and has a jump table.
- [ ] All relative links resolve to existing files.
- [ ] Maintenance note exists in both `AGENTS.md` and `docs/agents/README.md`.
- [ ] No long details remain in `AGENTS.md`.
