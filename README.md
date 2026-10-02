# 智能体记忆设计 · Codex 插件

版本 **0.1.0**。把 47 页综述压缩为一个可安装的中文 Codex 技能插件：**只记能改善下一次决策的信息，用任务结果验证是否值得记。**

适用于 Agent 跨会话遗忘、重复犯错、记忆噪声、过期事实，以及记忆架构设计。它提供设计与执行指引，本身不会建立记忆服务、自动收集资料或改变数据库。

## 六个重点

1. 先定义记忆要改善的具体决策，区分事实与推断。
2. 区分任务内历史、跨次经验和外部知识。
3. 默认从可追溯的文本或结构化记忆开始，按瓶颈升级。
4. 联合设计写入、管理与读取，控制上下文预算。
5. 依据来源和事实有效期处理冲突，让更正传递到派生记忆。
6. 同时评估检索正确性、真实任务收益、时间和成本。

入口：[SKILL.md](skills/llm-agent-memory/SKILL.md)。字段设计与来源依据放在两份按需读取的 reference 中，避免每次调用加载长篇综述。

## 安装插件

使用支持插件命令的 Codex CLI，并确保当前 Git 环境能访问这个私有仓库。以下命令语法已通过本机 `codex-cli 0.144.3` 帮助核对：

```sh
codex plugin marketplace add yikuiyuan3-create/llm-agent-memory-skill --ref v0.1.0
codex plugin add llm-agent-memory@llm-agent-memory-marketplace
```

检查安装状态：

```sh
codex plugin list --marketplace llm-agent-memory-marketplace --json
```

也可从 [v0.1.0 下载页](https://github.com/yikuiyuan3-create/llm-agent-memory-skill/releases/tag/v0.1.0) 下载 ZIP。解压后在含有 `.agents`、`.codex-plugin` 和 `skills` 的插件根目录执行：

```sh
codex plugin marketplace add .
codex plugin add llm-agent-memory@llm-agent-memory-marketplace
```

本仓库提供独立市场名，安装命令会注册该来源并安装插件。未提交到公共插件目录。独立 Skill 版仍保留在 `skills/llm-agent-memory`；已使用旧版的用户可继续使用，按需选择一种安装方式以避免同名入口重复。

新开一个 Codex 对话后使用：

> 用 $llm-agent-memory 分析我的业务助手为何跨会话遗忘，给出最小改进方案和验证办法。

插件内技能的完整名称为 `llm-agent-memory:llm-agent-memory`，也可在技能选择器中选择“智能体记忆设计”。

普通问答无需调用。运行使用当前 Codex 模型，不依赖 DeepSeek API、外部 MCP 或额外安装包；已有项目的技术栈和来源门禁仍然适用。插件清单中的能力标签描述适用能力，不授予额外权限。

## 包结构

```text
.agents/plugins/marketplace.json  安装来源与插件目录
.codex-plugin/plugin.json         插件名称、版本与界面信息
skills/llm-agent-memory/          Skill、界面元数据及来源依据
evals/                          合成案例与验证范围
```

插件没有启动钩子、常驻进程、MCP 服务或凭证配置。具体文件操作仍由用户任务和现有权限控制。

## 来源与制作

- Zhang et al. (2025), *A Survey on the Memory Mechanism of Large Language Model-based Agents*, ACM TOIS 43(6), Article 155, 47 pages. [DOI](https://doi.org/10.1145/3748302)。
- 原始提炼通过 DeepSeek Harness 完成，界面模型标签为 `DeepSeek-V41-Flash / High`，生成会话自报 `deepseek-flash`；未独立核验后台精确模型版本。
- Codex 依据用户提供的 PDF 复核章节、修正绝对化表述，并整理为 Skill。制作日期：2026-10-02。
- [论文依据与工程扩展边界](skills/llm-agent-memory/references/paper-evidence.md)；[验证案例](evals/cases.md)。

仓库不包含论文 PDF、提取全文、论文图表、私人数据、运行凭证或 DeepSeek 原始会话。这里只保留概括性归纳、原创操作指引和来源引用；原文权利归原权利人。仓库尚未指定开源许可证。

验证范围与结果见 [验证记录](evals/validation.md)。结构校验和合成案例检查不等于业务系统或生产环境验收。
