# ai-collaboration-retro-skill

English version: [readme_en.md](readme_en.md)

`ai-collaboration-retro` 是一个 Codex skill，用来把项目经验整理成低 token 的 AI 协作知识库，并在真实项目工作中告诉 AI 先读什么、后读什么。

一句话理解：
把项目经验整理成 AI 更容易读、上下文更省、排查更快的复盘知识库，并在真实项目里明确读取顺序。

## 这个 skill 解决什么问题

很多 AI 辅助项目协作会反复浪费上下文：

- 模型在没有路由的情况下读了太多文件，
- “最佳实践”和根因案例混在同一份长文档里，
- 可复制命令埋在大段叙述里，
- 坑号重复、漂移或跨文件冲突，
- 每次新会话都要重新踩一遍环境和依赖问题。

这个 skill 的目标，是把这些经验压缩成一套 route-first 的结构，让未来的 AI 会话少读、少猜、少试错。

## 它能做什么

这个 skill 支持两种主要工作流和一种审计工作流：

- `复盘提炼模式`
  有权限读取项目后，选择性提炼出可复用的工程和 AI 协作经验，并整理成低 token 知识库。
- `项目使用模式`
  在真实项目里使用现有复盘库，决定先读什么、跳过什么，什么时候从短指引升级到专题深挖。
- `知识库审计模式`
  审查现有复盘库结构，给出路由修正、文件拆分/合并、坑号清理和链接清理建议。

## 改造前后是什么样

典型的“改造前”：

- 一份很长的复盘笔记，命令、案例、结论全混在一起，
- 没有清晰的读取顺序，
- 坑号重复，
- AI 必须先把长文档读很多才能开始行动，
- 同样的环境或依赖坑每次会话都要重新发现。

典型的“改造后”：

- `README.md` 负责路由，
- `最佳实践/` 一类问题一个短文件，
- `专题/` 负责根因、案例和预防，
- `命令速查.md` 专门放可复制命令，
- AI 通常只需要先读一个路由文件和一个聚焦的最佳实践文件就能开始解决问题。

## 适合谁

- 在中大型仓库里使用 Codex 或类似 agent 的团队，
- 想把内部 AI 协作经验沉淀成可复用手册的维护者，
- 已经在写复盘，但希望这些文档对 AI 更友好的人，
- 不想每次会话都重新解释环境、依赖和流程坑的人。

## 仓库结构

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

## 安装方式

克隆这个仓库后，把 `ai-collaboration-retro/` 整个目录复制到本地 Codex 的 skills 目录。

常见位置：

- Windows: `%USERPROFILE%\\.codex\\skills\\`
- macOS/Linux: `~/.codex/skills/`

如果 `skills/` 目录还不存在，先创建它。

示例：

```powershell
git clone https://github.com/lllzzz1315/ai-collaboration-retro-skill.git
Copy-Item -Recurse .\ai-collaboration-retro-skill\ai-collaboration-retro $env:USERPROFILE\.codex\skills\
```

```bash
git clone https://github.com/lllzzz1315/ai-collaboration-retro-skill.git
cp -R ./ai-collaboration-retro-skill/ai-collaboration-retro ~/.codex/skills/
```

## 快速开始

1. 把 skill 安装到本地 Codex `skills/` 目录。
2. 打开你有权限查看的项目或复盘库。
3. 使用下面的提示词之一。
4. 让 skill 自动判断当前任务属于：
   `复盘提炼模式`、`项目使用模式` 或 `知识库审计模式`。

## 提示词示例

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

## 目标输出结构

这个 skill 对目标知识库结构有明确偏好：

```text
README.md              路由表，只负责指路
最佳实践/xx.md          一类问题一个短文件，只写默认动作
专题/专题_xx.md         现象、根因、案例、预防
命令速查.md             只放可复制命令
基线定义文档/            可复用 AGENTS/CLAUDE 或项目基线
```

## 一个具体路由例子

如果当前问题是 Windows 上 `pip install` 失败：

1. 先读当前项目自己的本地规则。
2. 再把复盘库的 `README.md` 当作路由表来读。
3. 然后只选一个聚焦文件，例如 `最佳实践/02_依赖与包管理最佳实践.md`。
4. 只有默认动作不够时，才升级去读 `专题/专题_平台兼容.md` 或 `专题/专题_依赖与版本.md` 这类深挖文件。

重点就是：不要在行动前把整个复盘库全读一遍。

## 使用边界

- 只在你有权限查看的项目或文档上使用这个 skill。
- 这个 skill 的设计目标是“选择性读取”，不是默认全量扫描大仓库。
- 它推崇的是 route-first 知识库：
  `README.md` 负责路由，`最佳实践/` 放短动作，`专题/` 放根因，`命令速查.md` 放命令。

## 参与改进

欢迎提 issue 或 PR，尤其是这些方向：

- 更好的触发词设计，
- 更清晰的低 token 路由模式，
- 更多适用于复盘库的校验规则，
- 来自其他技术栈或仓库形态的示例。

尽量提交“可复用模式”，而不是仅针对某个具体项目的故事性内容。

## License

MIT。见 [LICENSE](LICENSE)。
