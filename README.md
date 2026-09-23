# Code Workspace Extensions

Code Workspace 扩展集合。每个扩展都是一个独立 package，安装后由 Code Workspace Host 管理其 Skill、MCP、Hook 或 Runtime 输出。

## 快速开始

在已初始化的 Workspace 中安装扩展：

```bash
codew extension install <extension-id> --yes
```

需要固定版本时：

```bash
codew extension install <extension-id> --version <version> --yes
```

每个扩展的详细说明、使用边界和配置项见对应目录的 `README.md`。

## 扩展列表

| 扩展 | 用途 | 版本 | 文档 |
|---|---|---:|---|
| `codew-extension-devlopement-guide` | Code-W 扩展开发、打包、本地调试与发布交接指南 | `0.1.0` | [README](extensions/codew-extension-devlopement-guide/README.md) |
| `monitor` | 监控 Agent 会话、执行状态和授权请求 | `1.1.0` | [README](extensions/monitor/README.md) |
| `zhuiyi-guangda-coding-spec` | 中国光大银行客户项目研发规范 Skill | `1.0.0` | [README](extensions/zhuiyi-guangda-coding-spec/README.md) |
| `zhuiyi-jira-issue-fix-summary` | 生成并确认 Jira 修复总结后发布评论 | `1.0.0` | [README](extensions/zhuiyi-jira-issue-fix-summary/README.md) |
| `zhuiyi-jira-mcp` | Jira 查询、附件、关联任务和写入能力 | `1.1.0` | [README](extensions/zhuiyi-jira-mcp/README.md) |
| `zhuiyi-jira-prd-analysis` | Jira 需求范围、完整性和代码可行性分析 | `1.2.0` | [README](extensions/zhuiyi-jira-prd-analysis/README.md) |
| `zhuiyi-jira-task-breakdown` | 将已确认需求拆分为前端、后端和测试任务 | `1.0.0` | [README](extensions/zhuiyi-jira-task-breakdown/README.md) |
| `zhuiyi-opensvn-mcp` | OpenSVN 页面、目录和设计资料读取 | `0.2.0` | [README](extensions/zhuiyi-opensvn-mcp/README.md) |

## MCP 认证提示

MCP 扩展安装后不会保存真实凭证：

- Jira：在生成的 Codex `.codex/config.toml` 或 Claude `.mcp.json` 中填写 `JIRA_COOKIE`，值为浏览器请求头中的完整 Cookie，不要包含 `Cookie:` 前缀。
- OpenSVN：在同类配置中填写 `SVN_AUTHORIZATION`，值为完整的 `Authorization` 头内容，例如 `Basic ...` 或 `Bearer ...`，具体以服务端要求为准。

不要把包含 Cookie、Token、密码或 Authorization 的配置提交到 Git。完整步骤和字段说明请参阅 [Jira MCP README](extensions/zhuiyi-jira-mcp/README.md) 与 [OpenSVN MCP README](extensions/zhuiyi-opensvn-mcp/README.md)。

## 安全与卸载

扩展只在 Host 提供的 staging 中准备输出，不直接写入 Workspace。卸载示例：

```bash
codew extension uninstall <extension-id> --yes
```

卸载后的缓存和用户数据是否保留，以对应扩展 README 的说明为准。
