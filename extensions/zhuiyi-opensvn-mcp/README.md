# Zhuiyi OpenSVN MCP

为 Codex 和 Claude 提供 OpenSVN MCP 服务，用于读取允许范围内的 SVN 页面、目录和设计资料。它适合与 Jira 需求分析 Skill 配合使用。

## 安装

```bash
codew extension install zhuiyi-opensvn-mcp
```

安装时会生成：

- Codex：`.codex/config.toml` 中的 `[mcp_servers.zhuiyi-opensvn]` 配置；
- Claude：`.mcp.json` 中的 `mcpServers.zhuiyi-opensvn` 配置；
- `.gitignore` 中忽略 `.mcp-cache-opensvn/` 运行目录。

扩展会按固定版本下载、校验并解压预构建的 MCP runtime；不会运行 `npm install` 或项目生命周期脚本。

## 配置认证信息

配置模板默认包含空的授权占位符：

```toml
[mcp_servers.zhuiyi-opensvn.env]
SVN_AUTHORIZATION = ""
```

`SVN_AUTHORIZATION` 应填写完整的 HTTP `Authorization` 请求头值，从浏览器的 Devtool 的 network 中，找一个请求，在 header 中找到即可，不要填写 `Authorization:` 字段名。具体格式以 OpenSVN 服务端分配的认证方式为准，常见形式如下：

```toml
# Basic 认证示例（账号密码的 Base64 编码）
SVN_AUTHORIZATION = "Basic <base64(username:password)>"

# Bearer Token 示例
# SVN_AUTHORIZATION = "Bearer <token>"
```

Claude 配置使用同名 JSON 环境变量：

```json
{
  "mcpServers": {
    "zhuiyi-opensvn": {
      "env": {
        "SVN_AUTHORIZATION": "Bearer <token>"
      }
    }
  }
}
```

请向 OpenSVN 管理员确认应使用 Basic、Bearer 或其他认证方案，不要自行猜测。认证信息应直接填写在实际使用的 Codex 或 Claude 配置中，不要写入扩展源码、`release.json`、README 或 Git 仓库；也不要把真实 Token 发到聊天或提交记录中。

## 常用配置

| 变量 | 默认值 | 说明 |
|---|---|---|
| `SVN_ALLOWED_HOSTS` | `opssvn.in.wezhuiyi.com` | 允许访问的 SVN 主机，通常无需修改 |
| `SVN_OUTPUT_DIR` | `.mcp-cache-opensvn` | 运行期输出和缓存目录 |
| `SVN_MAX_RESOURCES` | `200` | 单次返回的最大资源数量 |

只有在管理员明确提供其他主机或资源限制时才修改这些值。若认证失败，先检查 Token/密码是否过期、Authorization 前缀是否正确，以及当前账号是否有目标 SVN 路径权限。

## 卸载

```bash
codew extension uninstall zhuiyi-opensvn-mcp --yes
```

卸载会移除 MCP 配置和扩展运行目录，但保留 `.mcp-cache-opensvn/` 用户运行数据；确认不再需要后可手动清理。
