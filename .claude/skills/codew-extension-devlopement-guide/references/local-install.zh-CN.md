# 本地扩展 tarball 安装协议

本文记录已经确认、但可能尚未合入公开开发文档的 `extension install-local` 目标协议。实现进度以当前 CLI 的 `codew extension install-local --help` 和实际行为为准。

## 目标命令

```bash
codew extension install-local ./dist/codew-ext-my-extension-0.1.0.tgz --yes
```

该命令用于把一个已由 `extension pack` 生成的本地 tarball 安装到一个已初始化的 Workspace，以便在不访问 Registry 的情况下进行本地调试。

## 协议范围

- 只接受单个 `.tgz` 文件。
- 使用现有 tarball 校验逻辑。
- 使用现有安全解压逻辑。
- 校验通过后导入 Extension Store。
- 复用现有 Workspace 安装事务。
- 本地来源记录为 `source: "local"`。
- 相同版本、相同 digest 时幂等跳过。
- 相同版本、不同 digest 时拒绝覆盖。
- 第一版不支持 `--replace`。
- 不访问 Registry。
- 不支持源码目录安装。
- `extension uninstall` 继续复用现有卸载命令。

## 本地开发闭环

```text
extension init
  |
  v
开发扩展
  |
  v
extension pack
  |
  v
extension install-local
  |
  v
本地测试
  |
  v
extension uninstall
```

建议流程：

1. 使用 `codew extension init` 初始化并开发扩展。
2. 使用 `codew extension pack` 生成 `.tgz`。
3. 选择一个已初始化的测试 Workspace。
4. 执行本地安装并验证扩展输出和生命周期。
5. 完成后执行 `codew extension uninstall <extension-id> --yes`。

## 非 tarball 输入

不支持目录安装。传入目录等非 `.tgz` 输入时，应返回明确错误，例如：

```text
EXTENSION_LOCAL_ARCHIVE_INVALID
Local extension installation accepts only a .tgz tarball.
```

## 版本与 digest

- 相同版本、相同 digest：幂等跳过，不重复导入。
- 相同版本、不同 digest：拒绝覆盖。
- 第一版不支持 `--replace`。

修改一个已经导入的版本后，应提升版本再打包；不要假设卸载 Workspace activation 会删除 Store 中已有的同版本不同 digest 内容，也不要伪造覆盖能力。

## 实现状态

`install-local` 正在实现，公开开发文档可能暂时没有此章节。执行前先检查当前 CLI 是否支持该命令；如果尚不支持，应明确报告能力未就绪，不要用 Registry 安装代替本地 tarball 安装，也不要声称已经完成本地安装验证。
