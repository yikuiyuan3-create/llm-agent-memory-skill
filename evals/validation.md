# 验证记录

日期：2026-10-02。

已完成：

- 用 Codex `skill-creator/scripts/quick_validate.py` 验证技能命名、frontmatter 和占位符，返回 `Skill is valid!`。
- 解析 `agents/openai.yaml`，检查显示描述长度与默认提示中的技能名；核对仓库内相对链接。
- 核对引用的原文章节与 PDF 页码；确认用户源 PDF 的 SHA-256 未改变。
- 检查上传文件列表以及凭证模式、本机绝对路径等明显泄露项；未把论文原文、临时资料或个人文件加入仓库。这是限定范围检查，不是全面安全审计。
- DeepSeek 根据初版 Skill 完成六个合成场景的纸面决策演练。它正确拒绝了无必要的持久化、越权片段命令和无证据的收益承诺；其余回答暴露的三个误读点已用于修订 Skill：实时事实必须回源、单次事件不能泛化成事实、逻辑失效不能代替数据删除。
- 修订后再次运行技能结构校验。三个修订点经编写者检查，未作为独立模型测试通过记录。

边界：纸面演练不等于运行一个记忆系统。本次没有真实业务样本、生产读写、数据库删除、性能基准或线上收益测试。新对话中的自动发现与目标业务场景效果仍需实际验证。未设定通用提升幅度、检索权重、保留期限或升级阈值。

复验方式：安装 Skill 后，以 `cases.md` 中的合成请求观察输出，再用获准的脱敏样本在同模型同任务下对照基线；只报告实际观察到的结果。

## 插件 v0.1.0 打包验证

同日将现有 Skill 包装为 Codex 插件，Skill 本体及两个 reference 的内容未改动。

- 依据本机已安装插件的清单格式及 `codex-cli 0.144.3` 帮助构建 `.codex-plugin/plugin.json` 和 `.agents/plugins/marketplace.json`。
- 使用该 CLI 自带的 app-server `plugin/read`，以本仓库的 marketplace 文件路径作只读识别，成功读出插件 `llm-agent-memory@llm-agent-memory-marketplace`、本地版本 `0.1.0`、中文界面信息及技能 `llm-agent-memory:llm-agent-memory`。
- 解析器返回 `availability=AVAILABLE`；技能数量为 1，hooks、apps、MCP servers 均为空。返回的 `installed=false` 表示该检查没有安装插件，不能将可识别解释为已安装。
- 插件与 marketplace 名称、版本和路径一致；再次通过 Skill 格式校验、相对链接检查及 `git diff --check`。
- ZIP 包含上述隐藏目录，随包提供 SHA-256 校验值。安装说明采用已核验的 CLI 命令。

本次验证到插件解析与打包层；未执行个人环境的插件安装或新会话验收。先前独立安装的 Skill 与插件安装状态分开记录。
