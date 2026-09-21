---
name: codew-extension-devlopement-guide
description: "仅当用户显式调用 $codew-extension-devlopement-guide 时，引导 Code-W 扩展需求澄清、名称和路径确认、初始化、开发、打包、本地安装调试及发布交接。不要隐式调用本 Skill。"
---

# Code-W 扩展开发指南

仅在用户显式调用后启动。将需求推进为经过验证的扩展包，并在打包后交接本地调试和发布上线。

## 责任边界

AI 负责：澄清需求，执行初始化，开发扩展，运行开发校验，在开发者明确同意后打包，并提供本地调试和发布指引。

开发者负责：确认需求、扩展名称和代码路径，审核产物，使用自己的账号上传/发布扩展，并决定上线、版本和后续运营事项。

不可越过以下边界：

- 不执行 `npm login`、`npm publish`、`npm deprecate` 或任何 Registry 写操作。
- 不读取、保存、输出或传递 Registry Token、Cookie、密码等凭证。
- `extension pack` 只在本地产出并验证 tarball，不等于发布，也不得触发发布。

## 参考文件路由

只在对应阶段读取所需文件，不要预加载全部内容：

- 初始化、开发和打包：`references/extension-development-guide.zh-CN.md`
- manifest：`references/canonical/spec/extension/v1/specification.zh-CN.md`、`references/canonical/schemas/extension-manifest-v3.json`
- `init.js`：`references/canonical/schemas/extension-init-context-v1.json`、`references/canonical/schemas/extension-init-result-v1.json`
- 打包运输包：`references/canonical/schemas/extension-npm-transport-envelope-v1.json`
- 本地安装调试：`references/local-install.zh-CN.md`
- 发布上线：`references/nexus-publish.zh-CN.md`
- 快照来源与版本核对：`references/canonical/SOURCE.md`

如果在 `code-workspace` 源码仓库内工作，当前仓库中的 spec、schema、CLI 实现和 `--help` 是事实来源。发现快照或文档与当前实现冲突时，停止有副作用的操作，说明差异并优先遵循当前仓库；不要猜测参数或绕过校验。

## 与开发者交互

- 用户已经提供的信息不重复询问；能从上下文合理推断的信息先给出候选，再批量确认。
- 每轮只询问真正阻塞开发的 1–3 个问题；不要连续追问非关键细节。
- 技术设计由 AI 先给出可执行方案；只有会改变用户行为、安全边界或兼容性的歧义才需要开发者决定。
- 非阻塞项使用合理默认值，例如版本建议为 `0.1.0`，最后一次性展示给开发者确认或修正。
- 不要要求开发者先理解 manifest、ownership、schema 等实现细节。

## 开发前门禁

开始写文件或执行 `extension init` 前，必须明确以下三项。缺失或含糊时才提问；已有信息足够时直接整理成方案请开发者确认。

### 1. 需求

复述扩展要解决的问题、核心用户行为和明确不做的内容。开发者无需预先拆分 output；AI 负责把目标转换成扩展设计。

如果存在会显著改变实现方向的产品歧义，只询问必要问题。非阻塞的技术选择由 AI 决定并在方案中说明。

### 2. 扩展名称

如果开发者已提供 `extension-id` 或名称，直接采用并检查 ID 是否符合 `^[a-z0-9]+(?:-[a-z0-9]+)*$`。

如果未提供，根据需求提议 `extension-id` 和显示名称；必须展示候选值并取得确认，不能静默定名。

### 3. 代码路径

默认候选为当前目录，但必须向开发者展示解析后的绝对路径并取得确认。目标目录已有内容时，先检查并说明初始化将新增或修改什么。

### 开始确认

信息齐全后，用一条紧凑消息提出最终方案，不要逐项盘问：

```text
需求：<核心行为与边界>
扩展：<extension-id> / <显示名称>
版本：<建议版本，默认 0.1.0>
代码路径：<absolute-path>
实现方案：<目标工具、主要 output 及必要技术选择>

请确认，或直接指出需要修改的项；确认后开始初始化。
```

开发者确认后开始，不得把沉默或模糊回复视为确认。

## 执行流程

### 1. 初始化

读取开发指南并检查当前 CLI 的 `codew extension init --help`，确认参数后再使用已确认的路径和元数据初始化。初始化成功只代表包骨架存在，不代表功能已实现。

### 2. 开发

按开发指南和对应 schema 实现最小、内聚的能力。遵守 staging、manifest、工具过滤和摘要合同；每次修改 `init.js` 或 `extension/` 后更新 digest，并运行适用的语法、合同和测试校验。

AI 负责处理实现细节。只有发现需求冲突或需要开发者作出产品决策时，才返回询问。

### 3. 开发完成门禁

基础校验通过后停止自动打包，并明确询问：

```text
扩展开发已完成。现在要执行打包，还是继续修改？
```

开发者选择继续修改时，完成修改后再次执行本门禁；只有明确选择打包才能进入打包阶段。

### 4. 打包

1. 更新 digest，确保源码、入口摘要和 package 摘要一致。
2. 使用当前 CLI 打包，例如：

```bash
codew extension pack <confirmed-path> --output ./dist --json
```

3. 验证命令成功、tarball 身份和版本正确，并保留 `tarball.path`、`tarball.integrity`、各摘要和文件清单。
4. 打包失败时修复并重新打包，不交付未验证的 tarball。

### 5. 打包后的交接

打包成功后，必须同时提供以下两类信息。最终上传和发布始终由开发者执行。

#### A. 本地安装调试

读取 `references/local-install.zh-CN.md`，让开发者选择一个已初始化的测试 workspace，然后提供完整的本地闭环：

```text
install-local -> 本地测试 -> extension uninstall
```

至少给出与本次 tarball 对应的 `codew extension install-local <path-to-tarball.tgz> --yes` 和卸载命令，并说明 `install-local` 的当前实现状态及关键限制。不要把扩展默认安装到真实业务 Workspace；只有开发者明确指定测试 Workspace 并要求执行时才运行安装或卸载。

#### B. 发布上线

读取 `references/nexus-publish.zh-CN.md`，根据本次 tarball 给出发布步骤和验收清单，明确登录、上传、远端校验及发布后的生命周期要求。

AI 可以生成命令和 checklist，但不能代为登录、上传或修改 Registry。开发者要求 AI 上传时，说明责任边界并提供其可自行执行的命令。

## 完成交付

用简短状态说明当前阶段：

```text
开发状态：<已完成/仍需修改>
扩展：<extension-id>@<version>
代码路径：<absolute-path>
校验：<已通过/未执行项>
打包：<未打包/已打包/失败>
下一步：<本地调试或由开发者执行发布>
```

不要声称已经完成未实际执行的上传、发布、上线或验证。
