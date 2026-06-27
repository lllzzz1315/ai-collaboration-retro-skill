# ai-collaboration-retro-skill

`ai-collaboration-retro` is a Codex skill for turning project experience into a low-token AI collaboration knowledge base, and for routing what AI should read first during real project work.

中文一句话：
把项目经验整理成 AI 更容易读、上下文更省、排查更快的复盘知识库，并在真实项目里告诉 AI 先读什么、后读什么。

It is designed for teams that want to:

- extract reusable lessons from authorized repositories,
- reorganize retrospective notes into a cleaner `README -> 最佳实践 -> 专题` structure,
- reduce context waste during AI-assisted debugging and implementation,
- make AI reading order explicit instead of relying on full-repo scanning.

## Why This Exists

Most AI-assisted project work wastes context in the same ways:

- the model reads too many files before it has a route,
- "best practices" and root-cause cases are mixed together,
- commands are buried inside long notes,
- issue IDs collide or drift,
- every new session has to rediscover the same project lessons.

This skill helps compress that memory into a route-first structure that future AI sessions can use with less token burn and less guesswork.

## What The Skill Does

The skill supports two primary workflows and one audit workflow:

- `Retrospective extraction`
  Read an authorized project selectively, identify reusable engineering and AI-collaboration lessons, and organize them into a low-token knowledge base.
- `Project-use routing`
  Use an existing retrospective library during day-to-day work to decide what to read first, what to skip, and when to escalate from a short guide to a deep-dive issue note.
- `Knowledge-base audit`
  Review an existing retro library and suggest route fixes, file splitting, merging, ID cleanup, and link cleanup.

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

- teams using Codex or similar agents on medium-to-large repositories,
- maintainers building internal AI collaboration playbooks,
- people who already keep project retrospectives but want them to be more usable by AI,
- anyone tired of re-explaining the same environment, dependency, or workflow traps every session.

## Repository Layout

```text
ai-collaboration-retro-skill/
├── LICENSE
├── README.md
└── ai-collaboration-retro/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## Install

Clone this repository, then copy the `ai-collaboration-retro/` folder into your local Codex skills directory.

Typical locations:

- Windows: `%USERPROFILE%\\.codex\\skills\\`
- macOS/Linux: `~/.codex/skills/`

If the `skills/` directory does not exist yet, create it first.

Example:

```powershell
git clone https://github.com/<your-name>/ai-collaboration-retro-skill.git
Copy-Item -Recurse .\ai-collaboration-retro-skill\ai-collaboration-retro $env:USERPROFILE\.codex\skills\
```

```bash
git clone https://github.com/<your-name>/ai-collaboration-retro-skill.git
cp -R ./ai-collaboration-retro-skill/ai-collaboration-retro ~/.codex/skills/
```

## Quick Start

1. Install the skill into your local Codex `skills/` directory.
2. Open a project or retrospective library you are authorized to inspect.
3. Use one of the prompts below.
4. Let the skill decide whether the task is:
   `Retrospective extraction`, `Project-use routing`, or `Knowledge-base audit`.

## Example Prompts

```text
Use $ai-collaboration-retro to review this authorized project and extract a low-token AI collaboration retrospective.
```

```text
Use $ai-collaboration-retro to tell me what AI should read first in this project before fixing a dependency issue.
```

```text
Use $ai-collaboration-retro to audit this retrospective library and suggest split, merge, and routing fixes.
```

```text
Use $ai-collaboration-retro to convert this oversized retrospective note into a README -> 最佳实践 -> 专题 structure.
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
2. Then it should read the retrospective library `README.md` as a route table.
3. Then it should choose one focused file such as `最佳实践/02_依赖与包管理最佳实践.md`.
4. Only if the default action is not enough should it escalate to a deeper file such as `专题/专题_平台兼容.md` or `专题/专题_依赖与版本.md`.

The point is to avoid reading the whole retro library before acting.

## Guidance

- Only use the skill on projects or documents you are authorized to inspect.
- The skill is intentionally selective. It should not default to reading an entire large repository.
- The target structure is a route-first knowledge base:
  `README.md` for routing, `最佳实践/` for short default actions, `专题/` for root-cause notes, `命令速查.md` for copyable commands.

## Contributing

Issues and pull requests are welcome, especially for:

- better trigger wording,
- clearer low-token routing patterns,
- additional validation checks for retrospective libraries,
- examples from other stacks or repository shapes.

Keep contributions focused on reusable patterns rather than project-specific stories.

## License

MIT. See [LICENSE](LICENSE).
