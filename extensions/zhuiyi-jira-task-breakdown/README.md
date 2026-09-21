# Zhuiyi Jira Task Breakdown

用于将已经确认范围和口径的 Jira 需求拆分为可执行的【前端】【后端】【测试】任务，并在用户确认任务内容、维度、粒度和工作量后创建 Jira 任务。

## 作用

- 先确认父任务/子任务范围和拆分维度。
- 按可独立验收的交付物拆分任务，工作量只使用 `2h`、`4h` 或 `1d`。
- 创建前展示固定格式的任务表，避免未经确认产生 Jira 任务。

## 安装与使用

```bash
codew extension install zhuiyi-jira-task-breakdown
```

安装后：

- Codex 使用 `.codex/skills/zhuiyi-jira-task-breakdown/SKILL.md`；
- Claude 使用 `.claude/skills/zhuiyi-jira-task-breakdown/SKILL.md`。

仅在需求范围已经确认后显式调用该 Skill。它依赖 `zhuiyi-jira-mcp` 读取 Jira 内容，并在用户明确批准前不会创建任务。

## 卸载

```bash
codew extension uninstall zhuiyi-jira-task-breakdown --yes
```
