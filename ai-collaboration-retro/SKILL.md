---
name: ai-collaboration-retro
description: Use when extracting reusable AI-collaboration lessons from an authorized project, auditing a retrospective knowledge base for low-token structure problems, or deciding what AI should read first from an existing retro library during project work.
---

# AI Collaboration Retro

## Core Principle

Optimize project memory for future AI work: fewer files read, clearer routing, unique issue IDs, short default-action docs, and deeper root-cause docs only when needed.

## Quick Reference

| Situation | Action |
|---|---|
| New project, want reusable lessons | Use `Retrospective extraction` |
| Existing retro library, current task needs guidance | Use `Project-use routing` |
| Existing retro library feels bloated or confusing | Use `Knowledge-base audit` |

## Choose One Mode

| User intent | Mode |
|---|---|
| "Read this project and summarize the lessons" / "turn this into a复盘" | Retrospective extraction |
| "Use this复盘 while working on a project" / "what should AI read first" | Project-use routing |
| "Check whether this复盘 structure is good" / "split or merge these坑" | Knowledge-base audit |

If unclear, ask one short question: "Do you want me to extract a new复盘 from a project, or use an existing复盘 to guide current work?"

## Mode A: Retrospective Extraction

Use when authorized to read a project and produce or update a low-token AI collaboration knowledge base.

1. Read only the directory map first: `rg --files` or equivalent.
2. Identify project type and high-signal files:
   - Entry/rules: `README.md`, `AGENTS.md`, `CLAUDE.md`.
   - Tech evidence: `package.json`, `pom.xml`, `build.gradle`, `requirements*.txt`, lockfiles.
   - Workflow evidence: `scripts/`, `.github/`, `docs/`, `discuss/`, config files.
3. Sample code/config only to verify concrete patterns. Do not full-read large trees by default.
4. Extract issues by problem type, not by project name.
5. Write or update this structure when requested:

```text
README.md              entry route only
最佳实践/xx.md          short default actions for one problem type
专题/专题_xx.md         symptoms, root cause, case, prevention
命令速查.md             copyable commands only
基线定义文档/            reusable AGENTS/CLAUDE/project baselines
```

6. Each issue should include: trigger, symptom, root cause, correct action, prevention, and one concrete case if available.
7. Prefer domain IDs over discovery-order IDs when reorganizing:
   - `ENV-*`, `PKG-*`, `VERIFY-*`, `DOC-*`, `SEC-*`, `AI-CODE-*`, `AI-GOV-*`.
   - If existing `F-001` style IDs exist, keep them but ensure every ID is unique.
8. Keep route files short; move detailed project cases and long explanations into `专题/`.

## Mode B: Project-Use Routing

Use when working inside a project and using an existing retro library to reduce context.

1. Read the current project's local rules first: `AGENTS.md`, `CLAUDE.md`, then `README.md` if present.
2. Read the retro library `README.md` only as a route table.
3. Pick exactly one `最佳实践/` file based on the current problem.
4. Apply the default action from that file.
5. Read a `专题/` file only when the default action fails, root cause is unclear, or the issue is recurring.
6. Read `命令速查.md` only when a copyable command is needed.
7. Do not read multiple专题 files unless the problem is explicitly cross-domain.
8. Do not treat the retro library as a second codebase; it is a routing aid for the current project.

## Mode C: Knowledge-Base Audit

Use when the user asks whether the复盘 logic, order, or splitting is reasonable.

Check in this order:

1. Entry route: Does `README.md` tell AI what to read first without long explanation?
2. File size: Are best-practice files short enough to be read one at a time?
3. Scope: Does each `最佳实践/xx.md` cover one problem type only?
4. Depth: Are root causes and cases in `专题/`, not in the route file?
5. IDs: Are issue IDs unique, stable, and non-overlapping across files?
6. Cross-links: Do README, best practices, and专题 files point to each other correctly?
7. Commands: Are copyable commands centralized in `命令速查.md` or minimal-command sections?
8. Baselines: Are reusable AGENTS/CLAUDE templates separate from incident notes?

## Split / Merge Rules

Split a file when:

- It mixes tool commands, code-quality rules, and project-governance rules.
- AI must read unrelated sections to solve one problem.
- It has more than one owner question: "how to fix now" and "why it happened".

Merge entries when:

- Two routes point to the same专题 for the same reason.
- Multiple issue IDs describe the same root cause with different wording.
- A best-practice file has only one tiny rule and no distinct routing value.

Move content when:

- Commands are buried in专题 narrative -> move to `命令速查.md` or minimal commands.
- Project-specific examples are in `最佳实践/` -> move to `专题/`.
- AGENTS/CLAUDE template maintenance is inside document-conversion notes -> move to AI governance.

## Output Formats

For audit-only requests, lead with:

```text
结论：整体是否合理。
优先修改：3-5 条。
拆分/合并建议：按文件列出。
更高效流程：给 AI 的读取顺序。
```

For edit requests, make the smallest structural edits that improve routing, then report:

```text
已改：文件和目的。
校验：重复 ID、旧引用、链接目标。
注意：未触碰的既有脏改或待确认范围。
```

## Validation

After edits, run equivalent checks:

```powershell
# stale references
Get-ChildItem -Recurse -Filter *.md | Select-String -Pattern '旧文件名|旧坑号|错误专题名'

# duplicate F-style IDs
Get-ChildItem -Recurse -Filter *.md | Select-String -Pattern '^## F-' |
  ForEach-Object { if ($_.Line -match '^## (F(?:-OBS)?-[0-9]{3})') { $matches[1] } } |
  Group-Object | Where-Object { $_.Count -gt 1 }

# markdown links outside template folders
# Resolve relative .md links and report missing targets.
```

## Common Mistakes

- Do not full-read the whole project before seeing the directory map.
- Do not turn README into a long essay; it is a router.
- Do not put root-cause cases in best-practice files unless they are one-line references.
- Do not reuse issue IDs across专题 files.
- Do not split by project name; split by problem type.
- Do not silently rewrite user notes unrelated to the routing problem.
