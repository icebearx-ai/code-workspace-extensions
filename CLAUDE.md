## Code Workspace 扩展开发约定

（本约定需要配合 `codew-extension-devlopement-guide` 使用。)

- 扩展源码放在 `./extensions/<extension-id>/` 下。
- 扩展打包产物输出到 `./dist/extensions/`。
- 除非用户明确要求覆盖默认路径，否则不要在其他目录初始化扩展或输出打包产物。
