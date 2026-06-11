# 知识地图：AI Agent 循环工程

- 目标层级：L2 能讲
- 状态：已定稿（2026-06-11）
- 模块依赖：1 → 2 → (3、4 可换序) → 5

> **全图主线**：agentic loop（代理循环）是**机制**——agent 内部「思考→工具→观察」的循环本身，2022 年就存在；loop engineering（循环工程）是**工程实践**——2026 年得名，指设计外层系统来驱动和驾驭这些循环。模块 1-2 讲机制（内循环），模块 3-5 讲实践（外循环）。不懂内循环怎么转，就设计不好外循环。

## 模块 1：从一次调用到循环——agent 的本质

**内容**：LLM 单次调用 → 固定 workflow → agent loop，三者的本质区别；为什么「agent = 模型 + 工具 + 循环」；你每次给 Claude Code 发消息时，它内部跑的就是一个循环（思考 → 调工具 → 看结果 → 再思考……直到任务完成）。

**学完能答**：
- Claude Code 和「网页版 ChatGPT 问答」的根本区别在哪？
- 为什么说循环（而不是模型本身）是 agent 能力的来源？
- agentic loop 和 loop engineering 是什么关系？内循环和外循环分别指什么？

## 模块 2：循环的解剖学

**内容**：一个 loop = 触发器 + 可验证目标 + 「规划 → 行动 → 观察 → 验证 → 终止」；六大构件：自动化任务（定时触发）、工作树（并行隔离）、技能（SKILL.md 沉淀项目知识）、连接器（MCP 接外部系统）、子代理（分工）、状态管理（跨次记忆）；关键设计原则：**评判者和执行者分离**——写代码的 agent 不能自己给自己打分。

**学完能答**：
- loop 和 cron 定时任务的区别是什么？
- 为什么验证环节要用独立的模型/子代理？

## 模块 3：记忆与状态——循环的持久化

**内容**：上下文窗口是易失的、有限的；磁盘是持久的——「agent 会忘，但仓库不会」；长任务必须把进度状态写到盘上（markdown、看板）才能跨 session 续作；你正在用的这套学习工作流的 `state.md` 就是同一个设计思想。

**学完能答**：
- 为什么长任务必须把状态写到盘上而不是留在对话里？
- context engineering 和 loop engineering 各管什么？

## 模块 4：失败模式与防护

**内容**：五种典型失败：死循环、无进展空转、目标漂移、上下文溢出、成本失控（单 agent ≈ 4 倍普通对话 token，多 agent ≈ 15 倍）；防护四支柱：硬性迭代上限、token 预算、无进展检测、明确终止条件。

**学完能答**：
- AutoGPT（2023）当年为什么不可用？
- 一个生产级 loop 至少要装哪几道保险？

## 模块 5：演进脉络与人的位置

**内容**：ReAct（2022，推理+行动交替）→ AutoGPT（2023，概念验证）→ Reflexion / OODA / 多代理（2024-25）→ loop engineering（2026.6，Peter Steinberger 提出、Addy Osmani 系统化、Boris Cherny："我已经不提示 Claude 了，是循环在提示它"）；prompt → context → loop 三层工程范式的递进；理解债务（comprehension debt）与认知投降的风险——人从「提示者」变成「循环设计者 + 最终验证者」。

**学完能答**：
- 为什么说 loop engineering 在 prompt engineering 的「楼上」？
- 全自动化之后，人还剩什么不可让渡的责任？

## 主要来源

- [Addy Osmani — Loop Engineering](https://addyosmani.com/blog/loop-engineering/)（概念系统化原文）
- [Agentic Loops: From ReAct to Loop Engineering (2026 Guide)](https://datasciencedojo.com/blog/agentic-loops-explained-from-react-to-loop-engineering-2026-guide/)（演进史）
- [Lushbinary — Loop Engineering: The Guide for AI Agents](https://lushbinary.com/blog/loop-engineering-ai-coding-agents-guide/)
