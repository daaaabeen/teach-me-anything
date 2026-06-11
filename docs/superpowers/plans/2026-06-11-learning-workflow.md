# 学习工作流（Teach Me Anything）实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 搭建一套 Claude 驱动的五阶段学习工作流：METHODOLOGY.md 方法论 + 两条命令（/learn、/learn-status）+ 主题档案目录约定，并用真实主题 dogfood 验证。

**Architecture:** METHODOLOGY.md 是唯一行为依据（人可读、工具无关）；`.claude/skills/` 下的两个 skill 是执行器，运行时必读方法论后按其执行；每个学习主题在 `topics/<slug>/` 有独立档案，进度记在人可读的 `state.md` 中实现断点续学。

**Tech Stack:** 纯 markdown 文档 + Claude Code 项目级 skills（`.claude/skills/<name>/SKILL.md`）。无代码、无依赖。

**Spec:** `docs/superpowers/specs/2026-06-11-learning-workflow-design.md`（所有行为细节以规格为准，本计划给出完整文件内容）

**验证策略说明:** 交付物为文档，不适用单元测试。每个任务用两层验证替代：(a) 结构校验——用 grep/ls 确认关键章节与文件存在、引用路径有效；(b) 终验 Task 6 用真实主题完整 dogfood。

---

## File Structure

| 文件 | 职责 |
|------|------|
| `METHODOLOGY.md` | 方法论唯一来源：核心原则、五阶段定义与完成判据、state.md 规范、边界处理、目录与命名约定 |
| `CLAUDE.md` | 极薄工作区说明：定位 + 指向 METHODOLOGY.md 和命令，不复制方法论内容 |
| `.claude/skills/learn/SKILL.md` | /learn 命令：路由（新主题开题 / 断点续学 / 无参数引导），执行时引用方法论 |
| `.claude/skills/learn-status/SKILL.md` | /learn-status 命令：扫描 topics/ 输出进度总览 |
| `topics/README.md` | 占位 + 一句话说明目录用途（git 不跟踪空目录，故用 README 占位） |

---

### Task 1: METHODOLOGY.md（方法论核心）

**Files:**
- Create: `METHODOLOGY.md`

- [ ] **Step 1: 写入完整方法论文档**

写入以下完整内容到 `METHODOLOGY.md`：

````markdown
# 学习方法论 — Teach Me Anything

> 本文档是这套学习工作流的**唯一行为依据**。`/learn` 与 `/learn-status` 是它的执行器：
> 执行任何学习活动前先读本文档，行为冲突时以本文档为准。修改学习行为 = 修改本文档。

## 核心原则

1. **目标分层**：每个主题开题时选定目标层级，验收环节按层级适配
   - **L1 能聊**：知道是什么、为什么重要、适用边界
   - **L2 能讲**：能费曼式把原理讲清楚，经得起追问
   - **L3 能用**：能动手做出东西或解决实际问题
2. **验收 gate**：测验不通过不进下一模块，费曼终验不通过不结题——杜绝「假学会」
3. **基于已知教学**：讲解必须锚定到学习者已有的概念做类比，不照搬教材
4. **苏格拉底式**：先抛引导性问题让学习者推理，再讲解；目的是暴露「以为懂了其实没懂」
5. **趁热沉淀**：每模块过关立即写笔记，不留到结题补写
6. **真实数据复用**：测验错题和卡点记入 state.md，结题 PPT 的「常见误区」页从中取材

## 五阶段流程

### ① 开题（一次对话完成）

| 项 | 内容 |
|----|------|
| 输入 | 用户给出想学的主题 |
| 做法 | 1) **摸底**：问 2-3 个问题探明已有基础与学习动机；2) **定目标**：用户选 L1/L2/L3；3) **控范围**：主题过大主动建议拆小（颗粒度：一两周可拿下），如「学 Rust」→ 建议「Rust 所有权模型」 |
| 产出 | `topics/<slug>/state.md` 初稿（按下方规范） |
| 完成判据 | 用户确认了目标层级和主题范围 |

**slug 约定**：Claude 提议简短英文 slug（如「RAG 检索增强生成」→ `rag`），用户确认后作为目录名；`state.md` 标题保留中文全名。

### ② 建图

| 项 | 内容 |
|----|------|
| 输入 | 开题确认的主题、层级、用户基础 |
| 做法 | 先研究（官方文档、权威资料），再输出知识地图：模块清单、依赖顺序、每模块的「学完你能回答什么问题」。**按层级裁剪**：L1 砍实现细节，L3 加实践模块 |
| 产出 | `topics/<slug>/map.md`，用户确认或修改后定稿 |
| 完成判据 | 用户确认地图；state.md 的模块进度清单与地图同步 |

### ③ 循环学习（核心循环，逐模块）

每个模块固定三步，顺序不可跳：

1. **讲解**：基于用户已有基础，用其熟悉的概念做类比
2. **苏格拉底追问**：抛 2-3 个引导性问题让用户推理（如「若把 X 换成 Y 会怎样？」），不直接往下讲
3. **测验 gate**：3-5 题（概念辨析 + 应用题），通过才标记模块完成
   - **不通过不重考原题**：定位错因 → 针对薄弱点换角度重讲 → 出新题
   - 错因要点记入 state.md 测验记录

每模块过关时：要点沉淀到 `notes/<模块序号>-<模块名>.md`，更新 state.md（勾选模块 + 过关日期 + 最后学习日期）。

### ④ 实践（仅 L3 主题）

按知识类型适配，产物放 `exercises/`：

| 知识类型 | 实践形式 |
|----------|----------|
| 技术/工程类 | 最小可运行项目；**Claude 只评审不代写** |
| AI/ML 原理类 | 手推关键公式，或实现玩具版本（如百行实现 attention） |
| 领域概念/软知识 | 取用户工作中的真实场景做案例分析 |

完成判据：产物可运行/推导完整/案例成文，且 Claude 评审通过。

### ⑤ 结题（费曼终验 + 双产物）

**费曼终验**：角色反转——用户向 Claude 讲整个主题，Claude 扮演「爱追问的外行」专挑讲不圆的地方。讲不圆的回炉补课后再验。

通过后产出两份：

1. **`summary.md`**：一页式自用速查笔记——核心模型、关键结论、易错点、延伸方向
2. **`share/slides.pptx`**：对外分享 PPT（默认生成，用户可跳过），结构按「教别人」设计：
   - 动机/背景（为什么值得关心）
   - 核心概念地图（取 map.md 主干）
   - 原理讲解（复用费曼终验中讲得最顺的表述与类比）
   - 实战/案例（L3 用 exercises/ 产物做演示素材）
   - 常见误区（取自 state.md 测验记录——亲自踩过的坑）
   - Q&A 预埋（预测听众提问 + 参考答案，放备注或附录）

结题后 state.md 当前阶段标记为「已结题」。

## state.md 规范

人可读 markdown（不用 JSON），每完成一个模块/阶段立即更新。模板：

```markdown
# <主题中文全名>

- 目标层级: L1 能聊 | L2 能讲 | L3 能用
- 当前阶段: ① 开题 | ② 建图 | ③ 循环学习 | ④ 实践 | ⑤ 结题 | 已结题
- 开题日期: YYYY-MM-DD
- 最后学习: YYYY-MM-DD

## 模块进度
- [x] 1. <模块名>（已过关 M/D）
- [ ] 2. <模块名> ← 当前
- [ ] 3. <模块名>

## 测验记录
- 模块N 第一次未过：<错因要点> → <处理结果>

## 变更日志
- M/D <变更内容>（如：目标从 L3 降为 L2，原因）
```

## 边界处理

- **中途降级目标**（如 L3 → L2）：随时允许，记入变更日志，地图相应裁剪
- **模块卡住三次以上**：主动建议拆出前置子主题先学（新开一个 topic）
- **主题过大**：开题阶段即建议拆分，不带病开工
- **明确不做**：间隔复习/复习排期。工作流只负责「学会」，长期查阅靠 summary.md

## 目录约定

```
topics/<slug>/
├── state.md      # 进度状态（本规范）
├── map.md        # 知识地图
├── notes/        # 逐模块笔记
├── exercises/    # 实践产物（仅 L3）
├── summary.md    # 结题速查
└── share/
    └── slides.pptx  # 分享 PPT
```
````

- [ ] **Step 2: 结构校验**

Run: `grep -c '^### [①②③④⑤]' METHODOLOGY.md && grep -l 'slug 约定' METHODOLOGY.md && grep -l '不通过不重考原题' METHODOLOGY.md`
Expected: 第一条输出 `5`（五个阶段标题齐全），后两条输出文件名（关键规则在位）

- [ ] **Step 3: Commit**

```bash
git add METHODOLOGY.md
git commit -m "feat: add learning methodology (five-phase workflow)"
```

---

### Task 2: CLAUDE.md + topics/ 脚手架

**Files:**
- Create: `CLAUDE.md`
- Create: `topics/README.md`

- [ ] **Step 1: 写入极薄 CLAUDE.md**

````markdown
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

这是一个**学习工作区**，不是软件项目。用途：通过可复用工作流体系化学习新知识。

## 规则

- **方法论唯一来源是 `METHODOLOGY.md`**——执行任何学习活动前先读它，本文件不复制其内容
- 命令：`/learn [主题]`（开题或断点续学）、`/learn-status`（进度总览）
- 每个主题的档案在 `topics/<slug>/`，进度以各自的 `state.md` 为准
- 学习产物（state.md、notes、summary、PPT）随学习进程提交 git
````

- [ ] **Step 2: 写入 topics/README.md**

````markdown
# topics/

每个学习主题一个子目录（slug 命名），结构见根目录 `METHODOLOGY.md` 的「目录约定」。
由 `/learn` 工作流自动创建和维护。
````

- [ ] **Step 3: 结构校验**

Run: `wc -l CLAUDE.md && grep -L '五阶段' CLAUDE.md`
Expected: CLAUDE.md 在 20 行以内；第二条输出 `CLAUDE.md`（即文件中**不含**「五阶段」字样——证明没把方法论内容复制进来）

- [ ] **Step 4: Commit**

```bash
git add CLAUDE.md topics/README.md
git commit -m "feat: add workspace guide and topics scaffold"
```

---

### Task 3: /learn skill

**Files:**
- Create: `.claude/skills/learn/SKILL.md`

- [ ] **Step 1: 写入 skill 文件**

````markdown
---
name: learn
description: Use when 用户想学习新主题或继续学习——触发词：/learn、学习 X、我想学 X、继续学、接着上次学。驱动五阶段学习工作流（开题、建图、循环学习、实践、结题）。
---

# /learn — 学习工作流执行器

<HARD-RULE>
第一步永远是完整读取仓库根目录的 `METHODOLOGY.md`。
本 skill 只定义路由和铁律，所有阶段的具体做法、产出与完成判据以方法论为准。
</HARD-RULE>

## 路由

收到 `/learn [参数]` 后：

1. **有参数，且匹配已有主题**（参数与 `topics/` 下某个 slug 或某个 `state.md` 的标题模糊匹配）
   → 读该主题 `state.md`，向用户播报当前进度（阶段 + 模块位置），从断点继续
2. **有参数，且是新主题**
   → 走方法论「① 开题」：摸底 → 定层级（用 AskUserQuestion 提供 L1/L2/L3 选项）→ 控范围 → 提议 slug → 创建 `topics/<slug>/state.md`
3. **无参数**
   → 扫描 `topics/*/state.md`，按「当前阶段」字段分类：
   - 0 个进行中主题 → 引导用户开新主题（问想学什么）
   - 1 个进行中 → 直接续学该主题
   - 多个进行中 → 列表（主题/层级/阶段/最后学习日期）让用户选

## 执行铁律

按方法论执行当前阶段时，以下规则不可妥协：

- **测验 gate 不通过不进下一模块**；不通过不重考原题，换角度重讲后出新题
- **苏格拉底先问后讲**：每模块讲解后必有 2-3 个引导性追问，用户回应后才进测验
- **趁热沉淀**：模块过关立即写 `notes/`，立即更新 `state.md`（含错题记录）
- **状态先行**：每完成一个模块或阶段，先更新 `state.md` 再继续——保证任何时刻中断都可续学
- **实践环节只评审不代写**（L3 技术类）
- 结题 PPT 用 pptx 能力生成到 `share/slides.pptx`，生成前确认用户是否需要
````

- [ ] **Step 2: 结构校验**

Run: `head -4 .claude/skills/learn/SKILL.md | grep -c '^---\|^name:\|^description:' && grep -l 'METHODOLOGY.md' .claude/skills/learn/SKILL.md`
Expected: 第一条 ≥2（frontmatter 完整）；第二条输出文件名（指向方法论）

- [ ] **Step 3: Commit**

```bash
git add .claude/skills/learn/SKILL.md
git commit -m "feat: add /learn skill (routing + execution rules)"
```

---

### Task 4: /learn-status skill

**Files:**
- Create: `.claude/skills/learn-status/SKILL.md`

- [ ] **Step 1: 写入 skill 文件**

````markdown
---
name: learn-status
description: Use when 用户想查看学习进度总览——触发词：/learn-status、学习进度、我学到哪了、学习情况。只读汇报，不进入学习流程。
---

# /learn-status — 学习进度总览

只读命令：扫描 `topics/*/state.md`，汇总输出，不修改任何文件、不进入学习流程。

## 输出格式

按「进行中」「已结题」分组，各输出一张表：

| 主题 | 目标层级 | 当前阶段 | 模块进度 | 最后学习 |
|------|----------|----------|----------|----------|
| RAG 检索增强生成 | L2 能讲 | ③ 循环学习 | 2/4 | 2026-06-15 |

- 模块进度 = state.md 中已勾选模块数 / 总模块数（②建图前显示 `-`）
- `topics/` 为空时：说明还没有学习主题，提示用 `/learn <主题>` 开始
- 结尾提示：可用 `/learn` 继续进行中的主题
````

- [ ] **Step 2: 结构校验**

Run: `grep -l '只读' .claude/skills/learn-status/SKILL.md && grep -c 'state.md' .claude/skills/learn-status/SKILL.md`
Expected: 第一条输出文件名；第二条 ≥2

- [ ] **Step 3: Commit**

```bash
git add .claude/skills/learn-status/SKILL.md
git commit -m "feat: add /learn-status skill (progress overview)"
```

---

### Task 5: 整体结构验证

**Files:** 无新增（只验证）

- [ ] **Step 1: 全量文件存在性校验**

Run: `ls METHODOLOGY.md CLAUDE.md topics/README.md .claude/skills/learn/SKILL.md .claude/skills/learn-status/SKILL.md`
Expected: 五个路径全部列出，无 "No such file"

- [ ] **Step 2: 交叉引用校验**

Run: `grep -l 'METHODOLOGY' CLAUDE.md .claude/skills/learn/SKILL.md && grep -rl 'learn-status' CLAUDE.md`
Expected: CLAUDE.md 和 learn skill 都指向 METHODOLOGY；CLAUDE.md 提到两条命令

- [ ] **Step 3: skill 可发现性验证（需用户参与，不可由 subagent 自报通过）**

新开一个 Claude Code session（或由用户在本 session 输入 `/learn-status`），确认两个 skill 出现在可用技能列表且可触发。
Expected: `/learn-status` 正确回答「还没有学习主题」。

- [ ] **Step 4: Commit（如有修正）**

```bash
git add -A && git commit -m "fix: structural verification fixes" || echo "nothing to fix"
```

---

### Task 6: Dogfood 终验（需用户参与）

> 本任务**不能由 subagent 独立完成**——需要用户真实参与学习。这是规格定义的交付标准：「跑通才算交付」。

- [ ] **Step 1: 用户选一个真实想学的小主题（建议 L1 或 L2）**

由用户输入 `/learn <主题>`，走完 ① 开题 ② 建图。
Expected: `topics/<slug>/state.md` 与 `map.md` 生成且符合 METHODOLOGY.md 规范。

- [ ] **Step 2: 完成至少一个模块的完整循环**

讲解 → 苏格拉底追问 → 测验过关。
Expected: `notes/` 出现模块笔记，state.md 勾选该模块。

- [ ] **Step 3: 故意中断 session，验证断点续学**

新开 session 输入 `/learn`。
Expected: 准确播报进度并从断点（下一模块）继续，无重复开题。

- [ ] **Step 4: 走完剩余模块与结题**

费曼终验 → 生成 `summary.md` + `share/slides.pptx`。
Expected: 双产物生成；PPT 含六段结构，「常见误区」页取材自真实错题记录。

- [ ] **Step 5: 复盘修正**

dogfood 中发现的流程问题回写到 METHODOLOGY.md / skill 文件。

```bash
git add -A && git commit -m "feat: dogfood-verified learning workflow"
```
