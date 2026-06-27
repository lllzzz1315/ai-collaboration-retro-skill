# ai-collaboration-retro-skill

Chinese version: [README.md](README.md)

`ai-collaboration-retro` is a general AI collaboration retrospective skill. Its purpose is not to create one more retrospective document, but to turn past project mistakes, verified workflows, and easy-to-misread rules into a reading route that AI can quickly reuse.

In one sentence:
it helps AI avoid repeating old mistakes, send fewer wrong or redundant requests, read less irrelevant context, and spend fewer tokens during future project work.

## Why This Exists

This skill solves a specific collaboration problem: teams accumulate many project lessons, traps, commands, and rules, but each new AI session still has to rediscover what matters, what to read first, and which past lessons should guide the current task.

It turns past mistakes and project lessons into a reusable reading route, so future AI sessions can hit the right context faster, avoid known failure paths, and send fewer unnecessary follow-up, trial-and-error, or repeated execution requests.

In practical terms, it cares less about "more complete documentation" and more about:

- whether AI can find the right entry point with less context,
- whether AI can start from a verified default action instead of guessing again,
- whether AI can tell which content is a historical case and which content should guide the current operation,
- whether AI reads root-cause material only when deeper investigation is actually needed.

Common symptoms:

- retrospective notes keep getting longer, so AI must read too much before finding the actionable rule,
- environment, dependency, script, build, and AI-code-quality traps keep recurring without a stable entry route,
- short "what to do" rules are mixed with long "why it happened" root-cause cases,
- commands are scattered through narrative notes, so AI may improvise or copy stale commands,
- issue IDs, links, and topic boundaries drift over time,
- during real project work, AI does not know whether to read project rules, the retro README, a best-practice file, or a deep-dive topic first.

The goal is to compress that memory into a route-first structure: route first, apply the default action second, and read root-cause material only when needed. The practical payoff is less context, fewer tokens, fewer repeated mistakes, and fewer wasted AI requests.

## What The Skill Does

The skill supports two primary workflows and one audit workflow:

- `Retrospective extraction`
  Read an authorized project selectively, identify engineering lessons, collaboration rules, and known traps that affect future AI work, then organize them into a low-token knowledge base.
- `Project-use routing`
  Use an existing retrospective library during day-to-day work to decide what to read first, what to skip, which default action to apply first, and when to escalate from a short guide to a deep-dive issue note.
- `Knowledge-base audit`
  Review an existing retro library, find places that make AI over-read, misread, or repeat trial-and-error, and suggest route fixes, file splitting, merging, ID cleanup, and link cleanup.

## Before And After

Typical "before" state:

- one long retrospective note with mixed commands, cases, and conclusions,
- no clear reading order,
- duplicated issue IDs,
- AI has to full-read large notes before acting,
- the same environment or dependency traps get rediscovered every session.

Typical "after" state:

- `README.md` acts as the route table,
- `最佳实践/` contains one problem type per short file,
- `专题/` keeps root causes, cases, and prevention notes,
- `命令速查.md` keeps copyable commands out of long narratives,
- AI can usually solve a problem by reading one route file and one focused best-practice file first.

## Who It Is For

- teams using Codex, Claude Code, or similar agents on medium-to-large repositories,
- maintainers building internal AI collaboration playbooks,
- people who already keep project retrospectives but want them to be more usable by AI,
- anyone tired of re-explaining the same environment, dependency, or workflow traps every session.

## Repository Layout

```text
ai-collaboration-retro-skill/
├── LICENSE
├── README.md
├── readme_en.md
└── ai-collaboration-retro/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## Install

Clone this repository, then either copy the `ai-collaboration-retro/` folder into your tool's local skills directory, or use `SKILL.md` directly as a reusable instruction set.

For tools that support a skills directory, common locations include:

- Windows: `%USERPROFILE%\\.codex\\skills\\`
- macOS/Linux: `~/.codex/skills/`

If the `skills/` directory does not exist yet, create it first.

Example:

```powershell
git clone https://github.com/lllzzz1315/ai-collaboration-retro-skill.git
Copy-Item -Recurse .\ai-collaboration-retro-skill\ai-collaboration-retro $env:USERPROFILE\.codex\skills\
```

```bash
git clone https://github.com/lllzzz1315/ai-collaboration-retro-skill.git
cp -R ./ai-collaboration-retro-skill/ai-collaboration-retro ~/.codex/skills/
```

## Platform Notes

The core of this repository is the method inside `ai-collaboration-retro/SKILL.md`, and that method is tool-agnostic.

- If your AI tool supports native skills, install the full `ai-collaboration-retro/` directory.
- If your AI tool does not support native skills, you can still reuse the method by reading `SKILL.md` directly and treating it as a long-lived working prompt or team rule set.
- `agents/openai.yaml` is platform metadata for tools that use it. It is not required to benefit from the core method.

## Quick Start

1. Install the skill into your AI tool's local `skills/` directory, or make the tool read `SKILL.md`.
2. Open a project or retrospective library you are authorized to inspect.
3. Use one of the prompts below. They are intentionally short and task-first.
4. Let the skill decide whether the task is:
   `Retrospective extraction`, `Project-use routing`, or `Knowledge-base audit`.

## Short Prompt Cheatsheet

```text
I want to work on this task. Use $ai-collaboration-retro first and check whether we already have a reusable lesson.
```

```text
I want to handle this issue. Use $ai-collaboration-retro first and tell me which best-practice file should guide it.
```

```text
I want to reorganize this retrospective. Use $ai-collaboration-retro first and show me how it should be split.
```

```text
I want to archive this new trap. Use $ai-collaboration-retro first and tell me where it belongs.
```

## Example Prompts

```text
I want to work on this task. Use $ai-collaboration-retro first to see whether we already have a reusable lesson for it.
```

```text
I need to handle a dependency install problem. Use $ai-collaboration-retro first and check whether we have already seen this kind of issue.
```

```text
I am about to change this area. Use $ai-collaboration-retro first and see whether there is already a best-practice file for it.
```

```text
I am starting this task. Use $ai-collaboration-retro first and tell me what this kind of problem should usually read before acting.
```

```text
I want to reorganize this retrospective. Use $ai-collaboration-retro to show me how it should be split into a README, best practices, deep dives, and a command cheatsheet.
```

```text
I want to audit this retro library. Use $ai-collaboration-retro first and tell me whether the routing order makes sense, and which files should be split or merged.
```

```text
I want to archive a newly discovered trap. Use $ai-collaboration-retro first and tell me which best-practice or deep-dive file it belongs in.
```

```text
Use $ai-collaboration-retro while I work on this, and if we discover a new trap, remind me whether it should be archived.
```

```text
I want to add a new lesson. Use $ai-collaboration-retro first and tell me whether it should extend an existing file or become a new deep-dive topic.
```

## Expected Output Shape

The skill is opinionated about the target knowledge-base structure:

```text
README.md              route table only
最佳实践/xx.md          one problem type, short default actions
专题/专题_xx.md         symptoms, root cause, case, prevention
命令速查.md             copyable commands only
基线定义文档/            reusable AGENTS/CLAUDE or project baselines
```

## Concrete Routing Example

If the current issue is `pip install` failure on Windows:

1. The skill should read the current project's local rules first.
2. If the user explicitly invokes this skill, it must read the retrospective library `README.md` once as the route table before reading any best-practice or deep-dive file.
3. If the route table has a similar problem, it should choose one focused file such as `最佳实践/02_依赖与包管理最佳实践.md`.
4. If the route table does not have a similar problem, it should return to normal project-local investigation instead of widening retro reads.
5. For install or download tasks, it should prefer mirror, wheel, version, and platform rules from the best-practice file before trying the default upstream source blindly.
6. Only if the default action is not enough should it escalate to a deeper file such as `专题/专题_平台兼容.md` or `专题/专题_依赖与版本.md`.
7. In the same response, it should explicitly tell the user that it read the route table and which best-practice file it matched; if nothing matched, it should say that too and note that it returned to normal project-local investigation.
8. If the work reveals a new trap, a stale rule, a missing command, or a reusable new default action, it should remind the user to decide whether that lesson should be archived back into the retrospective library.

The point is to avoid reading the whole retro library before acting.

## Guidance

- Only use the skill on projects or documents you are authorized to inspect.
- The skill is intentionally selective. It should not default to reading an entire large repository.
- Installation details may vary by AI tool, but the core workflow stays the same.
- The target structure is a route-first knowledge base:
  `README.md` for routing, `最佳实践/` for short default actions, `专题/` for root-cause notes, `命令速查.md` for copyable commands.
- It does not replace local project rules; during real project work, still read the current project's `AGENTS.md`, `CLAUDE.md`, `README.md`, or equivalent local constraints first.

## Contributing

Issues and pull requests are welcome, especially for:

- better trigger wording,
- clearer low-token routing patterns,
- additional validation checks for retrospective libraries,
- examples from other stacks or repository shapes.

Keep contributions focused on reusable patterns rather than project-specific stories.

## License

MIT. See [LICENSE](LICENSE).
