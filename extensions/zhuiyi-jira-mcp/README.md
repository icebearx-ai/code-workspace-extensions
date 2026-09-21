# Zhuiyi Jira MCP

为 Codex 和 Claude 提供 Jira MCP 服务，用于读取 Jira 需求、评论、附件、关联任务和子任务，并在获得明确授权时创建评论或任务。

## 安装

在已初始化的 Workspace 中执行：

```bash
codew extension install zhuiyi-jira-mcp
```

安装时会生成：

- Codex：`.codex/config.toml` 中的 `[mcp_servers.zhuiyi-jira]` 配置；
- Claude：`.mcp.json` 中的 `mcpServers.zhuiyi-jira` 配置；
- Workspace 根目录的 `.mcp-cache-jira/` 缓存目录（由 `.gitignore` 贡献规则忽略）。

扩展会按固定版本下载并校验预构建的 MCP runtime；安装过程不会运行 `npm install` 或项目生命周期脚本。

## 配置 Jira 认证

扩展默认使用 Cookie 认证，安装后配置中会有一个空占位符：

```toml
[mcp_servers.zhuiyi-jira.env]
JIRA_AUTH_TYPE = "cookie"
JIRA_COOKIE = ""
```

按以下步骤填写：

1. 在浏览器中登录公司 Jira。
2. 打开浏览器开发者工具的 Network 面板，访问任意 Jira 页面或接口请求。
3. 从请求头复制 `Cookie` 的完整值（通常形如 `JSESSIONID=...; atlassian.xsrf.token=...`）。
4. 将 Cookie 值填入对应配置中的 `JIRA_COOKIE`，只填值本身，不要加 `Cookie:` 前缀。

Codex 示例：

```toml
JIRA_AUTH_TYPE = "cookie"
JIRA_COOKIE = "JSESSIONID=your-session; atlassian.xsrf.token=your-xsrf-token"
```

Claude 示例：

```json
{
  "mcpServers": {
    "zhuiyi-jira": {
      "env": {
        "JIRA_AUTH_TYPE": "cookie",
        "JIRA_COOKIE": "JSESSIONID=your-session; atlassian.xsrf.token=your-xsrf-token"
      }
    }
  }
}
```

两种工具只需配置实际使用的那一份文件。Cookie 过期、账号退出或权限变化后，重新复制新的 Cookie 并覆盖旧值即可。不要把包含真实 Cookie 的配置提交到 Git、粘贴到工单或发送到聊天中；建议将本地配置文件设置为仅当前用户可读。

## 常用配置

| 变量 | 默认值 | 说明 |
|---|---|---|
| `JIRA_API_VERSION` | `latest` | Jira API 版本选择 |
| `JIRA_ALLOWED_HOSTS` | `jira.in.wezhuiyi.com` | 允许访问的 Jira 主机，通常无需修改 |
| `JIRA_CUSTOM_FIELD_MAPPING` | 已预置 | 自定义字段到语义字段的 JSON 映射 |
| `JIRA_SUBTASK_ISSUE_TYPE_ID` | `10202` | 创建子任务时使用的 issue type id |
| `JIRA_CACHE_DIR` | `.mcp-cache-jira` | 需求和附件缓存目录 |
| `JIRA_CACHE_TTL` | `15m` | 缓存有效期 |
| `JIRA_CACHE_MAX_SNAPSHOTS` | `10` | 最大缓存快照数 |
| `JIRA_CACHE_STALE_IF_ERROR` | `true` | Jira 暂时不可用时是否允许使用旧缓存 |
| `JIRA_REQUEST_TIMEOUT` | `30s` | 单次请求超时 |

除非 Jira 管理员给出明确要求，不要修改 `JIRA_ALLOWED_HOSTS`，也不要把认证信息写入扩展源码、`release.json` 或 Skill 文档。

## 卸载

```bash
codew extension uninstall zhuiyi-jira-mcp --yes
```

卸载会移除 MCP 配置和扩展运行目录，但会保留 `.mcp-cache-jira/` 用户缓存；如需删除缓存，请确认其中没有需要保留的资料后手动删除。
