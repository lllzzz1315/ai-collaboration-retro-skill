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
    ├── local-config.example.yaml
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

## Configure The Knowledge-Base Path

This skill now expects a local config file that stores the path to the retrospective knowledge base.

Default location:

```text
~/.codex/skills/ai-collaboration-retro/local-config.yaml
```

Template file:

```text
~/.codex/skills/ai-collaboration-retro/local-config.example.yaml
```

Example:

```yaml
knowledge_base:
  path: "D:/Obsidian-Project/Obsidian/04_AI协作复盘"
  expected_entry: "README.md"
  requires_authorized_read: true
```

Two things matter:

- `knowledge_base.path` must point to the actual retro library this skill should use.
- The AI tool must be explicitly authorized to read that path before the skill can use it.

Recommended first-time setup:

1. Copy `local-config.example.yaml` to `local-config.yaml`.
2. Update `knowledge_base.path` to your actual retro-library path.
3. Make sure that path is authorized for AI read access.
4. If that path does not yet contain `README.md`, `最佳实践/`, `专题/`, or the expected core structure, generate the knowledge base first.

On first use, the skill should guide the user in this order:

```text
Check local-config.yaml first.
If missing, ask whether to generate it.
If present but unauthorized, ask for read authorization.
If present and authorized but the knowledge base is missing, tell the user to generate the knowledge base first.
```

## Quick Start

1. Install the skill into your AI tool's local `skills/` directory, or make the tool read `SKILL.md`.
2. Check whether `local-config.yaml` exists; if not, generate it from `local-config.example.yaml` and fill in the knowledge-base path.
3. Make sure that path is authorized for AI read access.
4. Open a project or retrospective library you are authorized to inspect.
5. Use one of the prompts below.
6. Let the skill decide whether the task is one of 3 scenarios:
   `Check existing lessons first`, `Generate or reorganize the knowledge base`, or `Audit or archive updates`.

## What Should Happen On First Use

The expected first-use behavior is:

1. Check `local-config.yaml`.
2. If it is missing, ask whether to generate it.
3. If it exists but the configured path is not authorized, ask for authorization first.
4. If it exists and is authorized but the knowledge base is not ready, tell the user to generate the knowledge base first.
5. Only after config, authorization, and knowledge-base structure are ready should the skill start normal routing.

## Short Prompt Cheatsheet

```text
I want to work on this task. Use $ai-collaboration-retro first and check whether we already have a reusable lesson.
```

```text
I want to turn this project's lessons into a knowledge base. Use $ai-collaboration-retro to generate or refresh it.
```

```text
I want to audit or update this knowledge base. Use $ai-collaboration-retro first and tell me whether this new trap should be archived.
```

## Example Prompts
Keep the prompts focused on 3 scenarios:

1. Check existing lessons first

```text
I want to work on this task. Use $ai-collaboration-retro first, check whether we already have a reusable lesson, and tell me which best-practice file matches.
```

2. Generate or reorganize the knowledge base

```text
I want to turn this project's lessons into a knowledge base. Use $ai-collaboration-retro to generate or refresh the README, best practices, deep dives, and command cheatsheet.
```

3. Audit or archive updates

```text
I want to audit this knowledge base and see where this new trap belongs. Use $ai-collaboration-retro first, and remind me if it should be archived.
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
2. Then it should check whether `local-config.yaml` exists and whether the configured knowledge-base path is authorized for read access.
3. If the config is missing, it should tell the user to generate it first.
4. If the config exists but the knowledge-base path does not contain `README.md` or the expected structure, it should tell the user to generate the knowledge base first.
5. If the knowledge base is ready and the user explicitly invokes this skill, it must read the knowledge-base `README.md` once as the route table before reading any best-practice or deep-dive file.
6. If the route table has a similar problem, it should choose one focused file such as `最佳实践/02_依赖与包管理最佳实践.md`.
7. If the route table does not have a similar problem, it should return to normal project-local investigation instead of widening retro reads.
8. For install or download tasks, it should prefer mirror, wheel, version, and platform rules from the best-practice file before trying the default upstream source blindly.
9. Only if the default action is not enough should it escalate to a deeper file such as `专题/专题_平台兼容.md` or `专题/专题_依赖与版本.md`.
10. In the same response, it should explicitly tell the user that it read the route table and which best-practice file it matched; if nothing matched, it should say that too and note that it returned to normal project-local investigation.
11. If the work reveals a new trap, a stale rule, a missing command, or a reusable new default action, it should remind the user to decide whether that lesson should be archived back into the retrospective library.

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
