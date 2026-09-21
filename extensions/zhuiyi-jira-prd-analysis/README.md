# Zhuiyi Jira PRD Analysis

用于分析内部 Jira 需求：先确认父任务/子任务范围，再检查需求的完整性、一致性和验收条件，最后结合用户指定的代码库评估实现可行性。

## 作用

- 读取 Jira 正文、评论、附件、关联任务和设计资料。
- 识别范围、权限、状态、异常、验收标准和跨端依赖中的歧义。
- 在用户确认分析范围和需求口径后，输出有证据的代码可行性结论。

## 安装与使用

```bash
codew extension install zhuiyi-jira-prd-analysis
```

安装后：

- Codex 使用 `.codex/skills/zhuiyi-jira-prd-analysis/SKILL.md`；
- Claude 使用 `.claude/skills/zhuiyi-jira-prd-analysis/SKILL.md`。

使用时显式调用 `zhuiyi-jira-prd-analysis`，并提供 Jira 链接或明确的需求范围。分析依赖 `zhuiyi-jira-mcp` 获取 Jira 资料，设计稿位于 OpenSVN 时还需要 `zhuiyi-opensvn-mcp`。鉴权失败或资料不足时，Skill 会停止并说明缺少的资料，不会根据标题或猜测补写需求。

## 

```bash
codew extension uninstall zhuiyi-jira-prd-analysis --yes
```
