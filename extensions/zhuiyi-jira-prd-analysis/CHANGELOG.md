# Changelog

本文件记录 `zhuiyi-jira-prd-analysis` 扩展的版本变更。

格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)。

## [1.2.0] - 2026-09-23

### Changed

- Claude Code 命令迁移到 `codew` 命名空间目录：`.claude/commands/zhuiyi-jira-prd-analysis.md` → `.claude/commands/codew/zhuiyi-jira-prd-analysis.md`。
- Claude Code 调用方式由 `/zhuiyi-jira-prd-analysis` 改为 `/codew:zhuiyi-jira-prd-analysis`，与 `/codew:add-projects` 共用命名空间，避免与用户的顶层自定义命令重名。

## [1.1.0] - 2026-09-23

### Changed

- Codex 侧改为被动调用：`agents/openai.yaml` 设置 `allow_implicit_invocation: false`，`SKILL.md` 的 description 与正文明确「仅当用户显式调用 `$zhuiyi-jira-prd-analysis` 时启动，不要隐式调用」，避免被普通 Jira 或开发请求自动触发。
- Claude 侧由 Skill 改为 slash command：不再输出 `.claude/skills/zhuiyi-jira-prd-analysis/SKILL.md`，改为输出 `.claude/commands/zhuiyi-jira-prd-analysis.md`。

## [1.0.0] - 2026-09-19

### Added

- 初始版本。
- Codex Skill：`.codex/skills/zhuiyi-jira-prd-analysis/SKILL.md` 及 `agents/openai.yaml`。
- Claude Skill：`.claude/skills/zhuiyi-jira-prd-analysis/SKILL.md`。
- 分析流程：先确认 Jira 父任务/子任务范围，再检查需求清晰度、一致性和验收条件，最后结合所选代码库评估可行性；每个歧义或冲突交由用户决策。
- 依赖 `zhuiyi-jira-mcp` 读取 Jira 资料，设计稿位于 OpenSVN 时使用 `zhuiyi-opensvn-mcp`；缺少 MCP 或鉴权失败时停止并说明缺口，不编造内容。
