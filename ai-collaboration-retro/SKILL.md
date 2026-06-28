---
name: ai-collaboration-retro
description: Use when an AI agent should first check whether the user's own project memory already has a reusable lesson, generate or reorganize that memory from authorized local projects, audit the structure, or remind the user to archive newly discovered traps back into the knowledge base.
---

# AI Collaboration Retro

## Core Principle

Turn the user's own verified project lessons into reusable AI project memory, so future sessions avoid repeating known mistakes, send fewer wrong requests, read less irrelevant context, and spend fewer tokens.

This skill is not for writing longer retrospectives. It is for helping AI read less, route faster, and start from the user's own proven paths instead of guessing again.

This method is platform-agnostic. If the current AI tool supports native skills, use this file as a skill. If not, use it as a reusable working guide or long-lived prompt.

## Quick Reference

| Situation | Action |
|---|---|
| "I want to do this task, first see whether we already learned it" | Use `Project-use routing` |
| "Turn my project into reusable AI memory" | Use `Knowledge-base generation` |
| "Review this memory base or archive a new trap" | Use `Audit or archive update` |

## Path Preflight

Before using any mode:

1. Look for `local-config.yaml` in this skill folder.
2. If it exists, read `knowledge_base.path` and treat it as the default knowledge-base path.
3. If it does not exist, ask the user which local knowledge-base path should be used for this run.
4. In either case, explicitly remind the user that the path must be authorized for AI read access.
5. If the user wants the current path reused next time, offer to save it to `local-config.yaml` based on `local-config.example.yaml`.
6. If the chosen path is outside the current authorized read scope, stop and ask for authorization before reading it.
7. If the path is authorized but does not contain `README.md` or the expected knowledge-base structure, tell the user that the knowledge base is not ready yet and that `Knowledge-base generation` should run first.

Use messages like these:

```text
路径检查：这次要用哪个本地知识库路径？把路径发我就行。注意：这个路径需要你授权 AI 可读；如果你愿意，我也可以把它记成下次默认路径。
```

```text
路径检查：已找到知识库路径。授权检查：我现在还不能读这个路径。下一步：请先授权这个路径，再继续。
```

```text
路径检查：已确认知识库路径。知识库检查：这个路径下还没有现成知识库。下一步：要不要先按这套规则生成一份基础结构？
```

## Choose One Mode

| User intent | Mode |
|---|---|
| "我要做这件事，先查一下有没有现成经验" | Project-use routing |
| "我要把这个项目沉淀成经验库" | Knowledge-base generation |
| "这个新坑帮我看看该归档到哪里" | Audit or archive update |

If unclear, ask one short question: "Do you want me to use an existing knowledge base for this task, or generate/update the knowledge base itself?"

## Mode A: Knowledge-base Generation

Use when authorized to read local projects and produce or refresh a low-token knowledge base from the user's own real, verified project history.

1. Use the chosen knowledge-base path as the default output location unless the user gives another authorized target.
2. Read only the directory map first: `rg --files` or equivalent.
3. Identify high-signal files first:
   - Entry/rules: `README.md`, `AGENTS.md`, `CLAUDE.md`
   - Tech evidence: `package.json`, `pom.xml`, `build.gradle`, `requirements*.txt`, lockfiles
   - Workflow evidence: `scripts/`, `.github/`, `docs/`, config files
4. Sample code or config only to verify patterns. Do not full-read large trees by default.
5. Extract only lessons that should change future AI behavior: known traps, default actions, verification commands, routing decisions, and prevention rules.
6. Group issues by problem type, not by project name.
7. Write or update this structure when requested:

```text
README.md              route table only
best-practice_xx.md    one short file for one problem type
topics/topic_xx.md     symptoms, root cause, case, prevention
command-cheatsheet.md  copyable commands only
baselines/             reusable AGENTS/CLAUDE/project baselines
```

8. Each issue should include: trigger, symptom, root cause, correct action, prevention, and one concrete case if available.
9. Prefer stable domain IDs such as `ENV-*`, `PKG-*`, `VERIFY-*`, `DOC-*`, `SEC-*`, `AI-CODE-*`, `AI-GOV-*`.
10. Keep route files short; move deep cases and long explanations into `topics/`.
11. Make it explicit that this is local-only work: read only authorized local projects and do not upload or store content externally.

## Mode B: Project-Use Routing

Use when working inside a project and wanting to reuse existing project memory with less context and fewer wrong turns.

Read in this order:

1. Read the current project's own local rules first: `AGENTS.md`, `CLAUDE.md`, then `README.md` if present.
2. If the user explicitly invokes this skill, read the knowledge-base `README.md` once as the route table before reading any best-practice or deep-dive file.
3. If the route table has a similar problem, pick exactly one matching best-practice file.
4. If the route table does not have a similar problem, continue with normal project-local investigation instead of widening retro reads.
5. Apply the default action from the matched best-practice file before asking the user to re-explain known history.
6. For install, download, environment, or dependency tasks, prefer the route's mirror, wheel, version-lock, and platform rules before trying the default upstream path blindly.
7. Read a deep-dive topic only when the default action fails, root cause is unclear, or the issue is recurring.
8. Read the command cheatsheet only when a copyable command is needed.
9. Do not read multiple topic files unless the issue is clearly cross-domain.
10. Do not treat the knowledge base as a second codebase; it is a routing aid for the current project.
11. In the same response, explicitly report whether the route table was read and which best-practice file was matched. If nothing matched, say so and state that normal project-local investigation continued.
12. If the work reveals a new trap, a stale rule, a missing command, or a reusable new default action, explicitly remind the user that this lesson may need to be archived back into the knowledge base.

## Mode C: Audit or Archive Update

Use when the user asks whether the logic, order, split, or merge strategy is reasonable, or when a new lesson should be archived into the existing knowledge base.

Check in this order:

1. Entry route: does `README.md` clearly tell AI what to read first?
2. File size: are best-practice files short enough to read one at a time?
3. Scope: does each best-practice file cover one problem type only?
4. Depth: are root causes and cases in `topics/`, not in route files?
5. Behavior: does each route tell AI what to do first, not just what to read?
6. IDs: are issue IDs unique, stable, and non-overlapping?
7. Cross-links: do README, best practices, and topics link correctly?
8. Commands: are copyable commands centralized or kept minimal?
9. Baselines: are reusable AGENTS/CLAUDE templates separate from incident notes?
10. Archive fit: should the new lesson extend an existing route/topic, or does it need a new file?

## Split / Merge Rules

Split a file when:

- it mixes commands, code-quality rules, and governance rules,
- AI must read unrelated sections to solve one problem,
- it mixes default actions with long historical cases,
- one file is answering both "what should I do now" and "why did it happen".

Merge entries when:

- two route files point to the same decision path,
- multiple issue IDs describe the same root cause in different words,
- a best-practice file is too small to provide distinct routing value,
- separate files always need to be read together for one decision.

Move content when:

- commands are buried in narrative -> move them to `command-cheatsheet.md`,
- project-specific case detail is in a best-practice file -> move it to `topics/`,
- AGENTS/CLAUDE maintenance notes are mixed into incident notes -> move them to `baselines/` or AI-governance topics.

## Output Formats

For audit-only requests, lead with:

```text
结论：整体是否合理。优先修改：1-5 条。拆分/合并建议：按文件列出。更高效流程：给 AI 的读取顺序。
```

For edit requests, report:

```text
已改：改了哪些文件，分别解决什么。校验：检查了哪些旧引用、重复 ID、路由链路。注意：哪些内容刻意没动。
```

For project-use requests that read the route table, include:

```text
路由：已读取 README.md。命中：best-practice_xx.md / 未命中。后续：默认动作 / 回到项目本地排查 / 升级专题。
```

If new project memory was discovered, also include:

```text
归档提醒：本次发现了可复用新经验，是否需要归档到知识库？
```

For path-missing or knowledge-base-missing cases, include:

```text
路径检查：已检查。当前状态：未提供路径 / 未授权 / 知识库未生成。下一步：提供路径 / 授权路径 / 先生成知识库。
```

## Validation

After edits, run equivalent checks:

```powershell
# stale references
Get-ChildItem -Recurse -Filter *.md | Select-String -Pattern 'old file name|deprecated topic name'

# duplicate F-style IDs
Get-ChildItem -Recurse -Filter *.md | Select-String -Pattern '^## F-' |
  ForEach-Object { if ($_.Line -match '^## (F(?:-OBS)?-[0-9]{3})') { $matches[1] } } |
  Group-Object | Where-Object { $_.Count -gt 1 }
```

## Common Mistakes

- Do not full-read the whole project before checking the directory map.
- Do not turn README into a long essay; it is a router.
- Do not put deep root-cause cases in best-practice files.
- Do not reuse issue IDs across topics.
- Do not split by project name; split by problem type.
- Do not silently rewrite unrelated user notes.
