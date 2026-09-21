# Code-W Extension Development Guide

将 Code-W 扩展从需求澄清推进到可验证安装包，并在打包后交接本地调试和发布上线。该扩展封装了 `codew-extension-devlopement-guide` Skill、开发规范快照和常用参考文档。

## 作用

- 在开发前确认需求、扩展名称、版本和代码路径。
- 按 Extension Spec v1 引导 manifest、输出、staging 和 init result 的实现。
- 在开发完成后提供校验、打包、本地安装调试和发布检查清单。
- 明确 AI 不代替开发者执行登录、上传、发布或 Registry 写操作。

## 安装与使用

```bash
codew extension install codew-extension-devlopement-guide
```

安装后：

- Codex 使用 `.codex/skills/codew-extension-devlopement-guide/SKILL.md`；
- Claude 使用 `.claude/skills/codew-extension-devlopement-guide/SKILL.md`。

该 Skill 默认禁止隐式调用，仅应在开发者显式调用 `$codew-extension-devlopement-guide` 时启动。

## 卸载

```bash
codew extension uninstall codew-extension-devlopement-guide --yes
```

扩展只安装 Skill 和参考文档，不执行发布，不访问网络，也不保存凭证。
