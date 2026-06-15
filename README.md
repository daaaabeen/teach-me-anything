# Teach Me Anything

> 一套跑在 Claude Code 上的**体系化学习工作流（Skill）**——把"学会一个新主题"从随缘看资料，变成有标准、有验收、可复用、防止"假学会"的流程。
>
> *A structured learning workflow for Claude Code — turn "learning something new" into a gated, verifiable, repeatable process.*

---

## 这是什么

普通做法学新知识，问题是**没有验收**：看完几篇文章、和 AI 聊几句，就以为懂了——实际经不起追问，过几天忘光。

这个 Skill 把学习设计成一条**带 gate 的流水线**：每个知识模块都要先经过"讲解 → 苏格拉底追问 → 测验"，**测验不过不许进下一模块**；全部学完还要做**费曼终验**（你反过来给 AI 讲，AI 扮演爱抬杠的外行专挑讲不圆的地方），**讲不圆不许结题**。学完自动产出速查笔记、分享 PPT 和进阶路线图。

核心理念：**用工程化的循环对抗"以为自己懂了"。**

## 核心特性

- 🎯 **目标分层**：开题先定级——L1 能聊 / L2 能讲 / L3 能用，验收标准随层级适配，不做无用功
- 🚧 **验收 gate**：测验不过不进下一模块，费曼终验不过不结题——从机制上杜绝"假学会"
- 🧠 **苏格拉底式**：先抛问题让你推理，再讲解，专门暴露"以为懂了其实没懂"
- 🪝 **基于已知教学**：所有讲解锚定到你已有的概念做类比，不照搬教材
- 💾 **进度落盘可续学**：状态写在 `state.md`，任何时候中断都能断点续学
- 📦 **结题三产物**：一页速查 `summary.md` + 分享 `slides.pptx` + 进阶路线 `roadmap.md`
- 🧪 **独立评判**：测验出题与费曼终验的"外行"由独立 agent 担任，避免讲解者自评放水
- 👀 **先展示后确认**：要你拍板的产物（地图、范围等）一定先完整呈现给你看，再问意见——绝不让你确认没看到的东西
- 🗣️ **流程透明**：开题先讲清流程怎么走，元流程术语首次出现就解释，不甩黑话

## 五阶段流程

```
① 开题 ──→ ② 建图 ──→ ③ 循环学习 ──→ ④ 实践(仅L3) ──→ ⑤ 结题
摸底定级    知识地图    逐模块            动手做             费曼终验
控范围      模块清单    讲解→追问→测验    Claude只评审       + 三产物
                       ↑过关才进下一个    不代写
```

完整做法（每阶段的输入、产出、完成判据）见 **[METHODOLOGY.md](./METHODOLOGY.md)**——它是整套工作流的唯一行为依据。

## 快速开始

### 1. 装上 Skill

把本仓库的内容放进你的 Claude Code 项目（或直接 clone 后在此目录里用）：

```bash
git clone <this-repo> teach-me-anything
cd teach-me-anything
```

关键文件：

- `.claude/skills/` —— Skill 本体（Claude Code 自动加载）
- `METHODOLOGY.md` —— 方法论唯一来源（Skill 运行时必读）
- `CLAUDE.md` —— 给 Claude 的项目级指引

> 想在已有项目里用：把 `.claude/skills/`、`METHODOLOGY.md`、`CLAUDE.md` 三者拷过去即可。

### 2. 开始学

在 Claude Code 里直接说：

| 命令 | 作用 |
|------|------|
| `/learn <主题>` | 开题学一个新主题，或断点续学已有主题 |
| `/learn` | 不带参数：自动续学进行中的主题 / 引导开新主题 |
| `/learn-status` | 只读：查看所有主题的学习进度总览 |

也可以直接自然语言触发：「我想学 RAG」「接着上次学」「我学到哪了」。

### 3. 看个真实例子

`topics/loop-engineering/` 是一个**已结题的完整样例**（主题：AI Agent 循环工程，L2 能讲）——里面有知识地图、逐模块笔记、测验错因记录、速查、进阶路线图和分享 PPT，可以照着看产出长什么样。

## 仓库结构

```
teach-me-anything/
├── README.md            # 你正在看的这份
├── METHODOLOGY.md       # ⭐ 方法论唯一来源（所有阶段做法）
├── CLAUDE.md            # 项目级 AI 指引
├── .claude/skills/      # Skill 本体
│   ├── SKILL.md         #   总入口：意图分发
│   ├── learn/SKILL.md   #   学习流程执行器
│   └── learn-status/SKILL.md  # 进度总览（只读）
├── topics/              # 学习档案，每主题一个子目录
│   └── loop-engineering/   # 已结题样例
│       ├── state.md     #   进度状态
│       ├── map.md       #   知识地图
│       ├── notes/       #   逐模块笔记
│       ├── summary.md   #   结题速查
│       ├── roadmap.md   #   进阶路线图
│       └── share/slides.pptx  # 分享 PPT
└── docs/                # 工作流自身的设计文档
```

## 设计取舍

- **只负责"学会/入门"**，不做间隔复习排期——长期查阅靠 `summary.md`，通往精通靠 `roadmap.md` 指路。
- **实践只评审不代写**（L3 技术类）——动手的是你，AI 当评审，确保真能用。
- **评判与执行分离**——检验环节交独立 agent，因为讲解者和自己的盲区共享假设，自评测不准。

## License

[MIT](./LICENSE)
