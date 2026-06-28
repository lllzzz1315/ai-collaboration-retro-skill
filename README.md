# ai-collaboration-retro-skill

English version: [readme_en.md](readme_en.md)

`ai-collaboration-retro` 是一个通用的 AI 协作复盘 skill。它的目标不是多写一份复盘文档，而是把项目里已经踩过的坑、验证过的流程、容易误读的规则，整理成 AI 可以快速读取和复用的工作路线。

一句话理解：
让 AI 在后续协作中少重复踩坑，少发错误或多余的请求，少读无关上下文，并尽量降低 token 消耗。

## 这个 skill 解决什么问题

这个 skill 解决的是：项目里明明已经积累了很多经验、坑和规则，但 AI 每次接手时仍然不知道该先读哪里、哪些是默认做法、哪些只是历史案例，于是又开始重复试错。

它把过往踩坑经验变成 AI 可以稳定复用的读取路线。这样后续使用 AI 处理同类问题时，可以更快命中正确上下文，少走错误路径，少发不必要的追问、试错和重复执行请求。

换句话说，它关心的不是“文档更完整”，而是：

- AI 是否能在更少上下文里找到正确入口。
- AI 是否能先使用已验证的默认动作，而不是重新猜。
- AI 是否能知道哪些内容只是案例，哪些内容应该直接指导当前操作。
- AI 是否能在需要深挖时再读取根因，而不是一开始就吞掉整份复盘库。

常见表现是：

- 项目复盘越写越长，AI 为了找一个结论要读很多无关内容。
- 环境、依赖、脚本、构建、AI 生成质量这些坑反复出现，但没有稳定的读取入口。
- “怎么做”的短规则和“为什么会这样”的根因案例混在一起，导致上下文很快膨胀。
- 命令散落在专题笔记里，AI 容易临时拼命令或复制过时命令。
- 坑号、链接、专题归类不稳定，后续维护时越补越乱。
- 在真实项目里，AI 不知道该先读项目规则、复盘 README、最佳实践，还是直接深挖专题。

这个 skill 的目标，是把这些经验压缩成一套 route-first 的结构，让未来的 AI 会话能按顺序读取：先路由，再默认动作，必要时才读根因。最终收益是更少上下文、更少 token、更少重复错误和更少无效请求。

## 它能做什么

这个 skill 支持两种主要工作流和一种审计工作流：

- `复盘提炼模式`
  有权限读取项目后，选择性提炼出会影响后续 AI 操作的工程经验、协作规则和已知坑，并整理成低 token 知识库。
- `项目使用模式`
  在真实项目里使用现有复盘库，决定先读什么、跳过什么、先执行哪个默认动作，以及什么时候才需要升级到专题深挖。
- `知识库审计模式`
  审查现有复盘库结构，找出会导致 AI 多读、误读、重复试错的地方，并给出路由修正、文件拆分/合并、坑号清理和链接清理建议。

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

- 在中大型仓库里使用 Codex、Claude Code 或其他 agent 的团队，
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
    ├── local-config.example.yaml
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## 安装方式

克隆这个仓库后，把 `ai-collaboration-retro/` 整个目录复制到你当前 AI 工具使用的 skills 目录，或者直接把 `SKILL.md` 作为通用提示规则使用。

对支持 skill 目录的工具，常见位置例如：

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

## 平台说明

这个仓库的核心是 `ai-collaboration-retro/SKILL.md` 中的方法论，它本身是通用的。

- 如果你的 AI 工具支持原生 skills，就把整个 `ai-collaboration-retro/` 目录安装进去。
- 如果你的 AI 工具不支持原生 skills，也可以直接读取 `SKILL.md`，把它作为长期工作提示或团队协作规则使用。
- `agents/openai.yaml` 只是某些工具会用到的平台元数据，不影响核心方法论本身的复用。

## 安装后先配路径

这个 skill 现在约定用一个本地配置文件来记录“复盘知识库路径”。

默认位置：

```text
~/.codex/skills/ai-collaboration-retro/local-config.yaml
```

模板文件：

```text
~/.codex/skills/ai-collaboration-retro/local-config.example.yaml
```

你需要在 `local-config.yaml` 里填写你的复盘知识库绝对路径，例如：

```yaml
knowledge_base:
  path: "D:/Obsidian-Project/Obsidian/04_AI协作复盘"
  expected_entry: "README.md"
  requires_authorized_read: true
```

注意两点：

- 这个路径需要是你希望 skill 实际读取的复盘知识库路径。
- 这个路径必须是当前 AI 工具已经明确授权可读的路径；没授权时，skill 应该先提醒授权，而不是直接继续读。

推荐的首次配置动作：

1. 把 `local-config.example.yaml` 复制为 `local-config.yaml`。
2. 把 `knowledge_base.path` 改成你自己的复盘知识库绝对路径。
3. 确认这个路径已经授权 AI 可读。
4. 如果这个路径下还没有 `README.md`、`最佳实践/`、`专题/` 等核心结构，就先生成知识库。

skill 首次使用时，建议按下面这个顺序提示用户：

```text
先检查 local-config.yaml。
没有配置就提示是否生成。
有配置但没授权就提示先授权。
有配置有授权，但知识库还没生成就提示先生成知识库。
```

## 快速开始

1. 把 skill 安装到本地 AI 工具的 `skills/` 目录，或让工具读取 `SKILL.md`。
2. 先检查 `local-config.yaml` 是否存在；如果不存在，就从 `local-config.example.yaml` 生成一份并填写知识库路径。
3. 确认该知识库路径已经授权 AI 可读。
4. 打开你有权限查看的项目或复盘库。
5. 直接用下面这种更口语、更直达目标的提示词。
6. 让 skill 自动判断当前任务属于 3 个场景之一：
   `先查现成经验`、`生成或整理知识库`、`审查或归档更新`。

## 首次使用会发生什么

第一次真正使用这个 skill 时，理想行为是：

1. 先查 `local-config.yaml`。
2. 如果没有，就提醒你是否生成配置。
3. 如果有配置，但路径没授权，就提醒你先授权读取。
4. 如果有配置也有授权，但知识库路径下没有内容，就提醒你先生成知识库。
5. 只有配置、授权、知识库都就绪后，才开始正式走路由。

## 超短提示词

```text
我要做这件事，先用 $ai-collaboration-retro 看看有没有现成经验。
```

```text
我要整理这个项目的经验，先用 $ai-collaboration-retro 生成或更新对应的知识库。
```

```text
我要审查或补充这份知识库，先用 $ai-collaboration-retro 看看该拆、该合，还是该把新坑归档进去。
```

## 提示词示例
只保留 3 类场景：

1. 先查现成经验

```text
我要做这件事，先用 $ai-collaboration-retro 看看有没有现成经验；有的话告诉我该读哪个最佳实践。
```

2. 生成或整理知识库

```text
我要把这个项目的经验整理成知识库，先用 $ai-collaboration-retro 帮我生成或更新对应的 README、最佳实践、专题和命令速查。
```

3. 审查或归档更新

```text
我要审查这份知识库，顺便看看这次新坑应该补到哪里；如果需要，提醒我归档。
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
2. 再检查 `local-config.yaml` 是否存在，以及里面配置的知识库路径是否已授权可读。
3. 如果没有配置，就提醒用户先生成配置。
4. 如果配置有了，但知识库路径下还没有 `README.md` 或核心结构，就提醒用户先生成对应知识库。
5. 如果知识库可用，且用户显式指定使用这个 skill，就先读一次知识库 `README.md` 当作路由表。
6. 如果路由表里能命中相似问题，就只选一个聚焦文件，例如 `最佳实践/02_依赖与包管理最佳实践.md`。
7. 如果路由表里没有相似问题，就回到项目本地继续排查，而不是额外全读复盘库。
8. 对下载或安装问题，优先执行最佳实践里已经写明的镜像站、wheel、版本或平台规则。
9. 只有默认动作不够时，才升级去读 `专题/专题_平台兼容.md` 或 `专题/专题_依赖与版本.md` 这类深挖文件。
10. 在当次回复里明确告诉用户：本次已经读取路由，以及命中了哪个最佳实践；如果没有命中，也要说明没有命中并已回到项目本地排查。
11. 如果本次操作发现了新的坑、旧规则失效、命令缺失，或产生了值得复用的新默认做法，要提醒用户是否需要把经验归档进复盘库。

重点就是：不要在行动前把整个复盘库全读一遍。

## 使用边界

- 只在你有权限查看的项目或文档上使用这个 skill。
- 这个 skill 的设计目标是“选择性读取”，不是默认全量扫描大仓库。
- 不同 AI 工具的安装方式可能不同，但核心工作流保持不变。
- 它推崇的是 route-first 知识库：
  `README.md` 负责路由，`最佳实践/` 放短动作，`专题/` 放根因，`命令速查.md` 放命令。
- 它不替代项目自身规则；真实项目中仍然先读本项目的 `AGENTS.md`、`CLAUDE.md`、`README.md` 等本地约束。

## 参与改进

欢迎提 issue 或 PR，尤其是这些方向：

- 更好的触发词设计，
- 更清晰的低 token 路由模式，
- 更多适用于复盘库的校验规则，
- 来自其他技术栈或仓库形态的示例。

尽量提交“可复用模式”，而不是仅针对某个具体项目的故事性内容。

## License

MIT。见 [LICENSE](LICENSE)。
