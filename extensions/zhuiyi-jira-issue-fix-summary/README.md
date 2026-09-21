# Zhuiyi Jira Issue Fix Summary

用于在问题修复完成后，从当前 Agent 会话整理“根因 / 修改内容”总结，并在用户分别确认总结文本和 Jira 工单 URL 后，通过 `zhuiyi-jira-mcp` 发布 Jira 评论。

## 作用

- 汇总已证实的问题现象、根因、实际修改和验证结果。
- 在发布前展示完整评论草稿，避免未经确认写入 Jira。
- 只有总结文本和目标工单 URL 都明确确认后，才执行外部写入。

## 安装与使用

```bash
codew extension install zhuiyi-jira-issue-fix-summary
```

安装后：

- Codex 使用 `.codex/skills/zhuiyi-jira-issue-fix-summary/SKILL.md`；
- Claude 使用 `.claude/skills/zhuiyi-jira-issue-fix-summary/SKILL.md`。

仅在用户明确调用该 Skill 时使用，例如：

```text
$zhuiyi-jira-issue-fix-summary
```

使用前请确保 `zhuiyi-jira-mcp` 已安装并完成认证。该 Skill 不会猜测项目、提交或 Jira 目标，也不会在用户确认前创建评论。

## 卸载

```bash
codew extension uninstall zhuiyi-jira-issue-fix-summary --yes
```
