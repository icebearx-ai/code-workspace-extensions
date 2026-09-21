# Code Workspace Monitor

监控 Code Workspace 中的 Agent 会话、执行状态、工具调用和待授权请求，并提供本机网页面板查看实时状态。

## 作用

- 接收 Codex 和 Claude 的任务、写入、回合结束等事件。
- 在 Agent 请求授权或任务完成时显示提醒。
- 保留监控事件历史，并通过本机 HTTP 服务提供快照和事件流。
- 监控服务默认只监听 `127.0.0.1:3211`，不会把事件主动发送到外网。

## 安装与使用

在已初始化的 Workspace 中执行：

```bash
codew extension install monitor --version 1.1.0 --yes
```

安装后 Host 会启动全局 monitor runtime，并自动接入已选择的 Codex/Claude Hook。配置文件为：

```text
.codew/config-monitor.yaml
```

首次生成的配置类似：

```yaml
schemaVersion: 1
enable: true
url: "http://127.0.0.1:3211"
```

需要暂停事件上报时，将 `enable` 改为 `false`；需要调整服务地址时，同时修改 `url`。该配置属于 seeded 文件，安装后用户的修改会被保留。

服务启动后可访问：

```text
http://127.0.0.1:3211/
```

健康检查地址为 `http://127.0.0.1:3211/api/v1/health`。如端口被占用，请先停止占用进程，再重新激活扩展。

## 卸载

```bash
codew extension uninstall monitor --yes
```

卸载会移除由扩展管理的 Hook 和运行服务，不会删除监控运行数据或用户自行修改的配置内容。
