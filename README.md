# 智能体记忆核心 Skill

把 47 页综述压缩为一个可复用的中文 Codex Skill：**只记能改善下一次决策的信息，用任务结果验证是否值得记。**

适用于 Agent 跨会话遗忘、重复犯错、记忆噪声、过期事实，以及记忆架构设计。它提供设计与执行指引，本身不会建立记忆服务、自动收集资料或改变数据库。

## 六个重点

1. 先定义记忆要改善的具体决策，区分事实与推断。
2. 区分任务内历史、跨次经验和外部知识。
3. 默认从可追溯的文本或结构化记忆开始，按瓶颈升级。
4. 联合设计写入、管理与读取，控制上下文预算。
5. 依据来源和事实有效期处理冲突，让更正传递到派生记忆。
6. 同时评估检索正确性、真实任务收益、时间和成本。

入口：[SKILL.md](skills/llm-agent-memory/SKILL.md)。字段设计与来源依据放在两份按需读取的 reference 中，避免每次调用加载长篇综述。

## 安装

将本仓库的 `skills/llm-agent-memory` 文件夹复制到 Codex 的技能目录：默认 `~/.codex/skills/`；如配置了 `CODEX_HOME`，则使用其下的 `skills/`。若同名目录已经存在，先比较差异并保留原版本，不直接覆盖。

新开一个 Codex 对话后使用：

> 用 $llm-agent-memory 分析我的业务助手为何跨会话遗忘，给出最小改进方案和验证办法。

普通问答无需调用。Skill 不依赖 DeepSeek API、外部 MCP 或额外安装包；已有项目的技术栈和来源门禁仍然适用。

## 来源与制作

- Zhang et al. (2025), *A Survey on the Memory Mechanism of Large Language Model-based Agents*, ACM TOIS 43(6), Article 155, 47 pages. [DOI](https://doi.org/10.1145/3748302)。
- 原始提炼通过 DeepSeek Harness 完成，界面模型标签为 `DeepSeek-V41-Flash / High`，生成会话自报 `deepseek-flash`；未独立核验后台精确模型版本。
- Codex 依据用户提供的 PDF 复核章节、修正绝对化表述，并整理为 Skill。制作日期：2026-10-02。
- [论文依据与工程扩展边界](skills/llm-agent-memory/references/paper-evidence.md)；[验证案例](evals/cases.md)。

仓库不包含论文 PDF、提取全文、论文图表、私人数据、运行凭证或 DeepSeek 原始会话。这里只保留概括性归纳、原创操作指引和来源引用；原文权利归原权利人。仓库尚未指定开源许可证。

验证范围与结果见 [验证记录](evals/validation.md)。结构校验和合成案例检查不等于业务系统或生产环境验收。
