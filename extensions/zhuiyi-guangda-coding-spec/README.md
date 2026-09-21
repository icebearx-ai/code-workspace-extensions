# Zhuiyi Guangda Coding Spec

面向中国光大银行客户项目的研发规范 Skill。它把研发、安全、测试、投产和关键正确性要求整理成可按需加载的参考资料，帮助 Agent 在设计、编码、审查和发布准备阶段进行风险检查。

## 适用范围

仅在当前项目明确属于中国光大银行客户项目时使用。客户归属不明确时，应先确认，不要仅因技术栈或目录名称自动套用本规范。

## 安装与使用

```bash
codew extension install zhuiyi-guangda-coding-spec
```

安装后：

- Codex 使用 `.codex/skills/zhuiyi-guangda-coding-spec/SKILL.md`；
- Claude 使用 `.claude/skills/zhuiyi-guangda-coding-spec/SKILL.md`。

需要使用时显式调用 `zhuiyi-guangda-coding-spec` Skill。Skill 会根据任务风险域按需读取 `references/`，不会默认加载全部规范资料。

## 卸载

```bash
codew extension uninstall zhuiyi-guangda-coding-spec --yes
```

## 注意事项

该扩展只提供规范和参考文档，不代替 DBA、安全、运维、业务或管理人员的正式审批，也不会自动修改项目代码。
