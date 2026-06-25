---
name: qwen-agent
description: Delegate menial, well-scoped coding tasks to a cheap Qwen-backed subagent instead of burning the main model's tokens/quota. Use when the work is mechanical and low-risk — bulk renames, formatting, boilerplate, find-replace, grep-style search & summarization, reading/condensing logs or files, test/docstring/comment scaffolding, or running builds/linters/tests and reporting pass-fail. Also use when the user says "use qwen", "delegate this", "send it to 9arm/qwen", or "do this cheaply". Do NOT use for architecture, design, debugging judgment, security-sensitive edits, or anything needing this conversation's context.
---

# qwen-agent

Offload **menial, self-contained** tasks to a cheap Qwen model running as an opencode subagent. Keeps the main model's reasoning quota for work that needs it.

## The subagent

This skill expects a subagent named `qwen-agent` to be configured in your opencode project.

Create or update `.opencode/agents/qwen-agent.md` (see the same-named file in this project for a starter). Register it in `opencode.json` if you prefer inline config. After changing any agent or skill file, **quit and restart opencode** — config is not hot-reloaded.

## Invoking the subagent

Use the `task` tool with `subagent_type: "qwen-agent"`:

```json
{
  "description": "Rename helper to utils",
  "subagent_type": "qwen-agent",
  "prompt": "In /Users/sirawat/personal/vietsCode/src/helpers.js, rename every function prefixed `old_` to `new_` and update all call sites in /Users/sirawat/personal/vietsCode/src. Run `npm run lint` and report any errors."
}
```

## Writing the task prompt (most important step)

The qwen subagent has **zero** context from this conversation. A vague prompt is the #1 failure mode. Every prompt must be standalone:

- **Absolute paths** for every input and output file (`/Users/sirawat/personal/vietsCode/src/foo.ts`, not `foo.ts`).
- **Explicit inputs, outputs, and acceptance criteria** — what to change, what "done" looks like.
- **No references** to "the file we discussed", "above", or prior turns.
- Treat qwen as a capable-but-literal junior: spell out the steps, keep scope tight.

Bad: `clean up the imports`  
Good: `In /Users/sirawat/personal/vietsCode/src/api.ts, remove unused imports and sort the remaining import statements alphabetically. Do not change any other code. Confirm the file still parses.`

## Mind the context window (128k)

Qwen runs with a **128k-token context window** — much smaller than the main model. The whole job (your prompt + every file it reads + its own reasoning and edits) has to fit inside it.

- **Estimate the footprint** before delegating: roughly the bytes of files it must read + open + write, ÷ 4 ≈ tokens. If a single task would pull in large files or many files at once, it won't fit.
- **Break large jobs into independent chunks** that each touch a bounded slice — e.g. one file (or a few small ones) per run, one directory per run, one log segment per run. Run the chunks as separate `task` invocations.
- **Don't make it read what it doesn't need.** Point it at the exact files/paths required; never tell it to "scan the repo" or read a whole large tree.
- **Watch for context-exhaustion symptoms** when verifying: truncated edits, ignored later instructions, or a summary that omits files it was told to touch usually mean the task overflowed — split it smaller and retry.

When a job is inherently too big to slice cleanly (it needs whole-codebase context to do correctly), that's a sign it isn't a qwen task — keep it yourself.

## Working directory

The subagent's working directory is the project root by default. Do not rely on relative paths:

- Put **absolute paths in the prompt**, or
- Grant access via `external_directory` permissions in the agent config.

## Return contract

- **Default (text):** qwen's final message is returned by the `task` tool — read it directly.
- **Need structured output:** instruct the prompt to return JSON with a specific schema, e.g. `Return a JSON object with keys "files_changed" (array of paths) and "errors" (array of strings). No markdown, no prose.`
- **Background / parallel:** opencode `task` calls can be launched concurrently in one response. Use this for independent chunks, then collect each result.

## Workflow checklist

1. Confirm the task is menial and low-risk (see the skill description). If it needs design judgment or this chat's context, **do it yourself** — don't delegate.
2. Check it fits qwen's **128k context window** — estimate the file footprint and split large jobs into bounded per-file/per-dir chunks.
3. Write a fully self-contained prompt with absolute paths and acceptance criteria.
4. Invoke the `qwen-agent` subagent via the `task` tool.
5. **Verify the output yourself** — qwen is cheaper and less reliable. Check the file/result actually meets the acceptance criteria before reporting success.

## When NOT to delegate

Architecture/design, debugging that needs reasoning, security-sensitive changes, anything requiring this conversation's context, or tasks where a wrong cheap-model edit is costly to catch. When in doubt, keep it.

## When Receive Result from delegate

When receive the result you must to validate the result and if it's not ok you should plan the fix then delegate to qwen again. if it's a big defect or need a big decision you have to stop and tell the user
